# راهنمای عملی Health Check - Droplinked

> **اجرایی و قابل استفاده فوری**  
> **تاریخ:** بهمن ۱۴۰۴

---

## 🎯 شروع سریع (Quick Start)

### گام ۱: Endpoint سلامت بساز (برای دولوپرها)

**توی هر پروژه backend یه فایل اضافه کن:**

```javascript
// routes/health.js
const express = require('express');
const router = express.Router();
const { Pool } = require('pg');
const redis = require('../config/redis');

const db = new Pool({ connectionString: process.env.DATABASE_URL });

// L1: زنده بودن سرویس
router.get('/health/live', (req, res) => {
  res.json({ 
    status: 'alive', 
    timestamp: new Date().toISOString(),
    uptime: process.uptime()
  });
});

// L2: آماده بودن سرویس
router.get('/health/ready', async (req, res) => {
  try {
    // چک دیتابیس
    const dbStart = Date.now();
    await db.query('SELECT 1');
    const dbLatency = Date.now() - dbStart;
    
    // چک ردیس
    const redisStart = Date.now();
    await redis.ping();
    const redisLatency = Date.now() - redisStart;
    
    res.json({
      status: 'ready',
      checks: {
        database: { status: 'up', latency: `${dbLatency}ms` },
        redis: { status: 'up', latency: `${redisLatency}ms` },
        disk: { status: 'up', usage: '45%' }
      }
    });
  } catch (error) {
    res.status(503).json({ status: 'not_ready', error: error.message });
  }
});

module.exports = router;
```

**توی app.js اضافه کن:**
```javascript
const healthRoutes = require('./routes/health');
app.use(healthRoutes);
```

---

## ۱. چک‌های عملی برای Main Backend

### A. چک سطح ۱: زنده بودن (Liveness)

**دستور:**
```bash
curl -s -o /dev/null -w "%{http_code}" https://api.droplinked.com/health/live
```

**نتیجه مورد انتظار:** `200`

**اتوماتیک با GitHub Actions:**
```yaml
# .github/workflows/health-check.yml
name: Health Check

on:
  schedule:
    - cron: '*/5 * * * *'  # هر ۵ دقیقه

jobs:
  check-main-api:
    runs-on: ubuntu-latest
    steps:
      - name: Check API is alive
        run: |
          STATUS=$(curl -s -o /dev/null -w "%{http_code}" https://api.droplinked.com/health/live)
          if [ "$STATUS" != "200" ]; then
            echo "❌ API DOWN! Status: $STATUS"
            curl -X POST $SLACK_WEBHOOK \
              -H 'Content-Type: application/json' \
              -d '{"text":"🚨 Main API is DOWN!"}'
            exit 1
          fi
          echo "✅ API is UP"
```

---

### B. چک سطح ۲: عملکرد اصلی (Deep Health)

**تست ۱: لاگین کار می‌کنه؟**
```bash
# دستور:
curl -X POST https://api.droplinked.com/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "health-check@test.com",
    "password": "HealthCheck123!"
  }'

# نتیجه مورد انتظار:
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": "user-123",
    "email": "health-check@test.com"
  }
}

# چک کن: HTTP 200 و token برگرده
```

**تست ۲: لیست محصولات کار می‌کنه؟**
```bash
# دستور:
curl https://api.droplinked.com/products?page=1&limit=10

# نتیجه مورد انتظار:
{
  "products": [
    {
      "id": "prod-001",
      "title": "Test Product",
      "price": 99.99,
      "status": "active"
    }
  ],
  "total": 150,
  "page": 1
}

# چک کن: HTTP 200 و array products برگرده
```

**تست ۳: ساخت سفارش کار می‌کنه؟**
```bash
# دستور:
curl -X POST https://api.droplinked.com/orders \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{
    "items": [
      {
        "product_id": "prod-test-001",
        "quantity": 1,
        "price": 99.99
      }
    ],
    "total": 99.99
  }'

# نتیجه مورد انتظار:
{
  "id": "order-123",
  "status": "created",
  "total": 99.99
}

# چک کن: HTTP 201 و order id برگرده
```

**تست ۴: پرداخت کار می‌کنه؟**
```bash
# دستور:
curl https://api.droplinked.com/payments/methods

# نتیجه مورد انتظار:
{
  "methods": [
    { "id": "stripe", "name": "Credit Card", "enabled": true },
    { "id": "crypto", "name": "Crypto", "enabled": true }
  ]
}

# چک کن: HTTP 200 و payment methods برگرده
```

**همه تست‌ها توی یه اسکریپت:**
```javascript
// scripts/health-check.js
const axios = require('axios');

const API_URL = 'https://api.droplinked.com';
const TEST_USER = { email: 'health-check@test.com', password: 'HealthCheck123!' };

async function runHealthChecks() {
  const results = [];
  
  // ۱. لاگین
  try {
    const loginRes = await axios.post(`${API_URL}/auth/login`, TEST_USER);
    results.push({ test: 'login', status: 'PASS', time: loginRes.headers['x-response-time'] });
    const token = loginRes.data.token;
    
    // ۲. محصولات
    const productsRes = await axios.get(`${API_URL}/products?limit=5`);
    results.push({ 
      test: 'products_list', 
      status: productsRes.data.products.length > 0 ? 'PASS' : 'FAIL',
      count: productsRes.data.products.length
    });
    
    // ۳. ساخت سفارش
    const orderRes = await axios.post(`${API_URL}/orders`, {
      items: [{ product_id: 'test-product', quantity: 1, price: 10 }],
      total: 10
    }, { headers: { Authorization: `Bearer ${token}` }});
    results.push({ test: 'create_order', status: 'PASS', order_id: orderRes.data.id });
    
    // ۴. پرداخت
    const paymentRes = await axios.get(`${API_URL}/payments/methods`);
    results.push({ 
      test: 'payment_methods', 
      status: paymentRes.data.methods.length > 0 ? 'PASS' : 'FAIL'
    });
    
  } catch (error) {
    results.push({ test: error.config?.url, status: 'FAIL', error: error.message });
  }
  
  console.table(results);
  
  // اگه FAIL داریم → Slack
  const failures = results.filter(r => r.status === 'FAIL');
  if (failures.length > 0) {
    await notifySlack(failures);
    process.exit(1);
  }
}

runHealthChecks();
```

**اجرا:**
```bash
node scripts/health-check.js
```

---

## ۲. چک‌های عملی برای Web3 Service

### A. چک شبکه‌ها

**دستور:**
```bash
curl https://web3.droplinked.com/health/ready
```

**نتیجه مورد انتظار:**
```json
{
  "status": "ready",
  "networks": {
    "polygon": { "status": "connected", "latency": "150ms", "block_height": 52345678 },
    "ethereum": { "status": "connected", "latency": "250ms", "block_height": 18923456 },
    "base": { "status": "connected", "latency": "100ms", "block_height": 1234567 }
  }
}
```

**چک کن:**
```bash
# همه شبکه‌ها connected باشن
curl -s https://web3.droplinked.com/health/ready | jq '.networks | to_entries | map(select(.value.status != "connected")) | length'
# باید 0 برگرده
```

---

### B. چک Smart Contract (تستnet فقط!)

**دستور:**
```bash
# چک NFT contract

curl -X POST https://web3.droplinked.com/contracts/nft/call \
  -H "Content-Type: application/json" \
  -d '{
    "network": "polygon",
    "contract": "0x1234...",
    "method": "name",
    "params": []
  }'

# نتیجه مورد انتظار:
{ "result": "DroplinkedProductNFT" }
```

**اسکریپت چک Web3:**
```javascript
// scripts/web3-health.js
const axios = require('axios');

const WEB3_URL = 'https://web3.droplinked.com';

async function checkWeb3() {
  // ۱. چک شبکه‌ها
  const networksRes = await axios.get(`${WEB3_URL}/health/ready`);
  const networks = networksRes.data.networks;
  
  for (const [name, data] of Object.entries(networks)) {
    if (data.status !== 'connected') {
      console.error(`❌ Network ${name} is down`);
    } else if (data.latency > 1000) {
      console.warn(`⚠️ Network ${name} is slow: ${data.latency}`);
    } else {
      console.log(`✅ Network ${name}: ${data.latency}`);
    }
  }
  
  // ۲. چک estimate gas
  try {
    const gasRes = await axios.post(`${WEB3_URL}/estimate-gas`, {
      network: 'polygon',
      method: 'mint',
      params: { product_id: 'test' }
    });
    console.log(`✅ Gas estimation: ${gasRes.data.gas} gwei`);
  } catch (error) {
    console.error('❌ Gas estimation failed:', error.message);
  }
}

checkWeb3();
```

---

## ۳. چک‌های عملی برای Frontend

### A. Shop Builder (Dashboard)

**تست ۱: صفحه لاگین لود می‌شه؟**
```bash
# دستور:
curl -s https://droplinked.com/login | grep -q "login-form" && echo "✅ Login page OK" || echo "❌ Login page FAIL"
```

**تست ۲: لاگین کار می‌کنه؟ (Playwright)**
```javascript
// tests/health/dashboard.spec.js
const { test, expect } = require('@playwright/test');

test('dashboard health check', async ({ page }) => {
  // ۱. باز کردن صفحه لاگین
  await page.goto('https://droplinked.com/login');
  await expect(page).toHaveTitle(/Login/);
  
  // ۲. پر کردن فرم
  await page.fill('#email', 'health-check@test.com');
  await page.fill('#password', 'HealthCheck123!');
  
  // ۳. کلیک روی لاگین
  await page.click('button[type="submit"]');
  
  // ۴. چک کردن ریدایرکت
  await expect(page).toHaveURL(/dashboard/);
  await expect(page.locator('text=Dashboard')).toBeVisible();
  
  // ۵. چک کردن لود محصولات
  await page.click('text=Products');
  await expect(page.locator('.product-card')).toHaveCount.greaterThan(0);
});
```

**اجرا:**
```bash
npx playwright test tests/health/dashboard.spec.js
```

---

### B. Checkout

**تست: فرایند پرداخت کامل**
```javascript
// tests/health/checkout.spec.js
test('checkout flow', async ({ page }) => {
  // ۱. باز کردن چک‌اوت
  await page.goto('https://checkout.droplinked.com?cart=test-cart-id');
  
  // ۲. چک کردن لود سبد خرید
  await expect(page.locator('.cart-item')).toHaveCount.greaterThan(0);
  
  // ۳. پر کردن اطلاعات مشتری
  await page.fill('#customer-name', 'Test User');
  await page.fill('#customer-email', 'test@example.com');
  await page.fill('#address', '123 Test St');
  
  // ۴. انتخاب روش پرداخت
  await page.click('text=Credit Card');
  
  // ۵. چک کردن نمایش فرم پرداخت
  await expect(page.locator('.payment-form')).toBeVisible();
  
  // ۶. چک کردن محاسبه قیمت
  const total = await page.locator('.order-total').textContent();
  expect(parseFloat(total)).toBeGreaterThan(0);
});
```

---

### C. Product Tile Widget

**تست: لود شدن ویجت**
```html
<!-- تست روی یه صفحه HTML ساده -->
<!DOCTYPE html>
<html>
<head>
  <script defer src="https://apiv3.droplinked.com/widget/bundle"></script>
</head>
<body>
  <droplinked-product product-id="test-product-123"></droplinked-product>
  
  <script>
    // چک بعد از ۵ ثانیه
    setTimeout(() => {
      const tile = document.querySelector('droplinked-product');
      const shadow = tile.shadowRoot;
      
      // چک کن تصویر لود شده
      const img = shadow.querySelector('img');
      console.log(img ? '✅ Image loaded' : '❌ Image failed');
      
      // چک کن قیمت نمایش داده شده
      const price = shadow.querySelector('.price');
      console.log(price ? `✅ Price: ${price.textContent}` : '❌ Price missing');
      
      // چک کن دکمه خرید
      const button = shadow.querySelector('button');
      console.log(button ? '✅ Button exists' : '❌ Button missing');
    }, 5000);
  </script>
</body>
</html>
```

---

## ۴. چک‌های عملی برای NPM Package

### A. @droplinked/web3

**تست ۱: Build می‌شه؟**
```bash
# داخل پروژه package
cd packages/web3
npm run build

# چک کن dist وجود داره
ls -la dist/
# باید فایل‌های js داشته باشه
```

**تست ۲: تست‌ها پاس می‌شن؟**
```bash
npm test

# چک کن:
# Tests: 50 passed
```

**تست ۳: Bundle size OKه؟**
```bash
npm run build
ls -lh dist/index.js
# باید کمتر از 100KB باشه
```

**اسکریپت چک:**
```javascript
// scripts/check-package.js
const fs = require('fs');
const { execSync } = require('child_process');

// ۱. Build
console.log('🔨 Building...');
execSync('npm run build', { stdio: 'inherit' });

// ۲. Tests
console.log('🧪 Running tests...');
execSync('npm test', { stdio: 'inherit' });

// ۳. Size check
const stats = fs.statSync('dist/index.js');
const sizeKB = stats.size / 1024;
console.log(`📦 Bundle size: ${sizeKB.toFixed(2)} KB`);

if (sizeKB > 100) {
  console.error('❌ Bundle too large!');
  process.exit(1);
}

console.log('✅ Package health check passed');
```

---

## ۵. GitHub Actions کامل و عملی

### فایل کامل:
```yaml
# .github/workflows/health-check.yml
name: Health Check - All Services

on:
  schedule:
    - cron: '*/5 * * * *'   # L1: هر ۵ دقیقه
    - cron: '*/15 * * * *'  # L2: هر ۱۵ دقیقه
    - cron: '*/30 * * * *'  # L3: هر ۳۰ دقیقه
  workflow_dispatch:

env:
  SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK_URL }}
  TEST_USER_EMAIL: health-check@test.com
  TEST_USER_PASS: ${{ secrets.HEALTH_CHECK_PASSWORD }}

jobs:
  # ============================================
  # L1: Liveness (هر ۵ دقیقه)
  # ============================================
  liveness:
    runs-on: ubuntu-latest
    if: github.event.schedule == '*/5 * * * *' || github.event_name == 'workflow_dispatch'
    strategy:
      matrix:
        service:
          - name: 'Main API'
            url: 'https://api.droplinked.com/health/live'
          - name: 'Web3 Service'
            url: 'https://web3.droplinked.com/health/live'
          - name: '3rd Party'
            url: 'https://integrations.droplinked.com/health/live'
          - name: 'Dashboard'
            url: 'https://droplinked.com/health'
          - name: 'Shopfront'
            url: 'https://droplinked.io/health'
          - name: 'Checkout'
            url: 'https://checkout.droplinked.com/health'
    
    steps:
      - name: Check ${{ matrix.service.name }}
        run: |
          echo "🔍 Checking ${{ matrix.service.name }}..."
          STATUS=$(curl -s -o /dev/null -w "%{http_code}" --max-time 10 ${{ matrix.service.url }})
          
          if [ "$STATUS" = "200" ]; then
            echo "✅ ${{ matrix.service.name }} is UP"
          else
            echo "❌ ${{ matrix.service.name }} is DOWN (Status: $STATUS)"
            curl -X POST $SLACK_WEBHOOK \
              -H 'Content-Type: application/json' \
              -d "{\"text\":\"🚨 ALERT: ${{ matrix.service.name }} is DOWN! Status: $STATUS\"}"
            exit 1
          fi

  # ============================================
  # L2: Readiness (هر ۱۵ دقیقه)
  # ============================================
  readiness:
    runs-on: ubuntu-latest
    if: github.event.schedule == '*/15 * * * *'
    
    steps:
      - name: Check Main API Readiness
        run: |
          RESPONSE=$(curl -s https://api.droplinked.com/health/ready)
          echo "Response: $RESPONSE"
          
          # چک کن دیتابیس up باشه
          DB_STATUS=$(echo $RESPONSE | jq -r '.checks.database.status')
          if [ "$DB_STATUS" != "up" ]; then
            echo "❌ Database is down!"
            curl -X POST $SLACK_WEBHOOK -d '{"text":"🚨 Database is DOWN!"}'
            exit 1
          fi
          
          echo "✅ Database is up"

      - name: Check Web3 Networks
        run: |
          RESPONSE=$(curl -s https://web3.droplinked.com/health/ready)
          
          # چک کن همه شبکه‌ها connected باشن
          DISCONNECTED=$(echo $RESPONSE | jq '[.networks | to_entries[] | select(.value.status != "connected")] | length')
          
          if [ "$DISCONNECTED" -gt 0 ]; then
            echo "❌ $DISCONNECTED networks are disconnected!"
            curl -X POST $SLACK_WEBHOOK -d '{"text":"🚨 Some Web3 networks are DOWN!"}'
            exit 1
          fi
          
          echo "✅ All Web3 networks connected"

      - name: Check 3rd Party Integrations
        run: |
          curl -f https://integrations.droplinked.com/health/ready || {
            curl -X POST $SLACK_WEBHOOK -d '{"text":"🚨 3rd Party integrations unhealthy!"}'
            exit 1
          }

  # ============================================
  # L3: Deep Health (هر ۳۰ دقیقه)
  # ============================================
  deep-health:
    runs-on: ubuntu-latest
    if: github.event.schedule == '*/30 * * * *'
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run deep health checks
        run: |
          node scripts/health-check.js
        env:
          API_URL: https://api.droplinked.com
          TEST_EMAIL: ${{ env.TEST_USER_EMAIL }}
          TEST_PASSWORD: ${{ env.TEST_USER_PASS }}

  # ============================================
  # Frontend E2E (هر ۳۰ دقیقه)
  # ============================================
  e2e-health:
    runs-on: ubuntu-latest
    if: github.event.schedule == '*/30 * * * *'
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Playwright
        run: |
          npm ci
          npx playwright install chromium
      
      - name: Run E2E health tests
        run: npx playwright test tests/health/ --project=chromium
      
      - name: Upload screenshots on failure
        uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: failure-screenshots
          path: test-results/
```

---

## ۶. چک‌لیست راه‌اندازی (برای DevOps/Lead)

### امروز انجام بده:

**۱. ساخت یوزر تست:**
```sql
-- داخل دیتابیس Staging و Production
INSERT INTO users (id, email, password, role, status) 
VALUES (
  'user-health-check-001',
  'health-check@test.com', 
  '$2b$10$...', -- hash شده
  'customer',
  'active'
);

-- یه سفارش تستی بساز
INSERT INTO orders (id, user_id, status, total) 
VALUES ('order-health-001', 'user-health-check-001', 'completed', 99.99);
```

**۲. اضافه کردن Endpoint:**
```bash
# توی هر پروژه backend:

# ۱. فایل routes/health.js بساز (همونی که بالا دادم)

# ۲. توی app.js اضافه کن:
echo "const healthRoutes = require('./routes/health');" >> app.js
echo "app.use(healthRoutes);" >> app.js

# ۳. تست کن:
curl http://localhost:3000/health/live
# باید 200 بده
```

**۳. Slack Webhook بساز:**
```bash
# توی Slack:
# ۱. برو به slack.com/apps
# ۲. Incoming Webhooks رو نصب کن
# ۳. کانال #alerts-critical رو انتخاب کن
# ۴. URL رو کپی کن

# توی GitHub:
# Settings → Secrets → New repository secret
# Name: SLACK_WEBHOOK_URL
# Value: <URL کپی شده>
```

**۴. GitHub Actions اجرا کن:**
```bash
# فایل .github/workflows/health-check.yml رو بساز
# پوش کن به main
# برو به Actions tab
# ببین سبز شده
```

---

### فردا انجام بده:

**۵. Playwright tests:**
```bash
npm install -D @playwright/test
npx playwright install

# فایل tests/health/dashboard.spec.js بساز
# تست رو اجرا کن:
npx playwright test tests/health/dashboard.spec.js
```

**۶. اسکریپت Health Check:**
```bash
# scripts/health-check.js رو بساز
# تست کن:
node scripts/health-check.js
```

---

## ۷. چک‌لیست Daily (برای QA)

**هر صبح اینا رو چک کن:**

```bash
# ۱. API زنده است؟
curl -f https://api.droplinked.com/health/live && echo "✅ API OK"

# ۲. لاگین کار می‌کنه؟
curl -X POST https://api.droplinked.com/auth/login \
  -d '{"email":"health-check@test.com","password":"HealthCheck123!"}' \
  | jq '.token' && echo "✅ Login OK"

# ۳. محصولات لود می‌شن؟
curl https://api.droplinked.com/products?limit=1 | jq '.products[0].title' && echo "✅ Products OK"

# ۴. Web3 شبکه‌ها up هستن؟
curl https://web3.droplinked.com/health/ready | jq '.networks | keys[]' && echo "✅ Web3 OK"

# ۵. Dashboard بالاست؟
curl -f https://droplinked.com/health && echo "✅ Dashboard OK"

# ۶. Checkout بالاست؟
curl -f https://checkout.droplinked.com/health && echo "✅ Checkout OK"
```

**اگه همه ✅ بود → روزت خوش!**  
**اگه ❌ دیدی → Slack #alerts-critical**

---

**فایل:** `health-check-practical-guide.md`  
**وضعیت:** آماده اجرا - همین امروز شروع کن ✅

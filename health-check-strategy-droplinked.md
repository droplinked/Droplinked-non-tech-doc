# استراتژی Health Check برای معماری چندپارچه Droplinked

> **معماری:** ۳ Backend + ۳ Frontend + ۲ NPM Package + ۱ Product Tile  
> **تاریخ:** بهمن ۱۴۰۴

---

## 🎯 نمای کلی معماری

### ساختار سرویس‌ها:

**Backend (۳ سرویس):**
1. **Main Backend** - Core API (Products, Orders, Users, Auth)
2. **3rd Party Service** - Integrations (Stripe, Shipping, Email)
3. **Web3 Service** - Blockchain (RPC, Smart Contracts, Wallets)

**Frontend (۳ اپلیکیشن):**
1. **Shop Builder** - Dashboard (droplinked.com)
2. **Shopfront** - Store (droplinked.io/shop)
3. **Checkout** - Payment (checkout.droplinked.com)

**Package (۲ تا):**
1. **@droplinked/web3** - Web3 utilities
2. **@droplinked/web3-ui-kit** - React components

**Widget (۱ تا):**
1. **Product Tile** - Embeddable component for external sites

---

## ۱. استراتژی کلی Health Check

### سطوح مختلف:

| سطح | نام | تناوب | چک می‌کنه | زمان |
|-----|-----|-------|-----------|------|
| **L1** | **Liveness** | هر ۱-۵ دقیقه | سرویس زنده است؟ | < ۱ ثانیه |
| **L2** | **Readiness** | هر ۵-۱۵ دقیقه | آماده پاسخگویی؟ | ۱-۵ ثانیه |
| **L3** | **Deep Health** | هر ۱۵-۳۰ دقیقه | قابلیت‌ها کار می‌کنن؟ | ۵-۳۰ ثانیه |
| **L4** | **E2E Health** | هر ۱-۲ ساعت | جریان کامل کاربر؟ | ۱-۲ دقیقه |

---

## ۲. Health Check برای هر سرویس

### A. Main Backend (Core API)

**Endpoint:** `https://api.droplinked.com/health`

#### L1: Liveness (هر ۱ دقیقه)
```http
GET /health/live

Response: { "status": "alive", "timestamp": "...", "version": "2.5.1" }
```

**چک‌ها:**
- HTTP 200؟
- Response time < 500ms؟
- Process up؟

#### L2: Readiness (هر ۵ دقیقه)
```http
GET /health/ready

Response:
{
  "status": "ready",
  "checks": {
    "database": { "status": "up", "latency": "12ms", "connections": "15/100" },
    "redis": { "status": "up", "latency": "3ms" },
    "disk": { "status": "up", "usage": "45%" }
  }
}
```

**چک‌ها:**
- Database connected؟
- Redis connected؟
- Disk < 80%؟
- Memory < 80%؟

#### L3: Deep Health (هر ۱۵ دقیقه)
```http
GET /health/deep

Response:
{
  "status": "healthy",
  "checks": {
    "auth": { "test_login": "passed", "token_gen": "passed" },
    "products": { "list": "passed", "get": "passed" },
    "orders": { "create": "passed", "get": "passed" },
    "payments": { "gateway": "connected" }
  }
}
```

**چک‌ها:**
- Login با اکانت تستی؟
- API Products کار می‌کنه؟
- API Orders کار می‌کنه؟
- Payment gateway وصله؟

---

### B. 3rd Party Integration Service

**Endpoint:** `https://integrations.droplinked.com/health`

#### L2: Readiness
```http
GET /health/ready

Response:
{
  "status": "ready",
  "integrations": {
    "stripe": { "status": "connected", "webhook": "active" },
    "shipping": { "status": "connected", "latency": "120ms" },
    "sendgrid": { "status": "connected", "queue": 15 }
  }
}
```

**چک‌های خاص:**
- Stripe webhook delivery OK؟
- Shipping API rate limit OK؟
- Email queue backed up نیست؟

#### L3: Deep Health
**تست‌های واقعی:**
- Mock payment به Stripe
- Mock shipping rate
- Mock email send

---

### C. Web3 Service

**Endpoint:** `https://web3.droplinked.com/health`

#### L2: Readiness (مهم!)
```http
GET /health/ready

Response:
{
  "status": "ready",
  "networks": {
    "polygon": { "status": "connected", "latency": "150ms", "block": 52345678 },
    "ethereum": { "status": "connected", "latency": "250ms", "block": 18923456 },
    "base": { "status": "connected", "latency": "100ms", "block": 1234567 }
  },
  "contracts": {
    "nft_polygon": "deployed",
    "payment_eth": "deployed"
  }
}
```

**چک‌ها:**
- همه شبکه‌ها connected؟
- RPC latency < ۱ ثانیه؟
- Smart contracts callable؟

#### L3: Deep Health
**چک‌ها (فقط testnet!):**
- Call smart contract (read-only)
- Estimate gas
- Get wallet balance
- Get NFT metadata

---

### D. Frontend Applications

#### Shop Builder (Dashboard)

**L1:**
```bash
curl -f https://droplinked.com/health
# چک: HTTP 200
```

**L2 (Playwright):**
```javascript
// Synthetic test
await page.goto('https://droplinked.com/login');
await page.fill('#email', 'health@test.com');
await page.fill('#password', 'testpass');
await page.click('#login-btn');
await page.waitForSelector('[data-testid="dashboard"]');
```

**چک‌ها:**
- Login page لود می‌شه؟
- Login کار می‌کنه؟
- Dashboard لود می‌شه؟
- Products list لود می‌شه؟

#### Shopfront

**L1:**
- `https://droplinked.io/test-shop` باز می‌شه؟
- Product list لود می‌شه؟

**L2:**
- Add to cart کار می‌کنه؟
- Cart overlay باز می‌شه؟
- Product detail لود می‌شه؟

**L3:**
- Claim NFT flow (testnet)
- Currency switch
- Currency conversion درسته؟

#### Checkout

**L1:**
- `https://checkout.droplinked.com` بالاست؟

**L2:**
- Cart display کار می‌کنه؟
- Payment methods لود می‌شن؟
- Address form کار می‌کنه؟

**L3:**
- Commission calculation درسته؟
- Tax calculation درسته؟
- Shipping calculation درسته؟
- Order creation کار می‌کنه؟

#### Product Tile Widget

**L1:**
- Script bundle لود می‌شه؟
- `https://apiv3.droplinked.com/widget/bundle`

**L2:**
- Component render می‌شه؟
- Product data fetch می‌شه؟
- Variants نمایش داده می‌شن؟

**L3:**
- Redirect mode کار می‌کنه؟
- Modal mode کار می‌کنه؟
- Out of stock state درسته؟

---

### E. NPM Packages

#### @droplinked/web3

**چک‌ها:**
- Build بدون error؟
- Tests پاس می‌شن؟
- Bundle size < threshold؟

**GitHub Actions:**
```yaml
- name: Test Package
  run: |
    npm run build
    npm test
    npm run test:bundle-size
```

#### @droplinked/web3-ui-kit

**چک‌ها:**
- Storybook build می‌شه؟
- همه components render می‌شن؟
- Tests پاس می‌شن؟
- Accessibility tests پاس می‌شن؟

---

## ۳. پیاده‌سازی Aggregator

### GitHub Actions Workflow:

```yaml
name: Health Check - All Services

on:
  schedule:
    - cron: '*/5 * * * *'   # L1: هر ۵ دقیقه
    - cron: '*/15 * * * *'  # L2: هر ۱۵ دقیقه
    - cron: '*/30 * * * *'  # L3: هر ۳۰ دقیقه
    - cron: '0 * * * *'     # L4: هر ۱ ساعت

jobs:
  liveness:
    runs-on: ubuntu-latest
    if: github.event.schedule == '*/5 * * * *'
    strategy:
      matrix:
        service:
          - { name: 'Main API', url: 'https://api.droplinked.com/health/live' }
          - { name: '3rd Party', url: 'https://integrations.droplinked.com/health/live' }
          - { name: 'Web3', url: 'https://web3.droplinked.com/health/live' }
          - { name: 'Dashboard', url: 'https://droplinked.com/health' }
          - { name: 'Shopfront', url: 'https://droplinked.io/health' }
          - { name: 'Checkout', url: 'https://checkout.droplinked.com/health' }
    steps:
      - run: |
          STATUS=$(curl -s -o /dev/null -w "%{http_code}" ${{ matrix.service.url }})
          if [ $STATUS -ne 200 ]; then
            echo "❌ ${{ matrix.service.name }} DOWN"
            curl -X POST $SLACK_WEBHOOK -d '{"text":"🚨 ${{ matrix.service.name }} DOWN"}'
            exit 1
          fi
          echo "✅ ${{ matrix.service.name }} UP"

  readiness:
    runs-on: ubuntu-latest
    if: github.event.schedule == '*/15 * * * *'
    steps:
      - run: curl -f https://api.droplinked.com/health/ready
      - run: curl -f https://web3.droplinked.com/health/ready
      - run: curl -f https://integrations.droplinked.com/health/ready

  deep-health:
    runs-on: ubuntu-latest
    if: github.event.schedule == '*/30 * * * *'
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run test:health:deep
        env:
          API_URL: https://api.droplinked.com
          WEB3_URL: https://web3.droplinked.com

  e2e-health:
    runs-on: ubuntu-latest
    if: github.event.schedule == '0 * * * *'
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npx playwright install
      - run: npm run test:e2e:health
```

---

## ۴. Dashboard مانیتورینگ

### Widgetهای پیشنهادی:

**۱. Service Status Grid:**
```
Dashboard    Shopfront    Checkout
  🟢 99.9%    🟢 99.8%    🟢 99.9%

Main API     3rd Party    Web3
  🟢 99.9%    🟡 98.5%    🟢 99.9%
```

**۲. Web3 Networks:**
```
Polygon:    🟢 150ms
Ethereum:   🟢 250ms
Base:       🟢 100ms
```

**۳. Integrations:**
```
Stripe:     🟢 Webhook OK
SendGrid:   🟢 Queue: 15
Shipping:   🟡 Latency: 800ms
```

**۴. Business Metrics:**
```
Orders/min:     45 🟢
Payment Success: 98.5% 🟢
Failed Logins:   12 🔴
```

---

## ۵. سیستم Alerting

### سطوح Alert:

#### P0 - Critical (فوری)
**چی:** Main API down, Checkout down, Database down  
**کی:** ۱ failure از ۲ check  
**به کی:** On-call engineer + PM + #alerts-critical  
**اکشن:** Auto-rollback

#### P1 - High (۵ دقیقه)
**چی:** Web3 networks down, 3rd party failing  
**کی:** ۲ failures از ۳ check  
**به کی:** Backend team + #alerts-high  
**اکشن:** Manual intervention

#### P2 - Medium (۱۵ دقیقه)
**چی:** High latency, elevated errors  
**کی:** ۳ failures از ۵ check  
**به کی:** #alerts-medium  
**اکشن:** Investigate

#### P3 - Low (۱ ساعت)
**چی:** Minor issues  
**کی:** ۵ failures از ۱۰ check  
**به کی:** #alerts-low  
**اکشن:** Next sprint

### Slack Alert Example:
```json
{
  "attachments": [{
    "color": "danger",
    "title": "🚨 P0: Main API Down",
    "fields": [
      {"title": "Service", "value": "Main Backend", "short": true},
      {"title": "Status", "value": "HTTP 503", "short": true},
      {"title": "Time", "value": "10:30 UTC", "short": true},
      {"title": "Impact", "value": "All frontends affected", "short": true}
    ],
    "actions": [
      {"name": "dashboard", "text": "View Dashboard", "type": "button"},
      {"name": "runbook", "text": "Runbook", "type": "button"},
      {"name": "rollback", "text": "Rollback", "type": "button", "style": "danger"}
    ]
  }]
}
```

---

## ۶. چک‌لیست پیاده‌سازی

### هفته ۱: Setup
- [ ] Endpoint /health/live به همه backendها اضافه کن
- [ ] Endpoint /health/ready به همه backendها اضافه کن
- [ ] GitHub Actions workflow برای L1 بساز
- [ ] Slack webhook تنظیم کن

### هفته ۲: Deep Health
- [ ] Test user بساز (health@test.com)
- [ ] Deep health tests بنویس
- [ ] GitHub Actions برای L2 و L3 بساز
- [ ] Playwright tests برای frontend setup کن

### هفته ۳: Monitoring
- [ ] Grafana یا Datadog setup کن
- [ ] Dashboards بساز
- [ ] Alert rules تعریف کن
- [ ] Runbooks بنویس

### هفته ۴: NPM Packages
- [ ] Health checks برای packages بساز
- [ ] Bundle size monitoring setup کن
- [ ] Dependency vulnerability scanning

---

**فایل:** `health-check-strategy-droplinked.md`  
**وضعیت:** آماده اجرا ✅

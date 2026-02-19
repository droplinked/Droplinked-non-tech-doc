# مسیرهای تستی UptimeRobot برای Droplinked

> **URLها و مسیرهای دقیق برای مانیتورینگ**  
> **تاریخ:** بهمن ۱۴۰۴

---

## 🎯 فهرست URLهای مورد نیاز

### سطح ۱: حیاتی (باید حتماً مانیتور بشن)

| سرویس | URL | نوع | Interval |
|-------|-----|-----|----------|
| **Main API** | `https://api.droplinked.com/health/live` | HTTP | ۵ دقیقه |
| **Dashboard** | `https://droplinked.com/health` | HTTP | ۵ دقیقه |
| **Checkout** | `https://checkout.droplinked.com/health` | HTTP | ۵ دقیقه |
| **Web3 Service** | `https://web3.droplinked.com/health/live` | HTTP | ۵ دقیقه |

---

## 🏪 شاپ تستی (Test Shops)

### ۱. شاپ اصلی تست

**بساز:**
```
نام: test-store-droplinked
URL: https://droplinked.io/test-store-droplinked

محصولات:
- Test Product 1 (قیمت: $10)
- Test Product 2 (قیمت: $25)
- Test Product Out of Stock (موجودی: 0)
```

**مانیتور:**
```
URL: https://droplinked.io/test-store-droplinked
Type: HTTP
Interval: 5 minutes
Keyword: "Test Product"  ← باید این کلمه باشه
```

---

### ۲. شاپ Web3 Test

**بساز:**
```
نام: test-web3-store
URL: https://droplinked.io/test-web3-store

محصولات:
- NFT Product (On-chain: true)
- Regular Product (On-chain: false)
```

**مانیتور:**
```
URL: https://droplinked.io/test-web3-store
Type: HTTP
Interval: 10 minutes
```

---

### ۳. شاپ Checkout Test

**بساز:**
```
نام: test-checkout-store
URL: https://droplinked.io/test-checkout-store

محصولات:
- Simple Product ($5)
- Product with Variants (Size: S, M, L)
- Digital Product
```

**مانیتور:**
```
URL: https://droplinked.io/test-checkout-store
Type: HTTP
Interval: 10 minutes
```

---

## 🧪 مسیرهای UI برای تست

### مسیر ۱: خرید کامل (Happy Path)

**مراحل تست:**
```
۱. ورود به شاپ: https://droplinked.io/test-store-droplinked
   ← چک: صفحه لود می‌شه؟

۲. کلیک روی محصول: https://droplinked.io/test-store-droplinked/product/test-product-1
   ← چک: Product Detail Page باز می‌شه؟

۳. Add to Cart:
   ← چک: محصول به سبد اضافه می‌شه؟

۴. رفتن به Checkout: https://checkout.droplinked.com?shop=test-store-droplinked
   ← چک: سبد خرید نمایش داده می‌شه؟

۵. پر کردن اطلاعات:
   ← چک: فرم اطلاعات مشتری کار می‌کنه؟

۶. انتخاب روش پرداخت:
   ← چک: لیست پرداخت نمایش داده می‌شه؟
```

**مانیتور هر مرحله:**
```
Monitor 1: Shop Home
URL: https://droplinked.io/test-store-droplinked
Keyword: "Test Product"

Monitor 2: Product Page
URL: https://droplinked.io/test-store-droplinked/product/test-product-1
Keyword: "Add to Cart"

Monitor 3: Checkout
URL: https://checkout.droplinked.com?shop=test-store-droplinked
Keyword: "Your Cart"
```

---

### مسیر ۲: Web3 و NFT

**مراحل تست:**
```
۱. شاپ Web3: https://droplinked.io/test-web3-store
   ← چک: محصولات NFT نمایش داده می‌شن؟

۲. محصول NFT: https://droplinked.io/test-web3-store/product/nft-product
   ← چک: "Verified on Blockchain" نمایش داده می‌شه؟

۳. Claim NFT: https://droplinked.io/test-web3-store/claim/nft-token-id
   ← چک: فرم Claim نمایش داده می‌شه؟
```

**مانیتور:**
```
Monitor: Web3 Shop
URL: https://droplinked.io/test-web3-store
Keyword: "NFT"

Monitor: Claim Page
URL: https://droplinked.io/test-web3-store/claim/test-token
Keyword: "Claim"
```

---

### مسیر ۳: داشبورد مرچنت

**مراحل تست:**
```
۱. لاگین: https://droplinked.com/login
   ← چک: فرم لاگین نمایش داده می‌شه؟

۲. داشبورد: https://droplinked.com/dashboard
   ← چک: داشبورد لود می‌شه؟

۳. لیست محصولات: https://droplinked.com/dashboard/products
   ← چک: لیست محصولات لود می‌شه؟

۴. ساخت محصول: https://droplinked.com/dashboard/products/new
   ← چک: فرم ساخت محصول باز می‌شه؟

۵. Onchain Inventory: https://droplinked.com/dashboard/onchain
   ← چک: صفحه Onchain لود می‌شه؟
```

**مانیتور:**
```
Monitor: Login Page
URL: https://droplinked.com/login
Keyword: "Sign In"

Monitor: Dashboard
URL: https://droplinked.com/dashboard
Keyword: "Dashboard"

Monitor: Products List
URL: https://droplinked.com/dashboard/products
Keyword: "Products"
```

---

## 📡 API Endpoints برای تست

### چک‌های ساده (GET)

```bash
# ۱. سلامت API
curl https://api.droplinked.com/health/live

# ۲. لیست محصولات
curl https://api.droplinked.com/products?limit=1

# ۳. اطلاعات یه محصول
curl https://api.droplinked.com/products/test-product-id

# ۴. اطلاعات شاپ
curl https://api.droplinked.com/shops/test-store-droplinked

# ۵. روش‌های پرداخت
curl https://api.droplinked.com/payments/methods
```

**مانیتور:**
```
Monitor: API Health
URL: https://api.droplinked.com/health/live

Monitor: Products API
URL: https://api.droplinked.com/products?limit=1
Keyword: "products"
```

---

### چک‌های پیچیده (POST)

**تست Login:**
```bash
curl -X POST https://api.droplinked.com/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "uptime-test@droplinked.com",
    "password": "TestPass123!"
  }'
```

**تست ساخت سفارش:**
```bash
curl -X POST https://api.droplinked.com/orders \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TEST_TOKEN" \
  -d '{
    "items": [
      {
        "product_id": "test-product-1",
        "quantity": 1,
        "price": 10.00
      }
    ],
    "total": 10.00,
    "shop_id": "test-store-droplinked"
  }'
```

**نکته:** اینا رو توی UptimeRobot می‌تونی با "Advanced Settings" ست کنی.

---

## 🔍 تست Product Tile Widget

### تست لود شدن اسکریپت

```bash
# چک کن اسکریپت bundle لود می‌شه
curl -I https://apiv3.droplinked.com/widget/bundle

# باید HTTP 200 برگرده
```

**مانیتور:**
```
Monitor: Widget Bundle
URL: https://apiv3.droplinked.com/widget/bundle
```

---

### تست رندر ویجت

**یه صفحه HTML تستی بساز:**
```html
<!DOCTYPE html>
<html>
<head>
  <title>Widget Test</title>
  <script defer src="https://apiv3.droplinked.com/widget/bundle"></script>
</head>
<body>
  <h1>Product Tile Test</h1>
  <droplinked-product 
    product-id="test-product-1" 
    shop-slug="test-store-droplinked">
  </droplinked-product>
</body>
</html>
```

**بذرش روی:** `https://test-widgets.droplinked.com`

**مانیتور:**
```
Monitor: Widget Test Page
URL: https://test-widgets.droplinked.com
Keyword: "product-tile-loaded"  ← این ID رو به div بده
```

---

## 📊 Web3 و Blockchain

### تست شبکه‌ها

```bash
# چک کن RPCهای هر شبکه کار می‌کنن

# Polygon
curl -X POST https://polygon-rpc.com \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'

# Ethereum
curl -X POST https://eth.llamarpc.com \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'

# Base
curl -X POST https://base.llamarpc.com \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

**مانیتور:**
```
Monitor: Web3 Service
URL: https://web3.droplinked.com/health/ready
Keyword: "polygon"
```

---

## ⚡ تست سرعت (Response Time)

### API Response Time

```
Monitor: API Speed Test
URL: https://api.droplinked.com/products?limit=1
Interval: 5 minutes
Alert if: Response time > 2000ms (2 seconds)
```

### Frontend Load Time

```
Monitor: Dashboard Speed
URL: https://droplinked.com/dashboard
Interval: 10 minutes
Alert if: Response time > 3000ms (3 seconds)
```

---

## 🎯 چک‌لیست کامل UptimeRobot

### لیست مانیتورهایی که باید بسازی:

**سطح ۱ - حیاتی (بساز اول):**
- [ ] Main Backend API - https://api.droplinked.com/health/live
- [ ] Dashboard - https://droplinked.com/health
- [ ] Checkout - https://checkout.droplinked.com/health
- [ ] Web3 Service - https://web3.droplinked.com/health/live

**سطح ۲ - شاپ‌ها (بساز دوم):**
- [ ] Test Store - https://droplinked.io/test-store-droplinked
- [ ] Web3 Test Store - https://droplinked.io/test-web3-store
- [ ] Checkout Test - https://droplinked.io/test-checkout-store

**سطح ۳ - مسیرهای UI (بساز سوم):**
- [ ] Login Page - https://droplinked.com/login (Keyword: "Sign In")
- [ ] Dashboard Page - https://droplinked.com/dashboard (Keyword: "Dashboard")
- [ ] Products Page - https://droplinked.com/dashboard/products (Keyword: "Products")
- [ ] Shop Home - https://droplinked.io/test-store-droplinked (Keyword: "Test Product")
- [ ] Product Detail - https://droplinked.io/test-store-droplinked/product/test-product-1 (Keyword: "Add to Cart")
- [ ] Checkout Page - https://checkout.droplinked.com?shop=test-store-droplinked (Keyword: "Your Cart")

**سطح ۴ - API ها (بساز چهارم):**
- [ ] Products API - https://api.droplinked.com/products?limit=1 (Keyword: "products")
- [ ] Login API - https://api.droplinked.com/auth/login (POST)
- [ ] Widget Bundle - https://apiv3.droplinked.com/widget/bundle

**سطح ۵ - Web3 (بساز پنجم):**
- [ ] Web3 Shop - https://droplinked.io/test-web3-store (Keyword: "NFT")
- [ ] Claim Page - https://droplinked.io/test-web3-store/claim/test-token (Keyword: "Claim")

---

## 🔧 تنظیمات Alert

### کی Alert بده؟

```
برای همه مانیتورها:

Alert When:
☑️ Monitor is Down for: 2 minutes
☑️ Monitor is Back Up
☑️ Response Time > 2000ms for 2 consecutive times

Notify via:
☑️ Email: dev-team@droplinked.com
☑️ Slack: #alerts-critical
☐ SMS: (اگه Pro داری)
```

---

## 📱 تست دستی (قبل از مانیتور)

### چک کن این URLها کار می‌کنن:

```bash
# تست ۱: API زنده است؟
curl -s -o /dev/null -w "%{http_code}" https://api.droplinked.com/health/live
# Expected: 200

# تست ۲: Dashboard بالاست؟
curl -s -o /dev/null -w "%{http_code}" https://droplinked.com/health
# Expected: 200

# تست ۳: شاپ تستی بالاست؟
curl -s https://droplinked.io/test-store-droplinked | grep -q "Test Product" && echo "✅ OK" || echo "❌ FAIL"

# تست ۴: Checkout بالاست؟
curl -s -o /dev/null -w "%{http_code}" https://checkout.droplinked.com/health
# Expected: 200

# تست ۵: Web3 Service بالاست؟
curl -s https://web3.droplinked.com/health/live | grep -q "alive" && echo "✅ OK" || echo "❌ FAIL"
```

**اگه همه ✅ بودن → برو UptimeRobot بساز مانیتور**  
**اگه ❌ داشتی → اول مشکل رو حل کن**

---

## 💡 نکات مهم

### ۱. Keyword چی بذارم؟

**برای صفحات HTML:**
- Login Page: `Sign In` یا `Login`
- Dashboard: `Dashboard` یا `Overview`
- Shop: نام یه محصول (مثل `Test Product`)
- Checkout: `Cart` یا `Checkout`

**برای API:**
- Products: `"products"` (فیلد JSON)
- Health: `"status"` یا `"alive"`

### ۲. Interval چقدر باشه؟

| نوع | Interval | دلیل |
|-----|----------|------|
| API Critical | ۵ دقیقه | مهم و حساس |
| Frontend | ۵ دقیقه | کاربر می‌بینه |
| Test Shops | ۱۰ دقیقه | کمتر مهم |
| Dashboard | ۵ دقیقه | مرچنت استفاده می‌کنه |

### ۳. Retry چقدر؟

```
Advanced Settings:
☑️ Retry: 2 times before alerting
☑️ Retry Interval: 30 seconds

یعنی: اگه ۲ بار پشت هم fail شد، alert بده
```

---

**فایل:** `uptimerobot-urls-droplinked.md`  
**وضعیت:** URLها آماده - شروع کن به ساخت مانیتور ✅

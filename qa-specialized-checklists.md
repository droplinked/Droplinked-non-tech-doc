# راهنمای عملی QA - چک‌لیست‌های تخصصی

> **برای تیم Droplinked - چالش‌های خاص**  
> **تاریخ:** بهمن ۱۴۰۴

---

## ۱. چک‌لیست Database Migration (تغییر دیتابیس)

### 🎯 کی به این نیاز داریم؟

وقتی:
- فیلد جدید به جدول اضافه می‌شه
- ولیدیشن‌های ساخت پروداکت تغییر می‌کنه
- فیچر Subscription بازنویسی می‌شه
- نوع داده فیلد عوض می‌شه
- جدول جدید ساخته می‌شه

---

### 📋 چک‌لیست کامل QA بعد از Migration

#### مرحله ۱: قبل از Migration (تحویل از دولوپر)

**دولوپر باید به QA بده:**

```markdown
## Migration Document

**نام:** add_subscription_status_to_users  
**تاریخ:** 2026-02-18  
**دولوپر:** @ali-dev  

### تغییرات:
- اضافه کردن ستون `subscription_status` به جدول `users`
- مقادیر مجاز: 'free', 'basic', 'premium', 'enterprise'
- default value: 'free'

### نوع Migration:
☑️ Backward Compatible (کد قدیمی با دیتابیس جدید کار می‌کنه)
☐ Breaking Change (نیاز به deploy همزمان کد و دیتابیس)

### زمان تست:
- تعداد رکورد تستی: 10,000
- زمان اجرا: 3 ثانیه
- downtime: ندارد

### رollback:
- دستور: `npm run migrate:down`
- زمان: 1 ثانیه

### داده‌های حساس:
☐ نیاز به mask دارد  
☑️ نیازی نیست

### تست‌های انجام شده توسط دولوپر:
- [x] تست محلی پاس شد
- [x] Rollback تست شد
- [x] داده‌های قدیمی سالم هستن
```

---

#### مرحله ۲: چک‌لیست QA بعد از Migration روی Staging

**الف) چک کردن Structure (ساختار):**

```sql
-- ۱. چک کن ستون اضافه شده
DESCRIBE users;

-- باید ببینی:
-- subscription_status | varchar(20) | YES | free | 
```

- [ ] ستون با اسم درست اضافه شده؟
- [ ] نوع داده درسته؟ (varchar/int/timestamp)
- [ ] default value تنظیم شده؟
- [ ] nullable هست یا required؟
- [ ] index اضافه شده؟ (اگه قرار بوده)

**ب) چک کردن داده‌های قدیمی (مهم!):**

```sql
-- ۲. چک کن داده‌های قدیمی سالم هستن
SELECT COUNT(*) FROM users;
-- باید تعداد رکوردها تغییر نکرده باشه

-- ۳. چک کن default value برای رکوردهای قدیمی ست شده
SELECT subscription_status, COUNT(*) 
FROM users 
GROUP BY subscription_status;
-- باید: 
-- free | 10000
-- (همه قدیمی‌ها free شدن)

-- ۴. چک کن رکوردهای قدیمی هنوز قابل خوندن هستن
SELECT id, email, subscription_status 
FROM users 
WHERE created_at < '2026-02-18'
LIMIT 5;
-- باید همه فیلدها پر باشن
```

**چک‌لیست Data Integrity:**
- [ ] تعداد کل رکوردها تغییر نکرده
- [ ] هیچ رکوردی null نداره (اگه required بود)
- [ ] داده‌های قدیمی قابل خوندن هستن
- [ ] داده‌های جدید ذخیره می‌شن
- [ ] foreign keyها سالم هستن
- [ ] هیچ رکوردی duplicate نشده

**ج) چک کردن Functionality (عملکرد):**

**برای مثال Subscription:**
- [ ] کاربر جدید ثبت‌نام می‌کنه → subscription_status = 'free'
- [ ] کاربر قدیمی لاگین می‌کنه → خطا نمی‌ده
- [ ] upgrade subscription کار می‌کنه؟
- [ ] downgrade subscription کار می‌کنه؟
- [ ] cancel subscription کار می‌کنه؟
- [ ] APIهای مربوط به subscription پاسخ درست می‌دن؟

**برای مثال Product Validation تغییر کرده:**
- [ ] پروداکت قدیمی (با داده‌های قدیمی) هنوز editable هست؟
- [ ] پروداکت جدید با ولیدیشن جدید ساخته می‌شه؟
- [ ] پروداکت قدیمی update می‌شه → ولیدیشن جدید اعمال می‌شه یا نه؟
- [ ] error messageها درست نمایش داده می‌شن؟

**د) چک کردن API:**

```bash
# ۵. APIهای مربوطه رو تست کن

# GET users - باید فیلد جدید رو برگردونه
curl https://api.staging.droplinked.com/users/123
# Expected: {"id": 123, "email": "...", "subscription_status": "free", ...}

# POST users - باید فیلد جدید رو قبول کنه
curl -X POST https://api.staging.droplinked.com/users \
  -H "Content-Type: application/json" \
  -d '{"email": "test@test.com", "subscription_status": "premium"}'

# PUT users - باید update کار کنه
curl -X PUT https://api.staging.droplinked.com/users/123 \
  -H "Content-Type: application/json" \
  -d '{"subscription_status": "enterprise"}'
```

- [ ] GET API فیلد جدید رو برمی‌گردونه؟
- [ ] POST API فیلد جدید رو ذخیره می‌کنه؟
- [ ] PUT/PATCH API update کار می‌کنه؟
- [ ] validation روی API کار می‌کنه؟
- [ ] APIهای قدیمی هنوز کار می‌کنن؟ (backward compatibility)

**ه) چک کردن Frontend:**
- [ ] فرم create/edit فیلد جدید رو نشون می‌ده؟
- [ ] validation روی فرم کار می‌کنه؟
- [ ] داده‌های قدیمی نمایش داده می‌شن؟
- [ ] UI برای مقادیر مختلف درسته؟ (badge/color برای subscription)
- [ ] filter/sort کار می‌کنه؟

---

#### مرحله ۳: چک‌های خاص برای داده‌های حساس

**اگه فیلد حساس اضافه شده (مثل credit card, password):**

- [ ] داده encrypted ذخیره می‌شه؟
- [ ] توی لاگ‌ها نمی‌افته؟
- [ ] توی API response نیست؟
- [ ] توی error message نیست؟
- [ ] توی database backup رمزنگاری شده؟

---

#### مرحله ۴: تست Rollback

**بعد از اینکه همه چی OK شد:**

```bash
# تست rollback
npm run migrate:down

# چک کن:
# ۱. ستون حذف شده؟
DESCRIBE users;
# subscription_status نباید باشه

# ۲. داده‌های دیگه سالم هستن؟
SELECT COUNT(*) FROM users;
# باید همون تعداد باشه

# ۳. APIها هنوز کار می‌کنن؟ (با کد قدیمی)
curl https://api.staging.droplinked.com/users/123
# باید 200 بده (بدون فیلد جدید)
```

- [ ] Rollback موفق بود؟
- [ ] داده‌های دیگه سالم هستن؟
- [ ] APIها با کد قدیمی کار می‌کنن؟
- [ ] Frontend با کد قدیمی کار می‌کنه؟

**بعدش دوباره migrate کن:**
```bash
npm run migrate:latest
```

---

### 🔄 مرحله ۵: تست Refactor کامل فیچر (مثال: Subscription)

**وقتی یه فیچر کلاً بازنویسی می‌شه (مثل Subscription):**

#### 📊 الف) تحلیل داده قبل از Migration

**دولوپر باید این اطلاعات رو بده:**

```markdown
## Pre-Migration Data Analysis
**فیچر:** Subscription Refactor  
**تاریخ:** 2026-02-18  
**دولوپر:** @ali-dev  

### آمار داده‌های فعلی (Production):

**جدول subscriptions:**
```sql
-- تعداد کل سابسکریپشن‌ها
SELECT COUNT(*) FROM subscriptions;
-- نتیجه: 50,000

-- بر اساس پلن
SELECT plan_type, COUNT(*) as count, 
       COUNT(CASE WHEN status = 'active' THEN 1 END) as active,
       COUNT(CASE WHEN status = 'expired' THEN 1 END) as expired
FROM subscriptions 
GROUP BY plan_type;
```

**نتایج:**
| Plan Type | Total | Active | Expired |
|-----------|-------|--------|---------|
| starter | 20,000 | 15,000 | 5,000 |
| pro | 15,000 | 10,000 | 5,000 |
| enterprise | 5,000 | 4,000 | 1,000 |
| trial | 10,000 | 8,000 | 2,000 |

**بر اساس تاریخ:**
```sql
-- سابسکریپشن‌هایی که امروز منقضی می‌شن
SELECT COUNT(*) FROM subscriptions 
WHERE end_date = CURRENT_DATE;
-- نتیجه: 150

-- سابسکریپشن‌هایی که در ۷ روز آینده منقضی می‌شن
SELECT COUNT(*) FROM subscriptions 
WHERE end_date BETWEEN CURRENT_DATE AND CURRENT_DATE + 7;
-- نتیجه: 1,200

-- سابسکریپشن‌های منقضی شده ولی هنوز active نشون داده می‌شن (bug?)
SELECT COUNT(*) FROM subscriptions 
WHERE status = 'active' AND end_date < CURRENT_DATE;
-- نتیجه: 50 (⚠️ اینا باید fix بشن!)
```

**بر اساس payment:**
```sql
-- auto-renew فعال
SELECT COUNT(*) FROM subscriptions WHERE auto_renew = true;
-- نتیجه: 35,000

-- payment_failed
SELECT COUNT(*) FROM subscriptions WHERE payment_status = 'failed';
-- نتیجه: 200

-- grace period
SELECT COUNT(*) FROM subscriptions 
WHERE status = 'grace_period';
-- نتیجه: 80
```

**داده‌های پرت (Edge Cases):**
```sql
-- سابسکریپشن بدون user_id
SELECT COUNT(*) FROM subscriptions WHERE user_id IS NULL;
-- نتیجه: 5 (⚠️ باید بررسی شن!)

-- end_date قبل از start_date
SELECT COUNT(*) FROM subscriptions 
WHERE end_date < start_date;
-- نتیجه: 0 ✅

-- duplicate active subscription برای یه user
SELECT user_id, COUNT(*) as count
FROM subscriptions 
WHERE status = 'active'
GROUP BY user_id 
HAVING COUNT(*) > 1;
-- نتیجه: 12 user دارن duplicate (⚠️)
```
```

---

#### 📝 ب) تهیه داده‌های تستی نمونه

**دولوپر باید این داده‌ها رو توی Staging بسازه:**

```sql
-- اسکریپت ساخت داده‌های تستی برای Subscription Refactor

-- ۱. کاربر تستی بساز
INSERT INTO users (id, email, created_at) VALUES 
('test-user-001', 'test1@droplinked.com', NOW()),
('test-user-002', 'test2@droplinked.com', NOW()),
('test-user-003', 'test3@droplinked.com', NOW()),
('test-user-004', 'test4@droplinked.com', NOW()),
('test-user-005', 'test5@droplinked.com', NOW());

-- ۲. سابسکریپشن‌های مختلف بساز

-- سابسکریپشن Active Pro
INSERT INTO subscriptions (id, user_id, plan_type, status, start_date, end_date, auto_renew, payment_status)
VALUES ('sub-001', 'test-user-001', 'pro', 'active', '2026-01-01', '2026-12-31', true, 'paid');

-- سابسکریپشن Active Starter
INSERT INTO subscriptions (id, user_id, plan_type, status, start_date, end_date, auto_renew, payment_status)
VALUES ('sub-002', 'test-user-002', 'starter', 'active', '2026-01-01', '2026-06-30', false, 'paid');

-- سابسکریپشن Expired
INSERT INTO subscriptions (id, user_id, plan_type, status, start_date, end_date, auto_renew, payment_status)
VALUES ('sub-003', 'test-user-003', 'pro', 'expired', '2025-01-01', '2025-12-31', false, 'unpaid');

-- سابسکریپشن امروز منقضی می‌شه
INSERT INTO subscriptions (id, user_id, plan_type, status, start_date, end_date, auto_renew, payment_status)
VALUES ('sub-004', 'test-user-004', 'enterprise', 'active', '2025-02-18', '2026-02-18', true, 'paid');

-- سابسکریپشن Grace Period
INSERT INTO subscriptions (id, user_id, plan_type, status, start_date, end_date, grace_period_end, auto_renew, payment_status)
VALUES ('sub-005', 'test-user-005', 'pro', 'grace_period', '2026-01-01', '2026-02-01', '2026-02-15', false, 'failed');

-- ۳. پرداخت‌های تاریخچه بساز
INSERT INTO subscription_payments (id, subscription_id, amount, status, paid_at)
VALUES 
('pay-001', 'sub-001', 99.00, 'success', '2026-01-01'),
('pay-002', 'sub-001', 99.00, 'success', '2026-02-01'),
('pay-003', 'sub-002', 29.00, 'success', '2026-01-01'),
('pay-004', 'sub-005', 99.00, 'failed', '2026-02-01');

-- ۴. تغییرات پلن (Plan Changes)
INSERT INTO subscription_changes (id, subscription_id, from_plan, to_plan, changed_at)
VALUES ('change-001', 'sub-001', 'starter', 'pro', '2026-01-15');
```

**چک‌لیست داده‌های تستی:**
- [ ] حداقل ۱۰۰ رکورد تستی توی Staging
- [ ] شامل همه plan types (starter, pro, enterprise, trial)
- [ ] شامل همه statusها (active, expired, grace_period, cancelled)
- [ ] شامل edge cases (payment failed, auto-renew on/off)
- [ ] شامل تاریخ‌های مختلف (امروز، دیروز، آینده)
- [ ] شامل duplicate records (اگه وجود داره)
- [ ] شامل data integrity issues (برای تست error handling)

---

#### ✅ ج) چک‌لیست اعتبارسنجی بعد از Migration

**بعد از اینکه Migration روی Staging اجرا شد:**

**۱. شمارش دقیق:**
```sql
-- مقایسه تعداد رکوردها
SELECT 'Before' as period, 50000 as count
UNION ALL
SELECT 'After', COUNT(*) FROM subscriptions;

-- باید: 50000 = 50000 ✅
```

- [ ] تعداد کل رکوردها قبل و بعد یکیه؟
- [ ] هیچ رکوردی حذف نشده؟
- [ ] هیچ رکورد اضافه ایجاد نشده؟

**۲. بررسی هر plan type:**
```sql
SELECT plan_type, COUNT(*) as count
FROM subscriptions 
GROUP BY plan_type
ORDER BY plan_type;

-- باید با آمار قبل یکی باشه:
-- starter: 20,000
-- pro: 15,000  
-- enterprise: 5,000
-- trial: 10,000
```

- [ ] starter: 20,000 ✅
- [ ] pro: 15,000 ✅
- [ ] enterprise: 5,000 ✅
- [ ] trial: 10,000 ✅

**۳. بررسی active vs expired:**
```sql
SELECT 
  plan_type,
  COUNT(CASE WHEN status = 'active' THEN 1 END) as active,
  COUNT(CASE WHEN status = 'expired' THEN 1 END) as expired
FROM subscriptions 
GROUP BY plan_type;

-- باید:
-- pro: active=10,000, expired=5,000
-- starter: active=15,000, expired=5,000
-- ...
```

- [ ] pro active: 10,000 ✅
- [ ] pro expired: 5,000 ✅
- [ ] starter active: 15,000 ✅
- [ ] starter expired: 5,000 ✅

**۴. بررسی تاریخ‌های خاص:**
```sql
-- اون ۱۵۰ تایی که امروز باید منقضی بشن
SELECT COUNT(*) FROM subscriptions 
WHERE end_date = CURRENT_DATE AND status = 'expired';
-- باید: 150 (یا به grace_period منتقل شدن)

-- اون ۵۰ تایی که منقضی شده ولی active بودن (bug)
SELECT COUNT(*) FROM subscriptions 
WHERE status = 'active' AND end_date < CURRENT_DATE;
-- باید: 0 (باید توسط migration درست شده باشن)
```

- [ ] سابسکریپشن‌های امروز منقضی شده → status=expired یا grace_period ✅
- [ ] اون ۵۰ تا باگ → درست شدن ✅

**۵. بررسی duplicate:**
```sql
-- اون ۱۲ نفر که duplicate داشتن
SELECT user_id, COUNT(*) as count
FROM subscriptions 
WHERE status = 'active'
GROUP BY user_id 
HAVING COUNT(*) > 1;

-- باید: 0 (migration باید اینا رو merge یا resolve کرده باشه)
-- یا اگه قرار بوده نگه داشته بشن → باید فقط ۱ active باشه
```

- [ ] duplicate active → resolve شده ✅

**۶. بررسی foreign keyها:**
```sql
-- سابسکریپشن بدون user
SELECT COUNT(*) FROM subscriptions s
LEFT JOIN users u ON s.user_id = u.id
WHERE u.id IS NULL;
-- باید: 0 (اون ۵ تایی که بودن باید درست شده باشن)
```

- [ ] همه سابسکریپشن‌ها user دارن؟ ✅
- [ ] همه paymentها subscription valid دارن؟ ✅
- [ ] همه تغییرات پلن subscription valid دارن؟ ✅

**۷. بررسی داده‌های تستی خاص:**
```sql
-- اون ۵ تا تستی که ساختیم رو چک کن
SELECT * FROM subscriptions 
WHERE user_id LIKE 'test-user-%' 
ORDER BY user_id;

-- sub-001: باید pro, active, auto_renew=true باشه
-- sub-002: باید starter, active, auto_renew=false باشه  
-- sub-003: باید expired باشه
-- sub-004: باید به grace_period یا expired منتقل شده باشه (چون امروز end_date)
-- sub-005: باید grace_period باشه
```

- [ ] sub-001 (pro, active) → به درستی migrate شده ✅
- [ ] sub-002 (starter, active) → به درستی migrate شده ✅
- [ ] sub-003 (expired) → به درستی migrate شده ✅
- [ ] sub-004 (ends today) → درست handle شده ✅
- [ ] sub-005 (grace period) → به درستی migrate شده ✅

**۸. بررسی payment history:**
```sql
-- تاریخچه پرداخت‌ها سالمه؟
SELECT COUNT(*) FROM subscription_payments;
-- باید همون تعداد قبل باشه

-- foreign keyها سالمه؟
SELECT COUNT(*) FROM subscription_payments p
LEFT JOIN subscriptions s ON p.subscription_id = s.id
WHERE s.id IS NULL;
-- باید: 0
```

- [ ] همه paymentها migrate شدن؟ ✅
- [ ] payment→subscription link سالمه؟ ✅

**۹. بررسی business logic:**
```bash
# APIهای subscription رو تست کن

# ۱. Get user subscription
curl https://api.staging.droplinked.com/users/test-user-001/subscription
# Expected: sub-001, pro, active, auto_renew=true

# ۲. Upgrade subscription
curl -X POST https://api.staging.droplinked.com/subscriptions/sub-001/upgrade \
  -d '{"to_plan": "enterprise"}'
# Expected: success, plan upgraded

# ۳. Cancel subscription
curl -X POST https://api.staging.droplinked.com/subscriptions/sub-002/cancel
# Expected: success, status=cancelled (در آخر دوره)

# ۴. Check expired handling
curl https://api.staging.droplinked.com/users/test-user-003/subscription
# Expected: sub-003, expired (و نه active!)
```

- [ ] Upgrade subscription کار می‌کنه؟ ✅
- [ ] Downgrade کار می‌کنه؟ ✅
- [ ] Cancel کار می‌کنه؟ ✅
- [ ] Auto-renew کار می‌کنه؟ ✅
- [ ] Grace period logic درسته؟ ✅
- [ ] Expired access مناسب handle می‌شه؟ ✅

**۱۰. بررسی Frontend:**
- [ ] کاربر با subscription active → به dashboard دسترسی داره؟ ✅
- [ ] کاربر با subscription expired → به صفحه renewal redirect می‌شه؟ ✅
- [ ] کاربر grace period → warning نشون داده می‌شه؟ ✅
- [ ] Plan selector درسته کار می‌کنه؟ ✅
- [ ] Pricing درست نشون داده می‌شه؟ ✅
- [ ] Upgrade/Downgrade flow کامل می‌شه؟ ✅

---

#### 📊 د) گزارش نهایی QA برای Feature Refactor

```markdown
## QA Report - Subscription Refactor

**تاریخ:** 2026-02-18  
**QA Engineer:** @mary-qa  
**دولوپر:** @ali-dev  
**وضعیت:** ✅ PASS / ❌ FAIL

### آمار قبل از Migration:
- Total Subscriptions: 50,000
  - starter: 20,000 (active: 15,000, expired: 5,000)
  - pro: 15,000 (active: 10,000, expired: 5,000)
  - enterprise: 5,000 (active: 4,000, expired: 1,000)
  - trial: 10,000 (active: 8,000, expired: 2,000)
- Data Issues: 50 expired-but-active, 12 duplicate-active, 5 null-user

### آمار بعد از Migration:
- Total Subscriptions: 50,000 ✅
  - starter: 20,000 ✅
  - pro: 15,000 ✅
  - enterprise: 5,000 ✅
  - trial: 10,000 ✅
- Data Issues Fixed: 
  - 50 expired-but-active → all fixed ✅
  - 12 duplicate-active → merged ✅
  - 5 null-user → assigned or removed ✅

### تست‌های تابعیتی:
- [x] Upgrade: 5/5 pass
- [x] Downgrade: 5/5 pass
- [x] Cancel: 5/5 pass
- [x] Auto-renew: 10/10 pass
- [x] Grace period: 8/8 pass
- [x] Expired handling: 10/10 pass

### Edge Cases تست شده:
- [x] End date = today → correctly marked expired
- [x] End date = yesterday → correctly marked expired
- [x] Payment failed during grace period → correctly cancelled
- [x] Auto-renew on but payment fails → grace period started
- [x] Plan change mid-cycle → prorated correctly

### API Tests:
- [x] All 25 endpoints responding correctly
- [x] Response times < 200ms
- [x] Error handling appropriate

### Frontend Tests:
- [x] Dashboard access control working
- [x] Plan selector functional
- [x] Payment flow complete
- [x] Mobile responsive

### باگ‌ها:
- ⚠️ Minor: Grace period message color should be yellow not red (Fixed)
- ⚠️ Minor: Upgrade confirmation modal slow to load on mobile (Fixed)

### نتیجه:
✅ **APPROVED FOR PRODUCTION**

**تاریخ پیشنهادی Deploy:** 2026-02-19  
**پنجره Maintenance:** 3:00-4:00 AM (کم‌ترافیک‌ترین زمان)
**Rollback Plan:** Backup taken, rollback tested
```

---

---

#### 🎯 ه) چک‌لیست آماده‌سازی دولوپر برای Feature Refactor

**قبل از اینکه دست به کد ببری، دولوپر باید این موارد رو آماده کنه:**

**۱. Data Analysis Report:**
```markdown
## Data Analysis Report - Subscription Refactor

### جداول مربوطه:
- subscriptions
- subscription_payments  
- subscription_changes

### آمار کلی:
| Table | Row Count | Size |
|-------|-----------|------|
| subscriptions | 50,000 | 25 MB |
| subscription_payments | 150,000 | 80 MB |
| subscription_changes | 5,000 | 2 MB |

### Distribution Analysis:
```sql
-- Plan distribution
SELECT plan_type, COUNT(*), 
       ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER(), 2) as percentage
FROM subscriptions GROUP BY plan_type;

-- Status distribution  
SELECT status, COUNT(*),
       ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER(), 2) as percentage
FROM subscriptions GROUP BY status;

-- Date distribution
SELECT 
  CASE 
    WHEN end_date < CURRENT_DATE THEN 'expired'
    WHEN end_date BETWEEN CURRENT_DATE AND CURRENT_DATE + 30 THEN 'expiring_soon'
    ELSE 'active_long_term'
  END as date_category,
  COUNT(*)
FROM subscriptions
GROUP BY date_category;
```

### Data Quality Issues:
- [ ] Orphaned records (subscription without user): 5
- [ ] Inconsistent status (expired date but active status): 50
- [ ] Duplicate active subscriptions: 12
- [ ] Missing payment records: 0
- [ ] Negative amounts: 0

### Business Rules to Migrate:
| Old Rule | New Rule | Migration Logic |
|----------|----------|----------------|
| status='active' AND end_date < today → still active | Should be 'expired' | UPDATE status='expired' |
| No grace period | 14-day grace period | Add grace_period_end = end_date + 14 |
| auto_renew=null | auto_renew=false | SET auto_renew=false WHERE null |
| plan='basic' | plan renamed to 'starter' | UPDATE plan='starter' WHERE 'basic' |
```

**۲. Migration Plan:**
```markdown
## Migration Plan

### Phase 1: Schema Changes
- [ ] Rename column: basic → starter
- [ ] Add column: grace_period_end (timestamp, nullable)
- [ ] Add column: cancellation_date (timestamp, nullable)
- [ ] Add column: prorated_amount (decimal, nullable)
- [ ] Create index: idx_user_id_status
- [ ] Create index: idx_end_date

### Phase 2: Data Migration
- [ ] Fix expired-but-active records (50 records)
- [ ] Set auto_renew=false where null (2,000 records)
- [ ] Calculate and set grace_period_end (all active records)
- [ ] Merge duplicate active subscriptions (12 users)
- [ ] Assign orphaned subscriptions to admin or delete (5 records)

### Phase 3: Validation
- [ ] Count verification
- [ ] Sample data verification
- [ ] Business rule verification
- [ ] API compatibility check

### Rollback Plan:
- [ ] Full database backup taken
- [ ] Rollback script prepared
- [ ] Rollback tested on staging
- [ ] Estimated rollback time: 5 minutes
```

**۳. Test Data Preparation:**
```markdown
## Test Data Scenarios

### Scenario 1: Normal Active Subscription
- User: test-active-pro
- Plan: pro
- Status: active
- End date: 2026-12-31
- Auto renew: true
- Expected after migration: Unchanged

### Scenario 2: Expired Today
- User: test-expire-today  
- Plan: starter
- Status: active (bug!)
- End date: 2026-02-18 (today)
- Expected after migration: status='expired'

### Scenario 3: Duplicate Active
- User: test-duplicate
- Plan 1: pro, active, ends 2026-12-31
- Plan 2: starter, active, ends 2026-06-30
- Expected after migration: Keep pro, cancel starter with prorated refund

### Scenario 4: Grace Period
- User: test-grace
- Plan: pro
- Status: active
- End date: 2026-02-01
- Current date: 2026-02-10 (9 days past)
- Expected after migration: status='grace_period', grace_period_end='2026-02-15'

### Scenario 5: Payment Failed
- User: test-failed-payment
- Plan: enterprise
- Status: active
- Auto renew: true
- Last payment: failed
- Expected after migration: status='grace_period' or 'payment_failed'
```

**۴. Checklist for Developer:**

- [ ] Data analysis complete with exact numbers
- [ ] All edge cases identified and documented
- [ ] Test data created (minimum 20 scenarios)
- [ ] Migration script written and tested locally
- [ ] Rollback script written and tested locally
- [ ] Business rules documented (old vs new)
- [ ] Data mapping documented (every field accounted for)
- [ ] API changes documented (breaking vs non-breaking)
- [ ] Performance impact assessed (how long will migration take)
- [ ] Backup strategy defined
- [ ] Maintenance window scheduled
- [ ] Team notified about changes

---

### 📊 تمپلیت گزارش QA برای Migration

```markdown
## QA Migration Report

**Migration:** add_subscription_status  
**تاریخ تست:** 2026-02-18  
**QA Engineer:** @mary-qa  
**نتیجه:** ✅ PASS / ❌ FAIL

### تست Structure:
- [x] ستون اضافه شده
- [x] نوع داده درسته (varchar(20))
- [x] default value = 'free'
- [x] nullable = YES

### تست Data Integrity:
- [x] تعداد رکوردها: 10,000 (تغییر نکرد)
- [x] همه رکوردهای قدیمی default 'free' دارن
- [x] هیچ null نیست
- [x] foreign keyها سالم

### تست Functionality:
- [x] کاربر جدید → subscription_status = 'free'
- [x] کاربر قدیمی → لاگین OK
- [x] Upgrade → OK
- [x] Downgrade → OK
- [x] Cancel → OK

### تست API:
- [x] GET /users - فیلد جدید رو برمی‌گردونه
- [x] POST /users - فیلد جدید رو قبول می‌کنه
- [x] PUT /users - update کار می‌کنه
- [x] APIهای قدیمی هنوز کار می‌کنن

### تست Frontend:
- [x] فرم نشون می‌ده
- [x] Validation کار می‌کنه
- [x] UI درسته

### تست Rollback:
- [x] Rollback موفق
- [x] داده‌ها سالم
- [x] دوباره migrate OK

### باگ‌ها:
- ❌ هیچی
- یا
- ⚠️ باگ جزئی: ...

### نتیجه نهایی:
✅ Ready for Production
```

---

## ۲. تست Web3 (Wallet, Smart Contract, Blockchain)

### 🎯 چالش Web3 چیه؟

Web3 متفاوت از Web2 معمولیه چون:
- تراکنش‌ها برگشت‌ناپذیرن (irreversible)
- نیاز به wallet دارن (Metamask, Phantom)
- گاهی gas fee دارن (هزینه تراکنش)
- شبکه‌های مختلف وجود دارن (Polygon, Ethereum, ...)
- Smart contractها رو باید تست کرد
- Transaction ممکنه fail بشه (network congestion, insufficient gas)

---

### 📋 چک‌لیست تست Web3 کامل

#### الف) تست Wallet Connection (اتصال ولت)

**برای هر فیچری که نیاز به wallet داره:**

**۱. Connect Wallet:**
- [ ] دکمه "Connect Wallet" نشون داده می‌شه؟
- [ ] کلیک روی دکمه → Modal باز می‌شه؟
- [ ] لیست walletها (Metamask, Phantom, ...) نمایش داده می‌شه؟
- [ ] انتخاب Metamask → Metamask popup باز می‌شه؟
- [ ] تایید در Metamask → wallet متصل می‌شه؟
- [ ] آدرس wallet نمایش داده می‌شه؟
- [ ] balance نشون داده می‌شه؟

**۲. Disconnect Wallet:**
- [ ] دکمه Disconnect کار می‌کنه؟
- [ ] بعد Disconnect وضعیت reset می‌شه؟
- [ ] session storage پاک می‌شه؟

**۳. Switch Wallet:**
- [ ] تغییر wallet کار می‌کنه؟
- [ ] داده‌های مربوط به wallet قبلی پاک می‌شه؟

**۴. Wrong Network:**
- [ ] اگه کاربر روی شبکه اشتباه باشه → warning نشون داده می‌شه؟
- [ ] دکمه "Switch Network" کار می‌کنه؟
- [ ] شبکه خودکار عوض می‌شه؟

**۵. Wallet Not Installed:**
- [ ] اگه Metamask نصب نباشه → پیام مناسب نشون داده می‌شه؟
- [ ] لینک دانلود Metamask هست؟

---

#### ب) تست Smart Contract (قرارداد هوشمند)

**برای Onchain Inventory / Record Product:**

**۱. Prepare Transaction:**
- [ ] Product info درست fetch می‌شه؟
- [ ] Metadata درست ساخته می‌شه؟
- [ ] Gas fee estimate درست محاسبه می‌شه؟
- [ ] Network درسته؟

**۲. Sign Transaction:**
- [ ] Transaction data درسته؟
- [ ] Popup امضا باز می‌شه؟
- [ ] کاربر می‌تونه reject کنه؟
- [ ] کاربر می‌تونه confirm کنه؟

**۳. Broadcast Transaction:**
- [ ] Transaction به شبکه broadcast می‌شه؟
- [ ] Transaction hash دریافت می‌شه؟
- [ ] لینک explorer (Etherscan/Polygonscan) نشون داده می‌شه؟

**۴. Confirm Transaction:**
- [ ] Status checking هر چند ثانیه انجام می‌شه؟
- [ ] Success نشون داده می‌شه؟
- [ ] Fail نشون داده می‌شه؟
- [ ] Timeout handling داره؟ (مثلاً بعد ۵ دقیقه)

**۵. Verify on Blockchain:**
- [ ] لینک explorer کار می‌کنه؟
- [ ] NFT واقعاً mint شده؟
- [ ] Token ID درسته؟
- [ ] Metadata درست ذخیره شده؟

---

#### ج) تست Network & Gas (شبکه و کارمزد)

**برای هر شبکه (Polygon, Base, Ethereum, ...):**

**۱. Network Availability:**
- [ ] شبکه available هست؟ (RPC کار می‌کنه؟)
- [ ] لیست شبکه‌ها درست لود می‌شه؟
- [ ] testnet vs mainnet قابل تشخیصه؟

**۲. Gas Fee:**
- [ ] Gas fee estimate درست محاسبه می‌شه؟
- [ ] مقدار gas به wallet user نشون داده می‌شه؟
- [ ] اگه gas کم باشه → warning؟
- [ ] اگه gas خیلی بالا باشه → confirmation اضافی؟

**۳. Balance Check:**
- [ ] قبل از تراکنش balance چک می‌شه؟
- [ ] اگه balance کمه → error مناسب؟
- [ ] اگه balance کافیه → اجازه تراکنش؟

**۴. Network Switch:**
- [ ] تغییر شبکه کار می‌کنه؟
- [ ] داده‌های مربوط به شبکه قبلی پاک می‌شه؟
- [ ] Gas fee برای شبکه جدید درست محاسبه می‌شه؟

---

#### د) تست Error Handling (مدیریت خطا)

**Scenarios مختلف:**

**۱. User Rejects Transaction:**
- [ ] کاربر reject می‌کنه → error مناسب؟
- [ ] UI reset می‌شه؟
- [ ] می‌تونه دوباره امتحان کنه؟

**۲. Insufficient Gas:**
- [ ] اگه gas نداشته باشه → error مناسب؟
- [ ] راهنمای خرید gas داده می‌شه؟

**۳. Network Congestion:**
- [ ] اگه شبکه شلوغ باشه → warning؟
- [ ] پیشنهاد افزایش gas؟

**۴. Transaction Fails on Chain:**
- [ ] Fail detected می‌شه؟
- [ ] Error reason نشون داده می‌شه؟ (revert message)
- [ ] Transaction fee از wallet کم شده؟ (failed tx هم gas داره!)
- [ ] Retry option وجود داره؟

**۵. Browser Closed During Transaction:**
- [ ] کاربر صفحه رو می‌بنده → چه اتفاقی می‌افته؟
- [ ] وقتی برمی‌گرده → status چک می‌شه؟
- [ ] pending transactions نشون داده می‌شن؟

**۶. Wallet Disconnected During Transaction:**
- [ ] wallet disconnect می‌شه → pause?
- [ ] reconnect prompt؟
- [ ] بعد reconnect ادامه پیدا می‌کنه؟

---

#### ه) تست Claim NFT (برای Shopfront)

**فرض:** فیچر Claim NFT توی Shopfront

**۱. Eligibility Check:**
- [ ] کاربر واجد شرایط claim هست؟
- [ ] NFT هنوز claim نشده؟
- [ ] محدودیت زمانی رعایت شده؟

**۲. Claim Process:**
- [ ] دکمه "Claim NFT" نشون داده می‌شه؟
- [ ] کلیک → wallet connection required؟
- [ ] Gas fee (اگه داره) نشون داده می‌شه؟
- [ ] Confirm → transaction broadcast می‌شه؟

**۳. After Claim:**
- [ ] Success message نشون داده می‌شه؟
- [ ] NFT به wallet کاربر منتقل می‌شه؟
- [ ] Status توی دیتابیس update می‌شه؟
- [ ] دکمه "Claim" disable می‌شه؟
- [ ] NFT توی OpenSea/Rarible دیده می‌شه؟

**۴. Already Claimed:**
- [ ] اگه قبلاً claim کرده → پیام مناسب؟
- [ ] دکمه disable؟
- [ ] لینک به NFT روی explorer؟

---

#### و) تست Data Consistency (یکپارچگی داده)

**مهم! داده‌های on-chain و off-chain باید sync باشن:**

- [ ] Product recorded on chain → توی دیتابیس هم recorded؟
- [ ] Transaction hash توی دیتابیس ذخیره می‌شه؟
- [ ] Token ID ذخیره می‌شه؟
- [ ] Contract address ذخیره می‌شه؟
- [ ] اگه on-chain باشه ولی off-chain نباشه → چی؟
- [ ] اگه off-chain باشه ولی on-chain نباشه → چی؟

---

### 🔧 ابزارهای تست Web3

**۱. برای تست Manual:**
- **Metamask** (wallet)
- **Testnet Faucets** (گرفتن gas رایگان برای تست)
  - Polygon Mumbai Faucet
  - Sepolia Faucet (Ethereum)
- **Block Explorers:**
  - Etherscan (Ethereum)
  - Polygonscan (Polygon)
  - BscScan (Binance)

**۲. برای تست اتوماتیک:**
- **Hardhat** (رایگان) - تست smart contract locally
- **Ganache** (رایگان) - local blockchain برای تست

**۳. برای Staging:**
- **Testnet** (نه Mainnet!) → پول واقعی نره
- Mumbai (Polygon testnet)
- Sepolia (Ethereum testnet)

---

### 🎯 چک‌لیست اختصاصی Onchain Inventory

**براساس document:**

**۱. Network Selection:**
- [ ] همه ۸ شبکه لیست شدن؟
- [ ] Non-custodial: SKALE, Redbelly, Xion
- [ ] External: Polygon, Base, Binance, Bitlayer, Ethereum

**۲. Wallet Handling:**
- [ ] Non-custodial → wallet address read-only
- [ ] External → connect button
- [ ] Wallet lock بعد اولین recording
- [ ] Error اگه wallet متفاوت باشه

**۳. Browse Inventory:**
- [ ] Modal باز می‌شه؟
- [ ] Search کار می‌کنه؟
- [ ] Filter by collection کار می‌کنه؟
- [ ] Product info درست نمایش داده می‌شه؟

**۴. Gas Fee:**
- [ ] Estimate درست محاسبه می‌شه؟
- [ ] USD equivalent نشون داده می‌شه؟
- [ ] Network currency درسته؟

**۵. Recording Process:**
- [ ] Progress modal باز می‌شه؟
- [ ] ۵ step نشون داده می‌شن؟
- [ ] Prepare → Sign → Broadcast → Confirm → Finalize
- [ ] Success یا Failed نشون داده می‌شه؟

**۶. After Recording:**
- [ ] Product read-only می‌شه؟
- [ ] NFT link توی storefront نمایش داده می‌شه؟
- [ ] Explorer link کار می‌کنه؟

**۷. Edge Cases:**
- [ ] Wrong wallet → error
- [ ] Wallet disconnected during recording → pause
- [ ] Network congestion → warning
- [ ] Transaction fails → error + retry
- [ ] Browser closed → recovery

---

## ۳. Health Check - چه چیزهایی باید چک بشه؟

### 🎯 تعریف Health Check

یه سری چک سریع که بعد از deploy (یا هر چند ساعت) اجرا می‌شن تا مطمئن بشیم سیستم سالمه.

**انواع Health Check:**
1. **Liveness:** سایت بالاست؟ (آدرس باز می‌شه؟)
2. **Readiness:** سایت آماده پاسخ‌دهیه؟ (دیتابیس وصله؟)
3. **Deep Health:** فیچرهای اصلی کار می‌کنن؟

---

### 📋 چک‌لیست Health Check برای Droplinked

#### Level ۱: Basic (Liveness) - هر ۱ دقیقه

**برای Dashboard:**
```bash
curl https://droplinked.com/health
# Expected: 200 OK
```

- [ ] سایت بالاست (HTTP 200)؟
- [ ] SSL certificate معتبره؟
- [ ] Response time < ۲ ثانیه؟

**برای API:**
```bash
curl https://api.droplinked.com/health
# Expected: {"status": "ok", "database": "connected", "redis": "connected"}
```

- [ ] API بالاست؟
- [ ] دیتابیس وصله؟
- [ ] Redis وصله (اگه استفاده می‌کنید)؟

**برای Checkout:**
```bash
curl https://checkout.droplinked.com/health
# Expected: 200 OK
```

- [ ] Checkout بالاست؟
- [ ] Payment gateway وصله؟

---

#### Level ۲: Functional (Readiness) - هر ۵ دقیقه

**چک‌های کاربردی:**

**۱. Authentication:**
- [ ] لاگین کار می‌کنه؟
- [ ] Session ساخته می‌شه؟
- [ ] Logout کار می‌کنه؟

**۲. Core Features:**
- [ ] Product list لود می‌شه؟
- [ ] Product detail باز می‌شه؟
- [ ] Add to cart کار می‌کنه؟
- [ ] Cart نمایش داده می‌شه؟

**۳. API Endpoints مهم:**
```bash
# Products
curl https://api.droplinked.com/api/products
# Expected: 200 + array of products

# User profile
curl -H "Authorization: Bearer $TOKEN" \
  https://api.droplinked.com/api/users/me
# Expected: 200 + user data

# Cart
curl -H "Authorization: Bearer $TOKEN" \
  https://api.droplinked.com/api/cart
# Expected: 200 + cart data
```

---

#### Level ۳: Deep Health - هر ۱ ساعت

**چک‌های عمیق:**

**۱. Database Health:**
- [ ] Connection pool healthy؟
- [ ] Query time < ۱۰۰ms؟
- [ ] No deadlocks؟

**۲. Web3 Health (اگه applicable):**
- [ ] RPC endpoints کار می‌کنن؟
- [ ] Wallet connection کار می‌کنه؟
- [ ] Smart contract قابل call هستن؟

**۳. Payment Health:**
- [ ] Payment gateway up هست؟
- [ ] Webhook delivery کار می‌کنه؟

**۴. Background Jobs:**
- [ ] Queue processor کار می‌کنه؟
- [ ] Jobs توی زمان انجام می‌شن؟
- [ ] Failed jobs داریم؟

---

### 📊 Monitoring Metrics (متریک‌های مانیتورینگ)

**چه چیزهایی باید monitor بشن:**

| Metric | Threshold | ابزار |
|--------|-----------|-------|
| **Uptime** | > 99.9% | UptimeRobot |
| **Response Time** | < 500ms | Datadog |
| **Error Rate** | < 1% | Sentry |
| **Database Connections** | < 80% | RDS Monitoring |
| **CPU Usage** | < 70% | CloudWatch |
| **Memory Usage** | < 80% | CloudWatch |
| **Disk Space** | < 85% | CloudWatch |

---

## ۴. Smoke Test - چیه و کی بنویسه؟

### 🎯 Smoke Test چیه؟

**تعریف ساده:**
یه سری تست خیلی سریع که چک می‌کنن "سیستم داره کار می‌کنه یا داره می‌سوزه؟"

**مثال:**
مثل اینه که وقتی ماشین رو روشن می‌کنی، اول چک کنی دود از اگزوز نده. اگه دود داد که نیازی نیست بری تست Drive!

---

### 👥 کی باید بنویسه؟

**۱. Product Manager / BA (تحلیل‌گر):**
- مشخص می‌کنه کدوم فیچرها حیاتی‌ترن
- اولویت‌ها رو تعیین می‌کنه
- Acceptance criteria می‌نویسه

**۲. QA Engineer:**
- Smoke test cases رو می‌نویسه
- اجرا می‌کنه
- گزارش می‌ده

**۳. Developer (کمک):**
- technical aspects رو کمک می‌کنه
- automated smoke tests رو نگهداری می‌کنه

---

### 📋 چه چیزهایی توی Smoke Test باشه؟

**اصول Smoke Test:**
1. **سریع** باشن (زیر ۵ دقیقه)
2. **Happy Path** رو تست کنن (نه edge cases)
3. **Critical features** رو پوشش بدن
4. **Pass/Fail** واضح باشه

---

### 📋 چک‌لیست Smoke Test برای Droplinked

**الف) Dashboard Smoke Test:**

```markdown
## Dashboard Smoke Test
⏱️ زمان: ۲ دقیقه

□ سایت بالاست؟
  → https://droplinked.com باز می‌شه؟
  → HTTP 200؟

□ لاگین کار می‌کنه؟
  → فرم لاگین نمایش داده می‌شه؟
  → لاگین با اکانت تستی کار می‌کنه؟
  → داشبورد لود می‌شه؟

□ محصولات لود می‌شن؟
  → لیست محصولات نمایش داده می‌شه؟
  → عکس‌ها لود می‌شن؟

□ Add Product کار می‌کنه؟
  → فرم ساخت محصول باز می‌شه؟
  → ذخیره می‌شه؟
```

**ب) Shopfront Smoke Test:**

```markdown
## Shopfront Smoke Test
⏱️ زمان: ۲ دقیقه

□ Storefront بالاست؟
  → https://droplinked.io/test-shop باز می‌شه؟

□ Products لود می‌شن؟
  → لیست محصولات نمایش داده می‌شه؟
  → عکس‌ها لود می‌شن؟

□ Product Detail کار می‌کنه؟
  → کلیک روی محصول → صفحه detail باز می‌شه؟
  → اطلاعات محصول نمایش داده می‌شه؟

□ Add to Cart کار می‌کنه؟
  → دکمه کار می‌کنه؟
  → سبد خرید update می‌شه؟
```

**ج) Checkout Smoke Test:**

```markdown
## Checkout Smoke Test
⏱️ زمان: ۳ دقیقه

□ Checkout بالاست؟
  → https://checkout.droplinked.com لود می‌شه؟

□ Cart Display کار می‌کنه؟
  → محصولات سبد نمایش داده می‌شن؟
  → قیمت‌ها درستن؟

□ Customer Info کار می‌کنه؟
  → فرم اطلاعات مشتری باز می‌شه؟
  → Validation کار می‌کنه؟

□ Payment Methods لود می‌شن؟
  → لیست روش‌های پرداخت نمایش داده می‌شه؟

□ Order Creation کار می‌کنه؟
  → سفارش ثبت می‌شه؟
  → Confirmation نمایش داده می‌شه؟
```

**د) Web3 Smoke Test (اگه applicable):**

```markdown
## Web3 Smoke Test
⏱️ زمان: ۵ دقیقه

□ Wallet Connection کار می‌کنه؟
  → دکمه Connect نشون داده می‌شه؟
  → کلیک → Modal باز می‌شه؟

□ Network List لود می‌شه؟
  → لیست شبکه‌ها نمایش داده می‌شه؟
  → حداقل ۳ شبکه available هستن؟

□ Gas Estimate کار می‌کنه؟
  → مقدار gas نشون داده می‌شه؟
  → USD equivalent درسته؟

□ Transaction Signing (Testnet):
  → Sign transaction کار می‌کنه؟
  → Transaction hash دریافت می‌شه؟
```

---

### 🤖 اتوماتیک کردن Smoke Test

**کی باید اجرا بشه؟**
- بعد از هر deploy
- هر ۳۰ دقیقه (در production)
- قبل از مرج به main

**GitHub Actions:**

```yaml
name: Smoke Tests

on:
  deployment_status:
    types: [success]
  schedule:
    - cron: '*/30 * * * *'  # هر ۳۰ دقیقه

jobs:
  smoke-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Test Suite
        uses: actions/checkout@v4
      
      - name: Run Smoke Tests
        run: |
          # تست Dashboard
          curl -f https://droplinked.com/health || exit 1
          
          # تست API
          curl -f https://api.droplinked.com/health || exit 1
          
          # تست Checkout
          curl -f https://checkout.droplinked.com/health || exit 1
      
      - name: Notify if Failed
        if: failure()
        run: |
          curl -X POST $SLACK_WEBHOOK \
            -d '{"text":"🔥 SMOKE TEST FAILED! Production might be down!"}'
```

**با Playwright (E2E Smoke Test):**

```javascript
// tests/smoke/dashboard.spec.js
const { test, expect } = require('@playwright/test');

test.describe('Dashboard Smoke Test', () => {
  
  test('site is up', async ({ page }) => {
    const response = await page.goto('https://droplinked.com');
    expect(response.status()).toBe(200);
  });

  test('login works', async ({ page }) => {
    await page.goto('https://droplinked.com/login');
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'testpass');
    await page.click('button[type="submit"]');
    await expect(page).toHaveURL(/dashboard/);
  });

  test('products load', async ({ page }) => {
    await page.goto('https://droplinked.com/dashboard');
    await page.waitForSelector('[data-testid="product-list"]');
    const products = await page.$$('[data-testid="product-item"]');
    expect(products.length).toBeGreaterThan(0);
  });
});
```

---

## خلاصه: نقش‌ها و مسئولیت‌ها

### Product Manager:
- ✅ مشخص می‌کنه کدوم فیچرها حیاتی‌ترن
- ✅ Acceptance criteria می‌نویسه
- ✅ اولویت Smoke Testها رو تعیین می‌کنه

### QA Engineer:
- ✅ Test cases رو می‌نویسه (migration, web3, smoke)
- ✅ تست‌ها رو روی Staging اجرا می‌کنه
- ✅ Data Integrity رو چک می‌کنه
- ✅ گزارش می‌نویسه
- ✅ Smoke Tests رو خودکار می‌کنه

### Developer:
- ✅ Migration document می‌نویسه
- ✅ Rollback رو تست می‌کنه
- ✅ Unit Tests می‌نویسه
- ✅ Smoke Tests رو بررسی می‌کنه
- ✅ Bug fixes

### DevOps/SRE:
- ✅ Health Checks رو monitor می‌کنه
- ✅ Alerts تنظیم می‌کنه
- ✅ Rollback انجام می‌ده (اگه نیاز باشه)

---

**تهیه شده برای:** تیم Droplinked  
**تاریخ:** بهمن ۱۴۰۴

# Credits & Coupons

# **Feature ID:** IAA-CC-101

# **Category:** Billing & Promotions | **Actors:** Merchant

# **Status:** Defined

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need an interface to manage **store credits** and **discount coupons**.

- Credits allow merchants to add balance to their shop (used for internal payments).
- Coupons allow merchants to create discount codes for customers.
    
    This page centralizes both, with permissions based on subscription level.
    

### **Desired Outcome**

- Merchant can view and charge shop credits.
- Merchant with a Premium plan can create, view, edit, and export coupon codes.
- Proper validation for coupon creation.
- Empty states and detail views work properly.

---

# **2) Scope – In / Out**

### **In Scope**

### **Credits Section**

- Display current store credit balance.
- “Charge” button → opens **Add Credit Modal**.
- Add Credit Modal:
    - Merchant enters credit amount.
    - Payment processed via Stripe.
    - On success → credit balance increases.

### **Coupons Section**

- Shown **only** if merchant has a *Premium Plan*.
- Empty state when no coupons exist.
- Create Coupon modal with fields:
    - **Title** (required, max 100 chars)
    - **Type**: Percentage OR Fixed/Fiat
    - **Amount** (required)
        - If Percentage → must be 1–99
        - If Fiat → must be > 0
    - **Quantity** (required, number of generated codes)
    - **Expiration Date** (required)
- Save coupon → system auto-generates the requested number of unique codes.
- Coupon table fields:
    - Title
    - Quantity
    - Amount
    - Expiration Date
    - Type (Coupon or Credit)
- Coupon detail view:
    - Shows full list of generated codes.
    - Includes **Export button** to download Excel of all codes.

### **Edit Coupon**

- Only **Expiration Date** can be edited after creation.

### **Out of Scope**

- Applying coupons at checkout (handled in storefront domain).
- Deleting individual codes.
- Analytics for coupon usage.

---

# **3) Key User Journeys**

---

## **Journey 1 – Viewing & Charging Credits**

**Step 1:** Merchant navigates to Settings → Credits & Coupons.

**Step 2:** Credits balance is displayed at top of page.

**Step 3:** Merchant clicks **“Charge”** → Add Credit Modal opens.

**Step 4:** Merchant enters amount and confirms payment via Stripe.

**Step 5:** Credits are added and balance updates.

---

## **Journey 2 – Creating First Coupon (Premium Merchants Only)**

**Step 1:** Merchant scrolls to **Coupons Section** (visible only for Premium).

**Step 2:** No coupons exist → Empty State displayed.

**Step 3:** Merchant clicks **“Create Coupon”** → modal opens.

**Step 4:** Merchant fills required fields:

- Title
- Type → Percentage or Fiat
- Amount (validated based on type)
- Quantity
- Expiration Date
    
    **Step 5:** Merchant clicks “Create”.
    
    **Step 6:** Coupon saved → system generates N unique codes.
    
    **Step 7:** Coupon table updates with new entry.
    

---

## **Journey 3 – Editing Coupon**

**Step 1:** Merchant clicks **Edit** on a coupon.

**Step 2:** Only Expiration Date is editable.

**Step 3:** Merchant updates date → saves changes.

**Step 4:** Coupon reflects new expiration value.

---

## **Journey 4 – Viewing Coupon Detail + Exporting Codes**

**Step 1:** Merchant clicks **“Details”** on a coupon.

**Step 2:** Detail page lists all generated codes (Quantity count).

**Step 3:** Merchant clicks **Export** → Excel file generated and downloaded.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Credits section must always be visible; Coupons section visible only for Premium plan.

**BAC 2:** Add Credit flow must integrate with Stripe and update balance upon successful payment.

**BAC 3:** Create Coupon modal must enforce all required fields and validation rules:

- Title max 100 chars
- Percentage: Amount 1–99
- Fiat: Amount > 0
- Quantity required
- Expiration Date required

**BAC 4:** On coupon creation, system must generate unique codes equal to Quantity.

**BAC 5:** Coupon table must display Title, Quantity, Amount, Expiration Date, Type.

**BAC 6:** Only Expiration Date can be edited after creation.

**BAC 7:** Coupon detail view must show all codes and allow exporting them as Excel.

**BAC 8:** Empty state must be shown when no coupons exist.
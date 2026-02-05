# Step 3 – Default Currency Selection

---

## **Step 3 – Default Currency Selection**

### Feature ID:

**ONB-STEP3-003**

### Title:

**Default Currency Selection**

### Category:

**Onboarding / Merchant Setup** | **Actors**: Merchant | **Channel**: Merchant Dashboard

### Status:

**Defined**

### Owner:

**PM X**

---

### 1) **Summary**

**Problem/Value:**

Merchants need to select a default currency for their shop so that all product prices, shipping costs, and gift card values can be entered and displayed consistently. This ensures clarity in pricing for both the merchant and customers.

**Desired Outcome:**

- Merchants can set a default currency for their shop during onboarding.
- Prices entered afterward (products, shipping, gift cards) use the selected currency.
- Customers see all product and cart prices in the selected currency.
- Subscription page remains in USD.
- Merchant can skip this step and configure currency later if desired.

---

### 2) **Scope – In / Out**

**In:**

- Merchant can select a default currency for the shop.
- All subsequent product, shipping, and gift card prices are entered in this currency.
- Customer-facing storefront displays prices in the selected currency.
- Skip option available to configure later.

**Out:**

- Automatic conversion of previously entered prices.
- Multi-currency display (only one currency applied per shop).
- Subscription page pricing conversion (remains USD).

---

### 3) **Key User Journeys**

- **Journey 1:**
    - **Role/Trigger**: Merchant reaches Step 3
    - **Behavior/Action**: Selects default currency from dropdown
    - **Value/Outcome**: All new product, shipping, and gift card prices use this currency
- **Journey 2:**
    - **Role/Trigger**: Merchant skips this step
    - **Behavior/Action**: Proceeds without selecting a currency
    - **Value/Outcome**: Merchant can configure default currency later in shop settings
- **Journey 3:**
    - **Role/Trigger**: Customer visits storefront
    - **Behavior/Action**: Views product and cart prices
    - **Value/Outcome**: Prices are displayed in the shop’s selected currency

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1**: Merchant can select a default currency → Pass/Fail
- **BAC 2**: Prices for new products, shipping, and gift cards reflect the selected currency → Pass/Fail
- **BAC 3**: Merchant can skip the step and update currency later → Pass/Fail
- **BAC 4**: Customer sees product prices in selected currency → Pass/Fail
- **BAC 5**: Subscription page prices remain in USD → Pass/Fail

---
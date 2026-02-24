# Shop Currency Selection

## **Shop Currency Selection**

### Feature ID:

**SHOP-CUR-001**

### Title:

**Shop Currency – Set Default Currency for Storefront**

### Category:

**Shop Settings / Pricing** | **Actors**: Merchant, Customer | **Channel**: Merchant Dashboard / Storefront

### Status:

**Defined**

### Owner:

**PM X**

---

### 1) **Summary**

**Problem/Value:**

Merchants need the ability to set a default currency for their shop so that all product pricing, shipping costs, and gift card values are displayed and entered consistently. Customers see all prices in the shop’s chosen currency, simplifying the shopping experience and avoiding confusion with multiple currencies.

**Desired Outcome:**

- Merchants can select a default currency for their shop.
- All new prices (product, shipping, gift card) are entered in the selected currency.
- Previously entered prices remain unchanged—they are **not auto-converted**.
- Customers see product and cart prices in the selected currency.
- Subscription page pricing remains in USD.

---

### 2) **Scope – In / Out**

**In:**

- Select a default currency for the shop.
- Enter product prices, shipping prices, and gift card prices in the selected currency.
- Customer-facing storefront displays prices in the selected currency.
- Previously added prices remain unchanged when changing currency.

**Out:**

- Automatic conversion of existing product prices when currency changes.
- Multi-currency storefront display (single currency only).
- Subscription pricing conversion (remains USD).

---

### 3) **Key User Journeys**

- **Journey 1:**
    - **Role/Trigger**: Merchant navigates to Shop Settings → Currency
    - **Behavior/Action**: Selects default currency for shop
    - **Value/Outcome**: New prices for products, shipping, and gift cards will be entered in selected currency
- **Journey 2:**
    - **Role/Trigger**: Merchant adds a new product
    - **Behavior/Action**: Enters price in shop’s selected currency
    - **Value/Outcome**: Price is stored in the selected currency
- **Journey 3:**
    - **Role/Trigger**: Customer visits storefront
    - **Behavior/Action**: Views product prices and shipping costs
    - **Value/Outcome**: Prices displayed in the shop’s selected currency
- **Journey 4:**
    - **Role/Trigger**: Merchant changes default currency
    - **Behavior/Action**: Previously added prices remain unchanged; new prices use selected currency
    - **Value/Outcome**: Consistency in currency input without altering historical prices

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1**: Merchant can select a default currency for the shop → Pass/Fail
- **BAC 2**: New product, shipping, and gift card prices are entered in the selected currency → Pass/Fail
- **BAC 3**: Previously added prices remain unchanged after changing currency → Pass/Fail
- **BAC 4**: Customers see all prices in the shop’s selected currency → Pass/Fail
- **BAC 5**: Subscription page remains in USD regardless of shop currency → Pass/Fail

---
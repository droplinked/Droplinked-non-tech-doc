# Affiliate Network Overview

# Affiliate Network Overview

### Feature ID:

**[IAA-AFF-500]**

### Title:

**Affiliate Network – System Overview**

### Category:

**Affiliate** | **Actors**: Merchant (Affiliator), Merchant (Co-seller), Customer | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants want to expand their sales reach without additional marketing efforts. Product owners (Affiliators) want others to sell their products and pay commission only on successful sales. Sellers (Co-sellers) want to earn money by selling existing products without inventory management or product creation. The Affiliate Network connects these two parties in a mutually beneficial marketplace.

**Desired Outcome:**

- Merchants can choose to be an **Affiliator** (product owner who lets others sell) OR a **Co-seller** (seller who sells others' products) – but NOT both.
- Affiliators can list their products on the Affiliate Marketplace with commission rates.
- Co-sellers can browse the marketplace, import products, and sell them in their shop.
- When a sale occurs, both parties receive their share automatically as shop credits.
- Clear dashboards for both roles to track performance and earnings.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Role System**
    - Exclusive roles: Affiliator OR Co-seller (not both)
    - Role is permanent once assigned
    - Role tracking in database
- **Affiliator Features**
    - Activate Affiliate Network
    - Configure affiliate settings (category, description, commission)
    - Choose products and set commission rates
    - Enable/disable affiliate status
    - View Co-seller performance dashboard
- **Co-seller Features**
    - Browse Affiliate Marketplace
    - View product and shop details
    - Import products to their shop
    - View earnings dashboard
- **Marketplace**
    - Product listing with filters and search
    - Shop pages with product lists
    - Product detail pages with commission info
- **Order Processing**
    - Affiliate orders tracked separately
    - Automatic credit distribution
    - Affiliator handles shipping
- **Dashboards**
    - Affiliator performance metrics
    - Co-seller earnings metrics

**Out of Scope:**

- Multiple roles per merchant
- Role switching
- Direct payment (uses credit system)
- Refund handling for affiliate orders
- Coupon codes on affiliate products
- Non-USD currency shops
- Imported products in inventory list

---

### 3) **Role Definitions**

| Role | Definition | How to Become | Restrictions |
| --- | --- | --- | --- |
| **Affiliator** | Product owner who enables others to sell their products | Activate Affiliate Network | Cannot import products from marketplace |
| **Co-seller** | Seller who imports and sells other merchants' products | Import first product from marketplace | Cannot activate Affiliate Network |
| **None** | Merchant with no affiliate role | Default state | Can browse marketplace, can choose either role |

---

### 4) **Key Business Rules**

| Rule ID | Rule Description |
| --- | --- |
| R-1 | A merchant can only have ONE affiliate role (Affiliator OR Co-seller) |
| R-2 | Role cannot be changed once assigned, even if all products are removed |
| R-3 | Both shop currencies must be USD for affiliate transactions |
| R-4 | Coupon codes cannot be applied to carts containing affiliate products |
| R-5 | Earnings are distributed as shop credits, not direct payments |
| R-6 | Affiliator is responsible for shipping affiliate orders |
| R-7 | Imported products do NOT appear in Co-seller's inventory |
| R-8 | When Affiliator disables affiliate, products become "Out of Stock" (not deleted) |
| R-9 | Price/commission changes by Affiliator reflect immediately for Co-sellers |
| R-10 | If Affiliator deletes product or sets stock to 0, product is removed from Co-seller shops |

---

### 5) **Revenue Split Model**

**Example: Product Price = $100, Commission = 15%**

| Party | Calculation | Amount |
| --- | --- | --- |
| Co-seller | Commission % × Price | $15 |
| Affiliator | Price - Commission | $85 |

**Payment Flow:**

1. Customer pays full price ($100) to Droplinked
2. Droplinked credits Co-seller's shop: $15
3. Droplinked credits Affiliator's shop: $85
4. Both credits are available immediately after payment confirmation

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Updated Business Rule R-3: Both shop currencies must be USD for affiliate transactions | Business requirement - Affiliate only for USD shops |
| 2026-02-01 | Behdad | Initial document creation | New Feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Affiliator** | Merchant who owns products and allows others to sell them for commission |
| **Co-seller** | Merchant who imports and sells other merchants' products |
| **Commission** | Percentage of product price paid to Co-seller on each sale |
| **Affiliate Product** | Product listed on marketplace for Co-sellers to import |
| **Imported Product** | Affiliate product added to Co-seller's shop |
| **Affiliate Order** | Order containing at least one imported affiliate product |
| **Shop Credit** | Balance in merchant's Droplinked account |

---

### B2) Role Assignment Logic

```
FUNCTION determineRole(merchant):
    IF merchant.affiliate_role != null:
        RETURN merchant.affiliate_role  // Already assigned
    ELSE:
        RETURN 'none'  // No role yet

FUNCTION assignRole(merchant, action):
    IF merchant.affiliate_role != null:
        RETURN error("Role already assigned and cannot be changed")

    IF action == 'activate_affiliate':
        merchant.affiliate_role = 'affiliator'
        merchant.affiliate_activated_at = NOW
        SAVE merchant

    ELSE IF action == 'import_product':
        merchant.affiliate_role = 'co-seller'
        merchant.coseller_activated_at = NOW
        SAVE merchant

    RETURN success

FUNCTION canPerformAction(merchant, action):
    role = merchant.affiliate_role

    IF action == 'activate_affiliate':
        RETURN role == null OR role == 'none'

    IF action == 'import_product':
        RETURN role == null OR role == 'none' OR role == 'co-seller'

    IF action == 'browse_marketplace':
        RETURN true  // Everyone can browse

    RETURN false

```

---

### B3) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| No Role | Activate Affiliate | Affiliator | Affiliate settings page shown |
| No Role | Import Product | Co-seller | Product added to shop |
| Affiliator | (any action) | Affiliator | Role locked permanently |
| Co-seller | (any action) | Co-seller | Role locked permanently |
| Affiliator | Disable Affiliate | Affiliator (disabled) | Products become Out of Stock for Co-sellers |
| Affiliator | Enable Affiliate | Affiliator (enabled) | Products available again |
| Affiliator | Delete Product | Affiliator | Product removed from all Co-seller shops |

---

### B4) Data Requirements

**Merchant Affiliate Profile:**

| Field | Type | Description |
| --- | --- | --- |
| affiliate_role | Enum | null, 'affiliator', 'co-seller' |
| affiliate_activated_at | DateTime | When affiliate was activated (for affiliators) |
| coseller_activated_at | DateTime | When first product was imported (for co-sellers) |
| affiliate_enabled | Boolean | Whether affiliate is currently active (affiliators only) |
| affiliate_category | String | Shop category for marketplace |
| affiliate_description | String | Shop description (max 500 chars) |
| commission_mode | Enum | 'fixed', 'manual' |
| fixed_commission_rate | Integer | 1-99 (if mode is fixed) |

**Affiliate Product:**

| Field | Type | Description |
| --- | --- | --- |
| product_id | UUID | Original product ID |
| shop_id | UUID | Affiliator's shop ID |
| commission_rate | Integer | 1-99 percentage |
| is_active | Boolean | Whether available on marketplace |
| created_at | DateTime | When added to affiliate |

**Imported Product:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Unique import record ID |
| affiliate_product_id | UUID | Reference to affiliate product |
| coseller_shop_id | UUID | Co-seller's shop ID |
| imported_at | DateTime | When imported |
| is_active | Boolean | Whether currently in Co-seller's shop |

---

### B5) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Merchant with no role activates affiliate | Becomes Affiliator |
| TC-2 | Merchant with no role imports product | Becomes Co-seller |
| TC-3 | Affiliator tries to import product | Action blocked |
| TC-4 | Co-seller tries to activate affiliate | Action blocked |
| TC-5 | Affiliator deletes all products, tries to become Co-seller | Not allowed |
| TC-6 | Co-seller removes all imports, tries to become Affiliator | Not allowed |
| TC-7 | Non-USD shop tries to import | Error shown |
| TC-8 | Apply coupon to cart with affiliate product | Coupon rejected |
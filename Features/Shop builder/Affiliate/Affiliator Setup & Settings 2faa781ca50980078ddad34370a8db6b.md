# Affiliator Setup & Settings

# Affiliator Setup & Settings

### Feature ID:

**[IAA-AFF-501]**

### Title:

**Affiliator Setup & Settings**

### Category:

**Affiliate** | **Actors**: Merchant (Affiliator) | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Product owners want to leverage other merchants to sell their products without managing additional sales channels. They need a simple way to activate affiliate selling, configure their shop profile for the marketplace, and set commission rates for their products.

**Desired Outcome:**

- Merchants can activate Affiliate Network with clear onboarding.
- Configure shop profile for marketplace visibility (category, description).
- Choose between fixed commission for all products or manual per-product commission.
- Select which products to list on marketplace.
- Enable/disable affiliate status at any time.
- Shop URL is shareable for direct marketplace access.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Activation Flow**
    - First-time activation page with explanation
    - Activation modal with details
    - One-click activation (only for shops with USD currency)
    - Currency validation before activation
- **General Information Section**
    - Shop URL display with copy functionality
    - Category selection (dropdown)
    - Description field (max 500 characters)
- **Commission and Products Section**
    - Fixed commission mode (all products, one rate)
    - Manual selection mode (per-product rates)
    - Product browser with search and filter
    - Commission rate input (1-99%)
    - Add/remove products from affiliate
- **Additional Settings**
    - Enable/disable affiliate toggle
- **Marketplace Visibility**
    - Products appear on marketplace when affiliate is enabled

**Out of Scope:**

- Multiple commission tiers
- Time-limited commissions
- Automatic commission adjustments
- Product bundles for affiliate
- Co-seller approval workflow

---

### 3) **Key User Journeys**

---

**Journey 1: First-Time Activation (USD Currency Required)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Affiliate Network" in sidebar | System checks shop currency |
| 2 | System | Validates currency | IF not USD: Show currency error page |
| 3 | Merchant (USD shop) | Views activation page | Message explaining affiliate + "Learn More" link |
| 4 | Merchant | Clicks "Activate Affiliate Network" | Modal opens with detailed explanation |
| 5 | Merchant | Reads modal content | Explains benefits and how it works |
| 6 | Merchant | Clicks "Activate Affiliate Network" in modal | System validates USD currency |
| 7 | System | Currency validation passed | Affiliate activated |
| 8 | System | Sets merchant role to Affiliator | Role locked permanently |
| 9 | System | Redirects to settings page | Settings page with configuration options |

---

**Journey 2: Configure General Information**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views General Information section | Shop URL, Category, Description shown |
| 2 | Affiliator | Clicks copy icon on URL | Marketplace URL copied to clipboard |
| 3 | Affiliator | Selects category from dropdown | Category saved |
| 4 | Affiliator | Enters description (max 500 chars) | Character count shown, saved on blur |
| 5 | System | Updates marketplace profile | Changes reflect on marketplace |

---

**Journey 3: Set Fixed Commission for All Products**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views Commission section | Two options displayed |
| 2 | Affiliator | Selects "Fixed commission for all products" | Input field appears |
| 3 | Affiliator | Enters commission rate (e.g., 15%) | Validates 1-99 range |
| 4 | Affiliator | Saves | All products now available on marketplace with 15% commission |
| 5 | System | Lists all products | Products appear on marketplace |

---

**Journey 4: Manual Product Selection with Custom Commissions**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Selects "Manual Selection" | Browse Inventory button appears |
| 2 | Affiliator | Clicks "Browse Inventory" | Product selection modal opens |
| 3 | Affiliator | Searches or filters products | Products filtered by title or collection |
| 4 | Affiliator | Selects products | Products added to selection list |
| 5 | Affiliator | For each product, enters commission rate | Input validates 1-99% |
| 6 | Affiliator | Saves selection | Selected products appear on marketplace |
| 7 | Affiliator | Can add more or remove products | List updates accordingly |

---

**Journey 5: Enable/Disable Affiliate**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views Additional Settings | Enable/Disable toggle shown |
| 2 | Affiliator | Toggles OFF | Confirmation shown |
| 3 | System | Disables affiliate | Products become "Out of Stock" for Co-sellers |
| 4 | System | Keeps sales history | Historical data preserved |
| 5 | Affiliator | Toggles ON | Products available again on marketplace |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Activation** |  |
| BAC-1 | First-time visitors see activation page with explanation |
| BAC-2 | Activation modal explains affiliate benefits clearly |
| BAC-3 | After activation, merchant role is set to "Affiliator" permanently |
| BAC-4 | Affiliate settings page loads after activation |
| BAC-4.1 | Affiliate activation is ONLY available for shops with USD currency |
| **General Information** |  |
| BAC-5 | Shop marketplace URL is displayed and copyable |
| BAC-6 | Category dropdown shows 10 predefined categories |
| BAC-7 | Description field allows max 500 characters with counter |
| BAC-8 | Changes save automatically or on explicit save action |
| **Commission Mode** |  |
| BAC-9 | Two commission modes available: Fixed and Manual |
| BAC-10 | Fixed mode applies one commission rate to ALL products |
| BAC-11 | Manual mode allows per-product commission rates |
| BAC-12 | Commission rate must be between 1-99% |
| **Product Selection** |  |
| BAC-13 | Product browser modal supports search by title |
| BAC-14 | Product browser modal supports filter by collection |
| BAC-15 | Selected products can be added or removed |
| BAC-16 | Each product in manual mode must have a commission rate |
| **Enable/Disable** |  |
| BAC-17 | Toggle controls affiliate visibility on marketplace |
| BAC-18 | Disabling makes products "Out of Stock" for Co-sellers |
| BAC-19 | Enabling restores product availability |
| BAC-20 | Sales history is preserved when toggling |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Added USD currency requirement for Affiliate activation | Business requirement - Affiliate only for USD shops |
| 2026-02-01 | Behdad | Initial document creation | New Feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Fixed Commission Mode** | All products share the same commission percentage |
| **Manual Selection Mode** | Each product can have a different commission percentage |
| **Marketplace URL** | Direct link to shop's page on affiliate marketplace |
| **Affiliate Status** | Whether the shop's products are visible on marketplace |

---

### B2) Shop Categories

| # | Category |
| --- | --- |
| 1 | Fashion & Apparel |
| 2 | Electronics & Gadgets |
| 3 | Home & Living |
| 4 | Beauty & Personal Care |
| 5 | Sports & Outdoors |
| 6 | Art & Collectibles |
| 7 | Books & Education |
| 8 | Food & Beverages |
| 9 | Health & Wellness |
| 10 | Toys & Games |

---

### B3) Exhaustive Functional Logic

---

### **FL-1: First Visit Check**

```
ON NavigateTo('/affiliate-network'):
    merchant = getCurrentMerchant()

    IF merchant.affiliate_role == 'co-seller':
        REDIRECT to '/affiliate-network/marketplace'
        RETURN

    IF merchant.shop_currency != 'USD':
        RENDER CurrencyErrorPage(
            message: "Affiliate Network is only available for shops with USD currency"
        )
        RETURN

    IF merchant.affiliate_role == null:
        IF NOT merchant.affiliate_activated:
            RENDER ActivationPage()
        ELSE:
            RENDER SettingsPage()

    IF merchant.affiliate_role == 'affiliator':
        RENDER SettingsPage()

```

---

### **FL-2: Activation Flow**

```
ActivationPage:
    - Title: "Affiliate Network"
    - Message: "Enable others to sell your products and earn more"
    - Link: "Learn More" → Opens help article
    - Button: "Activate Affiliate Network"

ON ClickActivateButton:
    OPEN ActivationModal

ActivationModal:
    - Title: "Activate Affiliate Network"
    - Content:
        "By activating the Affiliate Network, you allow other merchants
         (Co-sellers) to import and sell your products. You set the
         commission rate, and they earn a percentage of each sale.

         Benefits:
         • Expand your reach without marketing costs
         • Pay only when sales happen
         • Full control over which products and commission rates"
    - Button: "Activate Affiliate Network"
    - Button: "Cancel"

ON ClickActivateInModal:
    SET loading = true

    // Validate currency is USD
    IF merchant.shop_currency != 'USD':
        SHOW error "Affiliate Network is only available for shops with USD currency"
        SET loading = false
        RETURN

    response = API.activateAffiliate(merchant.id)

    IF response.success:
        merchant.affiliate_role = 'affiliator'
        merchant.affiliate_activated = true
        merchant.affiliate_enabled = true
        merchant.affiliate_activated_at = NOW

        CLOSE modal
        NAVIGATE to '/affiliate-network/settings'
    ELSE:
        SHOW error response.message

    SET loading = false

```

---

### **FL-3: Settings Page Structure**

```
SettingsPage:
    SECTION GeneralInformation:
        - ShopURL (readonly, with copy button)
        - CategoryDropdown
        - DescriptionTextarea

    SECTION CommissionAndProducts:
        - RadioGroup: [Fixed, Manual]
        - IF Fixed: CommissionRateInput
        - IF Manual: ProductSelectionList + BrowseButton

    SECTION AdditionalSettings:
        - EnableAffiliateToggle

```

---

### **FL-4: General Information Logic**

```
ON PageLoad:
    shopURL = "marketplace.droplinked.io/shop/" + merchant.shop_url
    selectedCategory = merchant.affiliate_category OR null
    description = merchant.affiliate_description OR ""

ON ClickCopyURL:
    COPY shopURL to clipboard
    SHOW toast "URL copied to clipboard"

ON CategoryChange(value):
    merchant.affiliate_category = value
    API.updateAffiliateSettings({ category: value })

ON DescriptionChange(value):
    IF value.length > 500:
        SHOW error "Maximum 500 characters allowed"
        RETURN

    merchant.affiliate_description = value
    DEBOUNCE 500ms:
        API.updateAffiliateSettings({ description: value })

```

---

### **FL-5: Commission Mode Logic**

```
commissionModes = ['fixed', 'manual']
selectedMode = merchant.commission_mode OR 'fixed'

ON ModeChange(mode):
    selectedMode = mode
    merchant.commission_mode = mode

    IF mode == 'fixed':
        SHOW FixedCommissionInput
        HIDE ProductSelectionList
    ELSE:
        HIDE FixedCommissionInput
        SHOW ProductSelectionList + BrowseButton

    API.updateAffiliateSettings({ commission_mode: mode })

```

---

### **FL-6: Fixed Commission Logic**

```
ON FixedCommissionChange(value):
    rate = parseInt(value)

    IF rate < 1 OR rate > 99:
        SHOW error "Commission must be between 1% and 99%"
        RETURN

    merchant.fixed_commission_rate = rate

    // Apply to all products
    API.setFixedCommission(merchant.shop_id, rate)

    SHOW success "Commission updated for all products"

```

---

### **FL-7: Manual Product Selection**

```
ON ClickBrowseInventory:
    OPEN ProductSelectionModal

ProductSelectionModal:
    - SearchInput (by title)
    - CollectionFilter (dropdown)
    - ProductList (with checkboxes)
    - SelectedProductsList (with commission input each)
    - SaveButton

ON Search(query):
    products = API.searchProducts(shop_id, query, collectionFilter)
    RENDER ProductList(products)

ON FilterByCollection(collectionId):
    products = API.getProductsByCollection(shop_id, collectionId)
    RENDER ProductList(products)

ON SelectProduct(productId):
    IF productId NOT IN selectedProducts:
        ADD productId to selectedProducts
        SET selectedProducts[productId].commission = null  // Must be set
    ELSE:
        REMOVE productId from selectedProducts

ON CommissionInputChange(productId, value):
    rate = parseInt(value)

    IF rate < 1 OR rate > 99:
        SHOW error "Commission must be between 1% and 99%"
        RETURN

    selectedProducts[productId].commission = rate

ON SaveSelection:
    // Validate all products have commission
    FOR EACH product IN selectedProducts:
        IF product.commission == null:
            SHOW error "All products must have a commission rate"
            RETURN

    response = API.setManualAffiliateProducts(shop_id, selectedProducts)

    IF response.success:
        CLOSE modal
        REFRESH affiliateProductList
        SHOW success "Products updated"
    ELSE:
        SHOW error response.message

```

---

### **FL-8: Remove Product from Affiliate**

```
ON ClickRemoveProduct(productId):
    SHOW confirmDialog("Remove this product from affiliate? Co-sellers will lose access.")

    IF confirmed:
        response = API.removeAffiliateProduct(shop_id, productId)

        IF response.success:
            // Product removed from all Co-seller shops
            REFRESH affiliateProductList
            SHOW success "Product removed"

```

---

### **FL-9: Enable/Disable Toggle**

```
affiliateEnabled = merchant.affiliate_enabled

ON ToggleAffiliate(enabled):
    IF enabled == false:
        SHOW confirmDialog(
            "Disabling affiliate will make all your products 'Out of Stock'
             in Co-seller shops. Sales history will be preserved. Continue?"
        )

        IF NOT confirmed:
            REVERT toggle
            RETURN

    response = API.setAffiliateStatus(shop_id, enabled)

    IF response.success:
        merchant.affiliate_enabled = enabled

        IF enabled:
            SHOW success "Affiliate Network enabled"
        ELSE:
            SHOW success "Affiliate Network disabled"
            // All imported products become Out of Stock
    ELSE:
        REVERT toggle
        SHOW error response.message

```

---

### B4) Behavioral Flow

```
[Merchant Opens Affiliate Network]
    ↓
[Check Role]
    ├─ Co-seller → [Redirect to Marketplace]
    └─ None/Affiliator → [Continue]
        ↓
[Check Activation]
    ├─ Not Activated → [Show Activation Page]
    │                       ↓
    │                  [Click Activate]
    │                       ↓
    │                  [Show Modal]
    │                       ↓
    │                  [Confirm Activate]
    │                       ↓
    │                  [Set Role = Affiliator]
    │                       ↓
    └─ Activated → [Show Settings Page]
                        ↓
    [Configure General Info]
        ↓
    [Select Commission Mode]
        ├─ Fixed → [Set Rate for All]
        └─ Manual → [Select Products + Set Individual Rates]
            ↓
    [Toggle Enable/Disable]
        ↓
    [Products Listed on Marketplace]

```

---

### B5) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Not Activated | Click Activate | Activated (Enabled) | Role = Affiliator, Settings page shown |
| Enabled | Toggle OFF | Disabled | Products = Out of Stock for Co-sellers |
| Disabled | Toggle ON | Enabled | Products available again |
| Fixed Mode | Change to Manual | Manual Mode | Must select products individually |
| Manual Mode | Change to Fixed | Fixed Mode | All products get same rate |
| Product Listed | Remove Product | Product Unlisted | Removed from Co-seller shops |

---

### B6) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Co-seller tries to access settings | Redirect to marketplace |
| **EC-2:** Commission rate < 1 or > 99 | Validation error shown |
| **EC-3:** Manual mode with no products selected | Cannot save (must select at least one) |
| **EC-4:** Product without commission in manual mode | Cannot save (must set rate) |
| **EC-5:** Description exceeds 500 chars | Character limit enforced |
| **EC-6:** Network error during save | Show error, allow retry |
| **EC-7:** Shop currency not USD | Block activation, show error: "Affiliate Network is only available for shops with USD currency" |
| **EC-8:** Disable affiliate with active Co-sellers | Products become Out of Stock |

---

### B7) Data Requirements

**API Endpoints:**

| Endpoint | Method | Purpose |
| --- | --- | --- |
| `/api/affiliate/activate` | POST | Activate affiliate for shop |
| `/api/affiliate/settings` | GET | Get affiliate settings |
| `/api/affiliate/settings` | PATCH | Update settings |
| `/api/affiliate/products` | GET | Get affiliate products |
| `/api/affiliate/products` | POST | Add products to affiliate |
| `/api/affiliate/products/{id}` | DELETE | Remove product |
| `/api/affiliate/status` | PATCH | Enable/disable affiliate |

---

### B8) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | First-time user sees activation page | Activation page with button shown |
| TC-2 | Click activate, confirm modal | Role set to Affiliator |
| TC-3 | Copy shop URL | URL copied to clipboard |
| TC-4 | Select category | Category saved |
| TC-5 | Enter description over 500 chars | Error shown, input truncated |
| TC-6 | Set fixed commission 15% | All products get 15% |
| TC-7 | Set commission 0% | Error: minimum 1% |
| TC-8 | Set commission 100% | Error: maximum 99% |
| TC-9 | Manual mode, select products | Products shown in list |
| TC-10 | Manual mode, save without commission | Error: commission required |
| TC-11 | Remove product | Product removed from marketplace and Co-sellers |
| TC-12 | Disable affiliate | Products become Out of Stock |
| TC-13 | Re-enable affiliate | Products available again |
| TC-14 | Co-seller accesses this page | Redirected to marketplace |
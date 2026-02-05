# Affiliate Marketplace

# Affiliate Marketplace

### Feature ID:

**[IAA-AFF-502]**

### Title:

**Affiliate Marketplace – Browse & Import Products**

### Category:

**Affiliate** | **Actors**: Merchant (Co-seller), Merchant (No Role) | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants who want to sell products without creating inventory need a way to discover and import products from other merchants. The Affiliate Marketplace provides a centralized catalog of products available for resale, with clear commission information and easy import functionality.

**Desired Outcome:**

- Merchants can browse all available affiliate products.
- Filter and search products by various criteria.
- View detailed product information including commission rates.
- View shop profiles with their product listings.
- Import products with one click to start selling.
- Importing first product assigns Co-seller role permanently.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Marketplace Home**
    - Grid/list of all affiliate products
    - Product cards with key information
    - Pagination/infinite scroll
- **Filtering & Search**
    - Search by product title or shop name
    - Filter by product type (Physical, Digital)
    - Filter by price range
    - Filter by commission percentage
    - Filter by shop category
- **Product Detail Page**
    - Full product information (like PDP)
    - Shop info (name, logo)
    - Available quantity
    - Commission percentage
    - Net profit per sale calculation
    - "Add Product" button
- **Shop Page**
    - Shop logo and name
    - Shop category and description
    - List of shop's affiliate products
- **Import Product**
    - One-click import
    - Role assignment for first import
    - Product appears in Co-seller's shop

**Out of Scope:**

- Wishlisting products
- Product reviews/ratings
- Direct messaging to Affiliators
- Bulk import
- Import history log

---

### 3) **Key User Journeys**

---

**Journey 1: Browse Marketplace**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Affiliate Network" → "Marketplace" tab | Marketplace loads |
| 2 | Merchant | Views product grid | All affiliate products displayed |
| 3 | System | Shows product cards | Each card: Image, Shop Name, Logo, Title, Price, Commission % |
| 4 | Merchant | Scrolls through products | More products load (pagination) |

---

**Journey 2: Search & Filter Products**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Enters search term | Filters by product title or shop name |
| 2 | Merchant | Selects product type filter | Shows only Physical or Digital |
| 3 | Merchant | Sets price range | Shows products within range |
| 4 | Merchant | Sets commission range | Shows products with matching commission % |
| 5 | Merchant | Selects category filter | Shows products from shops in that category |
| 6 | System | Updates results | Filtered products displayed |
| 7 | Merchant | Clicks "Clear Filters" | All filters removed, all products shown |

---

**Journey 3: View Product Details**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks on product card | Product detail page opens |
| 2 | System | Displays product info | Images, title, description, price, variants |
| 3 | System | Displays affiliate info | Shop name/logo, quantity, commission %, net profit |
| 4 | Merchant | Reviews net profit calculation | Sees: "You earn $X per sale" |
| 5 | Merchant | Decides to import | Clicks "Add Product" button |

---

**Journey 4: View Shop Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks shop name (on card or product page) | Shop page opens |
| 2 | System | Displays shop profile | Logo, name, category, description |
| 3 | System | Lists shop's products | All affiliate products from this shop |
| 4 | Merchant | Browses shop products | Can click any product for details |
| 5 | Merchant | Can import from shop page | Same import flow |

---

**Journey 5: Import Product (First Time - Becomes Co-seller)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant (No Role) | Clicks "Add Product" | Confirmation shown |
| 2 | System | Shows role warning | "Importing makes you a Co-seller. You cannot become an Affiliator after this." |
| 3 | Merchant | Confirms import | Product imported |
| 4 | System | Sets role to Co-seller | Role locked permanently |
| 5 | System | Adds product to shop | Product appears in Co-seller's storefront |
| 6 | System | Does NOT add to inventory | Imported products are separate |

---

**Journey 6: Import Additional Products (Existing Co-seller)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Clicks "Add Product" | Simple confirmation |
| 2 | Co-seller | Confirms | Product imported |
| 3 | System | Adds to shop | Product available for sale |

---

**Journey 7: Affiliator Browses Marketplace**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Opens Marketplace tab | Marketplace loads |
| 2 | Affiliator | Browses products | Can view all products |
| 3 | Affiliator | Clicks "Add Product" | Error: "Affiliators cannot import products" |
| 4 | System | Blocks import | Action not allowed |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Marketplace Display** |  |
| BAC-1 | Marketplace shows all active affiliate products |
| BAC-2 | Product cards display: Image, Shop Name, Shop Logo, Title, Price, Commission % |
| BAC-3 | Products are paginated or infinite scroll |
| BAC-4 | Only products from enabled Affiliators are shown |
| **Search & Filter** |  |
| BAC-5 | Search works for product title AND shop name |
| BAC-6 | Filter by product type: Physical, Digital |
| BAC-7 | Filter by price range (min-max) |
| BAC-8 | Filter by commission percentage range |
| BAC-9 | Filter by shop category |
| BAC-10 | Multiple filters can be combined |
| BAC-11 | Clear filters option available |
| **Product Detail** |  |
| BAC-12 | Product page shows full product info (like PDP) |
| BAC-13 | Shop name and logo are displayed |
| BAC-14 | Available quantity is shown |
| BAC-15 | Commission percentage is displayed |
| BAC-16 | Net profit per sale is calculated and shown |
| BAC-17 | "Add Product" button is prominent |
| **Shop Page** |  |
| BAC-18 | Shop page shows logo, name, category, description |
| BAC-19 | All shop's affiliate products are listed |
| BAC-20 | Can navigate to individual products |
| **Import** |  |
| BAC-21 | First import shows role warning and confirmation |
| BAC-22 | First import sets role to Co-seller permanently |
| BAC-23 | Subsequent imports require simple confirmation |
| BAC-24 | Affiliators cannot import (button disabled/error) |
| BAC-25 | Imported products do NOT appear in inventory |
| BAC-26 | Imported products appear in storefront |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Marketplace** | Central catalog of all affiliate products from all Affiliators |
| **Product Card** | Compact display of product with key info |
| **Net Profit** | Commission amount Co-seller earns per sale |
| **Import** | Action of adding affiliate product to Co-seller's shop |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Load Marketplace**

```
ON NavigateTo('/affiliate-network/marketplace'):
    merchant = getCurrentMerchant()

    // Check currency
    IF merchant.shop_currency != 'USD':
        SHOW warning "Your shop currency must be USD to import products"
        SET canImport = false
    ELSE:
        SET canImport = merchant.affiliate_role != 'affiliator'

    // Load products
    products = API.getMarketplaceProducts({
        page: 1,
        limit: 20,
        filters: defaultFilters
    })

    RENDER MarketplacePage(products, canImport)

```

---

### **FL-2: Product Card Rendering**

```
FUNCTION renderProductCard(product):
    RETURN {
        image: product.primary_image OR defaultImage,
        shopLogo: product.shop.logo_url OR defaultLogo,
        shopName: product.shop.name,
        title: product.title,
        price: formatCurrency(product.price, 'USD'),
        commission: product.commission_rate + '%',
        onClick: navigateToProduct(product.id),
        onShopClick: navigateToShop(product.shop.id)
    }

```

---

### **FL-3: Search Logic**

```
ON SearchChange(query):
    DEBOUNCE 300ms:
        // Search in both product title and shop name
        results = API.searchMarketplace({
            query: query,
            filters: currentFilters
        })

        RENDER productGrid(results)

```

---

### **FL-4: Filter Logic**

```
filters = {
    productType: null,      // 'physical', 'digital', or null
    priceMin: null,
    priceMax: null,
    commissionMin: null,
    commissionMax: null,
    category: null
}

ON FilterChange(filterType, value):
    filters[filterType] = value

    products = API.getMarketplaceProducts({
        page: 1,
        limit: 20,
        filters: filters,
        search: currentSearch
    })

    RENDER productGrid(products)

ON ClearFilters:
    filters = {
        productType: null,
        priceMin: null,
        priceMax: null,
        commissionMin: null,
        commissionMax: null,
        category: null
    }
    currentSearch = ""

    products = API.getMarketplaceProducts({ page: 1, limit: 20 })
    RENDER productGrid(products)

```

---

### **FL-5: Product Detail Page**

```
ON NavigateTo('/marketplace/product/{id}'):
    product = API.getMarketplaceProduct(id)

    IF product == null OR product.affiliate_active == false:
        SHOW error "Product not available"
        REDIRECT to '/affiliate-network/marketplace'
        RETURN

    // Calculate net profit
    netProfit = product.price * (product.commission_rate / 100)

    productDetail = {
        // Standard PDP fields
        images: product.images,
        title: product.title,
        description: product.description,
        price: product.price,
        variants: product.variants,

        // Affiliate-specific fields
        shop: {
            id: product.shop.id,
            name: product.shop.name,
            logo: product.shop.logo_url
        },
        quantity: product.available_quantity,
        commissionRate: product.commission_rate,
        netProfit: netProfit,
        netProfitFormatted: "You earn $" + netProfit.toFixed(2) + " per sale"
    }

    RENDER ProductDetailPage(productDetail)

```

---

### **FL-6: Shop Page**

```
ON NavigateTo('/marketplace/shop/{id}'):
    shop = API.getMarketplaceShop(id)

    IF shop == null OR shop.affiliate_enabled == false:
        SHOW error "Shop not available"
        REDIRECT to '/affiliate-network/marketplace'
        RETURN

    products = API.getShopAffiliateProducts(id)

    shopPage = {
        logo: shop.logo_url,
        name: shop.name,
        category: shop.affiliate_category,
        description: shop.affiliate_description,
        products: products
    }

    RENDER ShopPage(shopPage)

```

---

### **FL-7: Import Product**

```
ON ClickAddProduct(productId):
    merchant = getCurrentMerchant()

    // Check if Affiliator
    IF merchant.affiliate_role == 'affiliator':
        SHOW error "Affiliators cannot import products. You can only sell your own products."
        RETURN

    // Check currency
    IF merchant.shop_currency != 'USD':
        SHOW error "Your shop currency must be USD to import products."
        RETURN

    // Check if already imported
    IF API.isProductImported(merchant.shop_id, productId):
        SHOW error "You have already imported this product."
        RETURN

    // First-time import warning
    IF merchant.affiliate_role == null:
        SHOW confirmDialog({
            title: "Become a Co-seller",
            message: "By importing this product, you will become a Co-seller.
                      This is permanent - you will not be able to become an
                      Affiliator or list your own products on the marketplace.

                      Do you want to continue?",
            confirmText: "Yes, Import Product",
            cancelText: "Cancel"
        })

        IF NOT confirmed:
            RETURN
    ELSE:
        // Simple confirmation for existing Co-sellers
        SHOW confirmDialog({
            title: "Import Product",
            message: "Add this product to your shop?",
            confirmText: "Import",
            cancelText: "Cancel"
        })

        IF NOT confirmed:
            RETURN

    // Perform import
    SET loading = true

    response = API.importProduct({
        product_id: productId,
        coseller_shop_id: merchant.shop_id
    })

    IF response.success:
        // Update role if first import
        IF merchant.affiliate_role == null:
            merchant.affiliate_role = 'co-seller'
            merchant.coseller_activated_at = NOW

        SHOW success "Product imported successfully! It's now available in your shop."

        // Update button state
        SET buttonState = 'imported'
    ELSE:
        SHOW error response.message

    SET loading = false

```

---

### **FL-8: Check Already Imported**

```
ON ProductDetailLoad(productId):
    merchant = getCurrentMerchant()

    IF merchant.affiliate_role == 'affiliator':
        SET addButtonState = 'disabled'
        SET addButtonText = "Affiliators Cannot Import"
    ELSE IF API.isProductImported(merchant.shop_id, productId):
        SET addButtonState = 'disabled'
        SET addButtonText = "Already Imported"
    ELSE:
        SET addButtonState = 'enabled'
        SET addButtonText = "Add Product"

```

---

### B3) Behavioral Flow

```
[Merchant Opens Marketplace]
    ↓
[Check Currency]
    ├─ Not USD → [Show Warning, Disable Import]
    └─ USD → [Continue]
        ↓
[Load All Affiliate Products]
    ↓
[Display Product Grid]
    │
    ├─ [Search] → [Filter by Title/Shop Name]
    ├─ [Filter by Type] → [Physical/Digital]
    ├─ [Filter by Price] → [Min-Max Range]
    ├─ [Filter by Commission] → [Min-Max Range]
    └─ [Filter by Category] → [Shop Category]
        ↓
[Click Product Card]
    ↓
[Product Detail Page]
    ├─ Shows PDP-like info
    ├─ Shows Shop Info
    ├─ Shows Commission & Net Profit
    └─ [Add Product Button]
        │
        ├─ Affiliator → [Blocked]
        ├─ Already Imported → [Disabled]
        ├─ First Import → [Role Warning + Confirm]
        └─ Existing Co-seller → [Simple Confirm]
            ↓
[Product Imported]
    ↓
[Available in Co-seller's Shop]

```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Browsing | Click product | Product Detail | Full info displayed |
| Product Detail | Click shop name | Shop Page | Shop profile shown |
| Shop Page | Click product | Product Detail | Product info shown |
| No Role + Product Detail | Click Add | Co-seller | Product imported, role set |
| Co-seller + Product Detail | Click Add | Co-seller | Product imported |
| Affiliator + Product Detail | Click Add | Affiliator | Action blocked |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Product becomes unavailable while viewing | Show error, redirect to marketplace |
| **EC-2:** Shop disables affiliate while on shop page | Show error, redirect to marketplace |
| **EC-3:** Try to import same product twice | Show "Already imported" |
| **EC-4:** Affiliator clicks Add | Show "Affiliators cannot import" |
| **EC-5:** Non-USD shop tries to import | Show currency error |
| **EC-6:** Network error during import | Show error, allow retry |
| **EC-7:** Product sold out after page load | Show out of stock on import attempt |
| **EC-8:** No products match filters | Show empty state message |

---

### B6) Data Requirements

**API Endpoints:**

| Endpoint | Method | Purpose |
| --- | --- | --- |
| `/api/marketplace/products` | GET | List products with filters |
| `/api/marketplace/products/{id}` | GET | Get product detail |
| `/api/marketplace/shops/{id}` | GET | Get shop profile |
| `/api/marketplace/shops/{id}/products` | GET | Get shop's products |
| `/api/marketplace/import` | POST | Import product to shop |
| `/api/marketplace/check-imported` | GET | Check if product already imported |

**Query Parameters for Product List:**

| Parameter | Type | Description |
| --- | --- | --- |
| page | Integer | Page number |
| limit | Integer | Products per page |
| search | String | Search query |
| type | Enum | physical, digital |
| price_min | Float | Minimum price |
| price_max | Float | Maximum price |
| commission_min | Integer | Minimum commission % |
| commission_max | Integer | Maximum commission % |
| category | String | Shop category |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Load marketplace | Products displayed in grid |
| TC-2 | Search by product title | Matching products shown |
| TC-3 | Search by shop name | Products from matching shops shown |
| TC-4 | Filter by Physical type | Only physical products shown |
| TC-5 | Filter by price range | Products in range shown |
| TC-6 | Filter by commission range | Products in range shown |
| TC-7 | Combine multiple filters | All filters applied |
| TC-8 | Clear filters | All products shown |
| TC-9 | View product detail | Full info + affiliate info shown |
| TC-10 | View shop page | Shop profile + products shown |
| TC-11 | First import (no role) | Role warning shown, becomes Co-seller |
| TC-12 | Import as Co-seller | Simple confirm, product added |
| TC-13 | Affiliator tries import | Error shown, blocked |
| TC-14 | Import already imported product | Error: already imported |
| TC-15 | Non-USD shop tries import | Currency error shown |
| TC-16 | Net profit calculation | Correctly shows commission amount |
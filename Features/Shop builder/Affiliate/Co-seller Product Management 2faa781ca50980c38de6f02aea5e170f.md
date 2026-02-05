# Co-seller Product Management

# Co-seller Product Management

### Feature ID:

**[IAA-AFF-503]**

### Title:

**Co-seller Product Management – Imported Products**

### Category:

**Affiliate** | **Actors**: Merchant (Co-seller), Customer | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Co-sellers need to manage the products they've imported from the Affiliate Marketplace. Unlike regular products, imported products cannot be edited - they are controlled by the Affiliator. Co-sellers need visibility into their imported products and the ability to remove them if needed.

**Desired Outcome:**

- Imported products display in Co-seller's storefront automatically.
- Imported products do NOT appear in inventory/product list.
- Co-seller cannot edit any aspect of imported products.
- Changes by Affiliator (price, commission, stock) reflect immediately.
- Co-seller can remove imported products from their shop.
- Customers can purchase imported products normally.
- Coupon codes cannot be applied to carts with imported products.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Storefront Display**
    - Imported products appear in shop's PLP
    - Imported products have functional PDP
    - Same display as regular products (no special indicator for customers)
- **Automatic Sync**
    - Price changes sync from Affiliator
    - Commission changes sync from Affiliator
    - Stock/quantity syncs from Affiliator
    - Product removal by Affiliator removes from Co-seller
    - Affiliator disable makes products "Out of Stock"
- **Co-seller Controls**
    - View list of imported products (in dashboard)
    - Remove imported product from shop
- **Purchase Flow**
    - Customers add to cart normally
    - No coupon codes on affiliate carts
    - Order processed with commission split

**Out of Scope:**

- Editing imported product details
- Setting custom prices
- Managing inventory
- Bulk import/remove operations
- Product display order customization

---

### 3) **Key User Journeys**

---

**Journey 1: Customer Views Imported Product**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Opens Co-seller's shop | PLP loads |
| 2 | Customer | Sees imported product | Displays like any other product |
| 3 | Customer | Clicks product | PDP loads with full details |
| 4 | Customer | Views price, description, images | All from Affiliator |
| 5 | Customer | Adds to cart | Product added normally |

---

**Journey 2: Customer Purchases Imported Product**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Has imported product in cart | Checkout available |
| 2 | Customer | Tries to apply coupon code | Error: "Coupon cannot be applied to this order" |
| 3 | Customer | Proceeds to checkout | Normal checkout flow |
| 4 | Customer | Completes payment | Order created |
| 5 | System | Splits payment | Co-seller gets commission, Affiliator gets remainder |
| 6 | System | Creates orders | Order appears in both dashboards |

---

**Journey 3: Affiliator Changes Price**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Updates product price | Price saved |
| 2 | System | Syncs to all Co-sellers | Price updates immediately |
| 3 | Customer | Views product on Co-seller's shop | New price displayed |
| 4 | System | Commission recalculated | Based on new price |

---

**Journey 4: Affiliator Changes Commission**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Updates commission rate | New rate saved |
| 2 | System | Syncs to all Co-sellers | Commission updates |
| 3 | Co-seller | Views dashboard | New commission % shown |
| 4 | System | Future sales use new rate | Historical data unchanged |

---

**Journey 5: Affiliator Removes Product or Sets Stock to Zero**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Deletes product OR sets stock = 0 | Product unavailable |
| 2 | System | Removes from all Co-sellers | Product disappears from shops |
| 3 | Co-seller | Views shop | Product no longer listed |
| 4 | Customer | Direct link to product | 404 or "Not Available" |

---

**Journey 6: Affiliator Disables Affiliate**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Toggles affiliate OFF | Affiliate disabled |
| 2 | System | Updates all imported products | Status = "Out of Stock" |
| 3 | Customer | Views product on Co-seller's shop | Shows "Out of Stock" |
| 4 | Customer | Cannot add to cart | Button disabled |
| 5 | Affiliator | Re-enables affiliate | Products available again |

---

**Journey 7: Co-seller Removes Imported Product**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Opens Affiliate Dashboard | Imported products listed |
| 2 | Co-seller | Finds product to remove | Remove option available |
| 3 | Co-seller | Clicks Remove | Confirmation dialog |
| 4 | Co-seller | Confirms | Product removed from shop |
| 5 | System | Deletes import record | Product no longer in storefront |
| 6 | Co-seller | Can re-import later | From marketplace |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Display** |  |
| BAC-1 | Imported products appear in Co-seller's PLP like regular products |
| BAC-2 | Imported products have functional PDP with all Affiliator's content |
| BAC-3 | No visual indicator to customer that product is imported |
| BAC-4 | Imported products do NOT appear in Co-seller's inventory/product list |
| **Sync** |  |
| BAC-5 | Price changes by Affiliator reflect immediately in Co-seller's shop |
| BAC-6 | Commission changes reflect immediately for future sales |
| BAC-7 | Stock changes reflect immediately |
| BAC-8 | Product deletion by Affiliator removes from all Co-seller shops |
| BAC-9 | Affiliator disable makes products "Out of Stock" |
| **Controls** |  |
| BAC-10 | Co-seller CANNOT edit any product details |
| BAC-11 | Co-seller CAN remove imported product |
| BAC-12 | Removed product can be re-imported from marketplace |
| **Purchase** |  |
| BAC-13 | Customers can add imported products to cart normally |
| BAC-14 | Coupon codes are BLOCKED on carts containing imported products |
| BAC-15 | Checkout flow works normally |
| BAC-16 | Payment is split: Commission to Co-seller, remainder to Affiliator |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Exhaustive Functional Logic

---

### **FL-1: PLP Display for Imported Products**

```
ON LoadShopPLP(shop_id):
    // Get regular products
    regularProducts = API.getShopProducts(shop_id, { status: 'public' })

    // Get imported products
    importedProducts = API.getImportedProducts(shop_id)

    // Filter imported: only active and in-stock
    activeImports = importedProducts.filter(p =>
        p.affiliate_active == true AND
        p.affiliator_enabled == true AND
        p.quantity > 0
    )

    // Merge and display
    allProducts = regularProducts.concat(activeImports)

    RENDER ProductGrid(allProducts)

```

---

### **FL-2: PDP for Imported Products**

```
ON LoadProductPage(shop_id, product_id):
    // Check if imported product
    importRecord = DB.findImport(shop_id, product_id)

    IF importRecord:
        // Load from Affiliator's data
        product = API.getAffiliateProduct(importRecord.affiliate_product_id)

        IF product == null OR product.affiliate_active == false:
            RETURN 404

        IF product.affiliator_enabled == false:
            RENDER OutOfStockPage(product)
            RETURN

        IF product.quantity == 0:
            RENDER OutOfStockPage(product)
            RETURN

        RENDER ProductDetailPage(product, { isImported: true })
    ELSE:
        // Regular product
        product = API.getProduct(product_id)
        RENDER ProductDetailPage(product, { isImported: false })

```

---

### **FL-3: Add to Cart Logic**

```
ON AddToCart(shop_id, product_id, quantity):
    importRecord = DB.findImport(shop_id, product_id)

    IF importRecord:
        // Mark cart item as affiliate
        cartItem = {
            product_id: product_id,
            quantity: quantity,
            is_affiliate: true,
            affiliate_product_id: importRecord.affiliate_product_id,
            affiliator_shop_id: importRecord.affiliator_shop_id,
            commission_rate: importRecord.commission_rate
        }
    ELSE:
        cartItem = {
            product_id: product_id,
            quantity: quantity,
            is_affiliate: false
        }

    ADD cartItem to cart
    RETURN success

```

---

### **FL-4: Coupon Validation**

```
ON ApplyCoupon(cart, coupon_code):
    // Check if cart has any affiliate products
    hasAffiliateProducts = cart.items.some(item => item.is_affiliate == true)

    IF hasAffiliateProducts:
        RETURN {
            success: false,
            error: "Coupon codes cannot be applied to orders containing affiliate products."
        }

    // Normal coupon validation
    CONTINUE normalCouponValidation(cart, coupon_code)

```

---

### **FL-5: Price Sync Logic**

```
// Triggered when Affiliator updates product price
ON ProductPriceUpdate(product_id, new_price):
    // Update source product
    UPDATE products SET price = new_price WHERE id = product_id

    // All imported instances automatically reflect this
    // because they reference the same product data

    // No need to update import records - they reference live data

```

---

### **FL-6: Commission Sync Logic**

```
// Triggered when Affiliator updates commission
ON CommissionUpdate(affiliate_product_id, new_rate):
    UPDATE affiliate_products
    SET commission_rate = new_rate
    WHERE id = affiliate_product_id

    // Future orders will use new rate
    // Historical orders are not affected

```

---

### **FL-7: Product Removal by Affiliator**

```
ON AffiliatorDeleteProduct(product_id):
    // Mark affiliate product as inactive
    UPDATE affiliate_products
    SET is_active = false
    WHERE product_id = product_id

    // Remove all imports
    DELETE FROM imported_products
    WHERE affiliate_product_id IN (
        SELECT id FROM affiliate_products WHERE product_id = product_id
    )

    // Products immediately disappear from all Co-seller shops

```

---

### **FL-8: Affiliator Disable**

```
ON AffiliatorToggleDisable(shop_id):
    UPDATE shop_affiliate_settings
    SET enabled = false
    WHERE shop_id = shop_id

    // All imported products become "Out of Stock"
    // They're not deleted - just not purchasable

    // When querying imported products:
    // Check affiliator_enabled flag

```

---

### **FL-9: Co-seller Remove Product**

```
ON CosellerRemoveImport(import_id):
    SHOW confirmDialog("Remove this product from your shop?")

    IF confirmed:
        DELETE FROM imported_products WHERE id = import_id

        SHOW success "Product removed from your shop"

        // Co-seller can re-import from marketplace anytime

```

---

### B2) Behavioral Flow

```
[Imported Product in Co-seller Shop]
    ↓
[Customer Views Product]
    ↓
[PDP Shows Affiliator's Data]
    ↓
[Add to Cart]
    ↓
[Try Coupon?]
    ├─ Yes → [BLOCKED - Has Affiliate Products]
    └─ No → [Continue]
        ↓
[Checkout]
    ↓
[Payment Successful]
    ↓
[Split Payment]
    ├─ Co-seller → [Commission % as Credit]
    └─ Affiliator → [(Price - Commission) as Credit]
        ↓
[Order Created for Both Parties]

```

---

### B3) State Transitions

| Current State | Event | New State | Effect on Co-seller |
| --- | --- | --- | --- |
| Active | Affiliator changes price | Active (new price) | Price updates in shop |
| Active | Affiliator changes commission | Active (new commission) | Future earnings change |
| Active | Affiliator sets stock = 0 | Removed | Product disappears |
| Active | Affiliator deletes product | Removed | Product disappears |
| Active | Affiliator disables affiliate | Out of Stock | Product not purchasable |
| Out of Stock | Affiliator enables affiliate | Active | Product purchasable again |
| Active | Co-seller removes | Removed | Product disappears |
| Removed | Co-seller re-imports | Active | Product appears again |

---

### B4) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Price changes while product in cart | Use price at checkout time |
| **EC-2:** Commission changes while product in cart | Use commission at order creation |
| **EC-3:** Product deleted while in cart | Remove from cart, notify customer |
| **EC-4:** Affiliator disables while product in cart | Show "Out of Stock" at checkout |
| **EC-5:** Customer tries direct link to removed product | 404 page |
| **EC-6:** Co-seller tries to edit imported product | No edit UI shown |
| **EC-7:** Mixed cart (affiliate + regular) with coupon | Coupon blocked |
| **EC-8:** Cart only has regular products with coupon | Coupon works |

---

### B5) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | View imported product on PLP | Displayed normally |
| TC-2 | View imported product PDP | Full details shown |
| TC-3 | Check inventory for imported product | Not in inventory list |
| TC-4 | Affiliator changes price | Price updates in Co-seller shop |
| TC-5 | Affiliator changes commission | Dashboard shows new rate |
| TC-6 | Affiliator deletes product | Removed from Co-seller shop |
| TC-7 | Affiliator sets stock to 0 | Product removed |
| TC-8 | Affiliator disables affiliate | Products show Out of Stock |
| TC-9 | Co-seller removes import | Product removed from shop |
| TC-10 | Apply coupon to affiliate cart | Coupon rejected |
| TC-11 | Apply coupon to regular cart | Coupon works |
| TC-12 | Complete purchase | Payment split correctly |
| TC-13 | Try to edit imported product | No edit option available |
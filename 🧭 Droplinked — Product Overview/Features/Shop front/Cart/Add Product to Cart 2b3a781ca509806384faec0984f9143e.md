# Add Product to Cart

**Feature ID:** STF-CART-ADD-201

**Category:** Storefront | Cart System

**Actors:** Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Customers must be able to add products to their cart based on SKU selection, quantity, stock availability, product visibility, and release date. The system must handle all cart creation/update logic automatically and prevent invalid add-to-cart actions.

### **Desired Outcome**

- Customer can select SKU + quantity (or just quantity for products without SKUs) on PDP and add items to cart.
- Cart is created automatically if it doesn't exist.
- If the same SKU/product already exists in cart, quantities merge.
- Customer cannot add products that are:
    - Out of stock (unless unlimited stock is enabled)
    - **Unlisted** or **Draft** status
    - Not yet released (Scheduled Release with future date)
    - **Zero or no price set**
- Cart icon in the header shows the **total number of products (sum of quantities)**.

---

# **2) Scope – In / Out**

### **In Scope**

- Adding product to cart with selected SKU + quantity (or product + quantity for products without SKUs).
- Auto-creating a cart if none exists.
- Updating existing cart lines when the same SKU/product is added again.
- **Quantity Selection Logic:**
    - Customer can select quantity before adding to cart
    - Default quantity: 1
    - Limited stock: "Out of Stock" when quantity=0 or selected qty > available stock
    - Unlimited stock (`continue_selling=true`): Never shows "Out of Stock"
- **Stock Validation:**
    - Limited inventory: Real-time validation (quantity ≤ available stock)
    - Unlimited inventory: No validation, always available
    - SKUs with 0 quantity cannot be added
- **Product Status Validation:**
    - **Public**: Full add to cart functionality
    - **Scheduled Release**: Product visible in PLP and PDP, but Add to Cart button hidden and API blocked
    - **Unlisted/Draft**: 404 error, cannot access or add to cart
- **Price Validation** (products with zero or no price cannot be added).
- First SKU pre-selected by default for products with variants.
- Color variants displayed as colored square swatches using merchant's hex color codes.
- Updating cart item count in the header.

### **Out of Scope**

- Cart UI display (separate feature).
- Checkout flow.
- Promotions, discounts, or coupons.
- Multi-cart support.

---

# **3) Key User Journeys**

---

### **Journey 1 – Add First Product to Cart**

**Step 1:** Customer selects SKU + quantity on PDP.

**Step 2:** Customer clicks **Add to Cart**.

**Step 3:** Cart is empty → system creates a new cart.

**Step 4:** Product (SKU) is added with selected quantity.

**Step 5:** Cart icon updates with total quantity.

---

### **Journey 2 – Add Additional Product**

**Step 1:** Customer selects SKU + quantity.

**Step 2:** Cart already has items.

**Step 3:** New product is added as a new line item.

**Step 4:** Cart icon count updates accordingly.

---

### **Journey 3 – Add Same Product/SKU Again**

**Step 1:** Customer selects SKU that already exists in cart.

**Step 2:** Customer enters new quantity.

**Step 3:** System checks inventory availability.

**Step 4:** If enough stock → quantity is **incremented**, not duplicated.

**Step 5:** Cart icon updates.

---

### **Journey 4 – Prevent Invalid Add-to-Cart**

**Scenario A:** SKU/Product inventory < requested quantity (and unlimited stock is disabled)

→ Add to Cart is disabled or error message shown.

**Scenario B:** Product release date is in the future (Scheduled Release)

→ Add to Cart disabled; customer cannot purchase early.

**Scenario C:** Product visibility = **Unlisted** or **Draft**

→ Product is not accessible; cannot be added to cart.

**Scenario D:** Product price is zero or not set

→ Add to Cart button is disabled; customer cannot add product to cart.

---

### **Journey 5 – Add Product Without SKU**

**Step 1:** Customer views a product that has no SKU variants.

**Step 2:** Customer selects quantity.

**Step 3:** Customer clicks **Add to Cart**.

**Step 4:** Product is added to cart with selected quantity (no SKU reference).

**Step 5:** Cart icon updates with total quantity.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Customer can add a product with chosen SKU + quantity to cart, and a new cart is created if none exists.

**BAC 2:** If the same SKU exists, the system increments quantity instead of creating duplicate line items.

**BAC 3:** System prevents adding products when requested quantity exceeds inventory.

**BAC 4:** Products with future release dates cannot be added to cart.

**BAC 5:** Products with visibility = **Unlisted** or **Draft** cannot be added to cart.

**BAC 6:** Cart icon in the header shows **sum of all quantities** across cart items.

**BAC 7:** Adding a product updates cart state instantly.

**BAC 8:** Products with **zero or no price** cannot be added to cart; Add to Cart button must be disabled.

**BAC 9:** Products **without SKUs** can be added to cart using product-level quantity; system handles them the same as SKU-based products.

**BAC 10:** Stock validation must respect the **"Continue selling when out of stock"** setting; if enabled, unlimited stock is assumed.

**BAC 11:** First SKU must be **pre-selected by default** when PDP loads for products with SKUs.

**BAC 12:** Color variants must display as **colored square swatches** using exact hex color codes selected by merchant.

**BAC 13:** Products with **Scheduled Release** status are displayed in PLP and accessible on PDP, but **Add to Cart button is hidden** and **API add to cart is blocked**.

**BAC 14:** Products with **limited stock** (quantity tracking enabled) must show "Out of Stock" and disable Add to Cart when quantity=0 or selected quantity exceeds available stock.

**BAC 15:** Products with **`continue_selling=true`** (unlimited stock) must never show "Out of Stock" and always allow adding to cart.

---

# **5) Detailed Logic**

## **Quantity Selection Flow**

```
Customer Opens PDP
    ↓
[Load Product]
    ├─ Check Status
    │   ├─ Draft/Unlisted → 404 Error
    │   ├─ Scheduled Release → Show PDP, Hide Add to Cart
    │   └─ Public → Full Functionality
    ↓
[For Products WITH SKUs]
    ├─ First SKU auto-selected by default
    ├─ Display color variants as hex-colored squares
    └─ Customer can select different SKU
    ↓
[Quantity Selection]
    ├─ Default: 1
    ├─ Customer can increment/decrement
    └─ Validation based on stock type:
        ├─ Limited Stock → Check against available quantity
        └─ Unlimited Stock (continue_selling) → No limit
    ↓
[Stock Validation]
    ├─ Limited Stock:
    │   ├─ quantity ≤ available → Enable Add to Cart
    │   └─ quantity > available → Show "Out of Stock", Disable button
    └─ Unlimited Stock → Always enabled
    ↓
[Price Validation]
    ├─ Price > 0 → Enable Add to Cart
    └─ Price = 0 or null → Show "$0", Disable Add to Cart
    ↓
[Add to Cart Clicked]
    ↓
[API Validation]
    ├─ Re-verify product status
    ├─ Re-verify stock (limited products)
    ├─ Re-verify price > 0
    └─ Add to cart or return error
```

## **Stock Types**

### **Limited Stock (track_inventory = true)**

| Scenario | UI Behavior | API Behavior |
| --- | --- | --- |
| quantity > 0 | Show quantity selector, Enable Add to Cart | Allow add to cart |
| quantity = 0 | Show "Out of Stock", Disable Add to Cart | Block with error |
| selected_qty > available | Show "Out of Stock"/"Insufficient stock", Disable | Block with error |

### **Unlimited Stock (continue_selling = true)**

| Scenario | UI Behavior | API Behavior |
| --- | --- | --- |
| Any quantity | No "Out of Stock" message, Always enabled | Always allow |
| quantity = 0 | Still allow purchase, No stock message | Allow add to cart |

## **Product Status Matrix**

| Status | PLP Visibility | PDP Accessibility | Add to Cart | API Cart |
| --- | --- | --- | --- | --- |
| **Public** | Visible | Full access | Enabled | Allowed |
| **Scheduled Release** | Visible | Accessible | Hidden | Blocked |
| **Unlisted** | Hidden | 404/Redirect | N/A | Blocked |
| **Draft** | Hidden | 404/Redirect | N/A | Blocked |

## **Error Messages**

| Error Condition | Message |
| --- | --- |
| Out of Stock | "This item is out of stock" |
| Insufficient Stock | "Only X items available" |
| Scheduled Release | "This product is not yet available for purchase" |
| Draft/Unlisted | 404 Page |
| Zero Price | "This product cannot be added to cart" |
| API - Product Not Found | "Product not found or unavailable" |
| API - Insufficient Stock | "Not enough stock available" |
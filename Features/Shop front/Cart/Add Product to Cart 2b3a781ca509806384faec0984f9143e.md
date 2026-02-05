# Add Product to Cart

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
- Validating stock before adding items (respecting unlimited stock setting).
- Verifying product visibility rules (**Unlisted/Draft** products cannot be added).
- Verifying product release date (**Scheduled Release** with future date cannot be added).
- **Validating product price** (products with zero or no price cannot be added).
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
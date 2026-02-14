# Product Detail Page (PDP)

# Product Detail Page (PDP)

**Feature ID:** IAA-PDP-101

**Category:** Storefront | **Actors:** Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Customers need a detailed view of a product before making a purchase. The PDP must provide all necessary product information, allow SKU selection, quantity management, and enable adding products to the cart. For physical and digital products, certain attributes must be displayed appropriately, and blockchain-verified products should provide access to ledger verification.

### **Desired Outcome**

- Display product images as a slider with zoom functionality (or **default placeholder image** if product has no images).
- Show product title, description, and price (SKU price if SKUs exist, or product-level price if no SKUs).
- For products **with SKUs**: Allow SKU selection and adjust displayed price accordingly.
  - **First SKU is selected by default** when the page loads.
  - **Color variants**: When merchant defines color variants using color picker, the exact hex color code is displayed as a colored square swatch on the PDP.
- For products **without SKUs**: Display product-level price only (no SKU selector).
- **Quantity selection**: Customer can select quantity before adding to cart.
  - If product/SKU has **limited stock** and quantity is 0 or selected quantity exceeds available stock → Show "Out of Stock" and disable Add to Cart.
  - If product has **unlimited stock** (`continue_selling=true`) → Customer can select any quantity, "Out of Stock" is never shown.
- Products with **zero or no price**: Display "$0" and **disable Add to Cart button**.
- **Product Status Handling**:
  - **Unlisted** or **Draft**: Return 404 or redirect to shop home (not accessible even via direct link).
  - **Scheduled Release**: Product page is accessible but **Add to Cart button is hidden** and **API add to cart is blocked**.
  - **Public**: Full functionality including Add to Cart.
- For recorded/verified products, display blockchain verification link.
- For physical products, hide Shipping and Size if irrelevant.
- Enable adding product to cart based on selected SKU/product and quantity (only if price > 0).

---

# **2) Scope – In / Out**

### **In Scope**

- Product image slider with zoom (or **default placeholder image** if no images exist).
- Product title display.
- Product description display (hidden if no description).
- **Product visibility enforcement**:
  - Unlisted/Draft products return 404 or redirect.
  - Scheduled Release products display the page but hide Add to Cart button and block API cart addition.
- **Products with SKUs**: SKU selection with **first SKU pre-selected by default**; SKU-based price display; **Color variants display as colored square swatches** using the exact hex color code selected by merchant.
- **Products without SKUs**: Display product-level price only; no SKU selector shown.
- Quantity selection (increment/decrement).
- "Out of Stock" handling for SKUs/products with 0 quantity.
- **Zero/No price handling**: Display "$0" and disable Add to Cart button.
- Blockchain verification section for recorded products with external link.
- Add to Cart button functional based on SKU/product and quantity (disabled if price ≤ 0).
- Conditional display of Shipping and Size for physical products.

### **Out of Scope**

- Product reviews or ratings.
- Recommendations or upsells.
- Dynamic price calculation based on promotions (handled separately).
- Multi-language content.
- Wishlist functionality.

---

# **3) Key User Journeys**

### **Journey 1 – View Product Details**

As a **Customer**, I can view all relevant details of a product.

**Step 1:** Customer clicks a product on PLP or accesses PDP via direct link.

**Step 2:** System checks product visibility status:

- If **Unlisted** or **Draft** → return 404 or redirect to shop home.
- If **Public** or **Scheduled Release** → proceed to display.

**Step 3:** PDP loads with product images in a slider with zoom. If no images exist, a **default placeholder image** is displayed.

**Step 4:** Product title, price, and description (if exists) are displayed.

- If product has SKUs → default SKU price shown.
- If product has no SKUs → product-level price shown.
- If price is zero or not set → "$0" displayed.

**Step 5:** If the product is physical, Shipping and Size may be shown if relevant; otherwise, hidden.

---

### **Journey 2 – Select SKU and Quantity (Products WITH SKUs)**

As a **Customer**, I can choose a SKU and quantity before adding to cart.

**Step 1:** Customer sees SKU options and selects a SKU from available options.

**Step 2:** Price updates based on selected SKU.

**Step 3:** Customer adjusts quantity using increment/decrement buttons.

**Step 4:** If selected SKU has 0 quantity, "Out of Stock" disables Add to Cart.

---

### **Journey 2B – Select Quantity (Products WITHOUT SKUs)**

As a **Customer**, I can choose quantity for a product that has no variants.

**Step 1:** Customer views product with no SKU options (no variant selector displayed).

**Step 2:** Product-level price is displayed.

**Step 3:** Customer adjusts quantity using increment/decrement buttons.

**Step 4:** If product has 0 quantity (and unlimited stock is disabled), "Out of Stock" disables Add to Cart.

---

### **Journey 3 – Blockchain Verification**

As a **Customer**, I can verify recorded products on the blockchain.

**Step 1:** PDP shows a blockchain verification section for recorded products.

**Step 2:** Customer clicks the link → opens external ledger in a new tab.

---

### **Journey 4 – Add Product to Cart**

As a **Customer**, I can add products to the cart based on selected SKU/product and quantity.

**Step 1:** Customer selects SKU (if applicable) and quantity.

**Step 2:** System validates:

- Price must be greater than zero.
- Stock must be available (unless unlimited stock enabled).

**Step 3:** If valid → Customer clicks **Add to Cart** → Product is added to cart.

**Step 4:** If price is zero or not set → **Add to Cart button is disabled**; customer cannot add to cart.

---

### **Journey 5 – Access Restricted Product (Unlisted/Draft)**

As a **Customer**, I cannot access products that are not published.

**Step 1:** Customer tries to access PDP via direct URL for a product with **Unlisted** or **Draft** status.

**Step 2:** System returns 404 page or redirects customer to shop homepage.

**Step 3:** Product details are NOT displayed under any circumstances.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Product images display as a slider; zoom functionality works for each image. If no images exist, a **default placeholder image** is displayed.

**BAC 2:** Product title and price are visible. Description is shown if it exists; hidden if not.

**BAC 3:** For products **with SKUs**: SKU selection updates price correctly; "Out of Stock" disables Add to Cart for SKUs with 0 quantity.

**BAC 4:** For products **without SKUs**: No SKU selector is shown; product-level price and quantity controls are displayed.

**BAC 5:** Physical products correctly display or hide Shipping and Size.

**BAC 6:** Blockchain verification link is shown for recorded products and opens in a new tab.

**BAC 7:** Add to Cart functionality works according to selected SKU/product and quantity.

**BAC 8:** Products with **zero or no price** display "$0" and the **Add to Cart button is disabled**.

**BAC 9:** Products with **Unlisted** or **Draft** status must return 404 or redirect; they are NOT accessible via direct link, search, or any other method.

**BAC 10:** The page must not break or show errors when product has no images, no description, or no SKUs.

**BAC 11:** First SKU must be **pre-selected by default** when the PDP loads for products with SKUs.

**BAC 12:** Color variants must display as **colored square swatches** using the exact hex color code selected by the merchant during product creation.

**BAC 13:** Products with **Scheduled Release** status must display the product page but **hide the Add to Cart button** and **block API cart addition**.

**BAC 14:** Products with **limited stock** (quantity tracking enabled) must show "Out of Stock" and disable Add to Cart when:
   - Available quantity is 0, OR
   - Customer's selected quantity exceeds available stock

**BAC 15:** Products with **`continue_selling=true`** (unlimited stock) must **never show "Out of Stock"** and always allow adding to cart regardless of quantity selected.
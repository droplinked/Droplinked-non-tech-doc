# Cart Page

**Feature ID:** STF-CART-PAGE-203

**Category:** Storefront | Cart System

**Actors:** Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Customers need a dedicated page to review their cart contents in detail before proceeding to checkout. This page allows customers to make final adjustments to quantities, remove unwanted items, and verify their order before committing to purchase.

### **Desired Outcome**

- Provide a full cart page where customers can review all cart items in detail.
- Allow customers to modify quantities, remove individual items, or clear the entire cart.
- Display comprehensive product information including images, titles, variant details, pricing, and totals.
- Enable easy navigation back to shopping or forward to checkout.
- Handle empty cart state gracefully.

---

# **2) Scope – In / Out**

### **In Scope**

- Displaying full cart page at `/cart` route.
- Showing product list with:
    - Product image (clickable, links to product page)
    - Product title
    - Variant/SKU information
    - Cost per unit
    - Quantity controls (+ and - buttons)
    - Total price per product line
- Displaying cart subtotal/total.
- Increasing quantity with + button.
- Decreasing quantity with - button.
- Removing item when quantity reaches 0 (or via - button at quantity 1).
- Individual product removal (delete/remove button).
- Clear entire cart functionality.
- "Continue Shopping" button (returns to shop/storefront).
- "Checkout" button (navigates to checkout in same tab).
- Empty state display when cart has no items.
- Clicking product image navigates to Product Detail Page.

### **Out of Scope**

- Promotional codes or discount application.
- Saved carts for later.
- Shipping cost calculation.
- Tax calculation preview.
- Product recommendations or upsells.
- Cart sharing functionality.

---

# **3) Key User Journeys**

---

## **Journey 1 – View Cart Page**

**Step 1:** Customer navigates to `/cart` (via cart overlay or direct URL).

**Step 2:** Cart page loads showing all cart items.

**Step 3:** For each product, customer sees:

- Product image
- Product title
- Variant information (e.g., "Size: XL, Color: Black")
- Cost per unit
- Quantity controls (+ and -)
- Total price for that product line

**Step 4:** Customer sees cart total at the bottom.

---

## **Journey 2 – Increase Product Quantity**

**Step 1:** Customer is on the cart page.

**Step 2:** Clicks the **+** button next to a product.

**Step 3:** Quantity increases by 1.

**Step 4:** Product line total updates.

**Step 5:** Cart total updates.

**Step 6:** Header cart counter updates.

---

## **Journey 3 – Decrease Product Quantity**

**Step 1:** Customer clicks the **-** button next to a product.

**Step 2:** If quantity is greater than 1:

- Quantity decreases by 1
- Product line total updates
- Cart total updates

**Step 3:** If quantity is exactly 1:

- Clicking  removes the item from cart
- Product disappears from cart list
- Cart total updates
- Header counter updates

---

## **Journey 4 – Remove Individual Product**

**Step 1:** Customer clicks the **Remove** or **Delete** button next to a product.

**Step 2:** Product is immediately removed from cart.

**Step 3:** Cart list updates.

**Step 4:** Cart total updates.

**Step 5:** Header cart counter updates.

**Step 6:** If this was the last item, empty state is displayed.

---

## **Journey 5 – Clear Entire Cart**

**Step 1:** Customer clicks **Clear Cart** button.

**Step 2:** All items are removed from cart.

**Step 3:** Empty state is displayed.

**Step 4:** Header cart counter resets to 0.

---

## **Journey 6 – Click Product Image**

**Step 1:** Customer clicks on a product image in the cart.

**Step 2:** Customer is redirected to the Product Detail Page for that product.

**Step 3:** Can return to cart via back button or cart icon.

---

## **Journey 7 – Continue Shopping**

**Step 1:** Customer clicks **Continue Shopping** button.

**Step 2:** Customer is redirected to the main shop/storefront page.

**Step 3:** Cart remains intact (items are not removed).

---

## **Journey 8 – Proceed to Checkout**

**Step 1:** Customer reviews cart items.

**Step 2:** Clicks **Checkout** button.

**Step 3:** Customer is redirected to the Checkout Page in the same tab.

---

## **Journey 9 – Empty Cart State**

**Step 1:** Customer navigates to `/cart` with no items in cart.

**Step 2:** Empty state is displayed showing:

- Message: "Your cart is empty"
- Optional: Illustration or icon
- "Continue Shopping" or "Browse Products" button

**Step 3:** Customer clicks the button and returns to shopping.

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Cart page must be accessible at `/cart` route.

### **BAC 2:** Each cart item must display:

- Product image (clickable, linking to Product Detail Page)
- Product title
- Variant/SKU information
- Cost per unit
- Quantity with + and - controls
- Total price per product line (quantity × cost per unit)

### **BAC 3:** Clicking **+** must increase quantity by 1 and update totals instantly.

### **BAC 4:** Clicking  must decrease quantity by 1. If quantity is 1, clicking  must remove the item from cart.

### **BAC 5:** Each product line must have a **Remove** button that immediately deletes the item from cart.

### **BAC 6:** **Clear Cart** button must remove all items and display empty state.

### **BAC 7:** Cart total must always reflect the sum of all product line totals.

### **BAC 8:** Header cart counter must update immediately when quantities change or items are removed.

### **BAC 9:** **Continue Shopping** button must redirect to the main shop/storefront.

### **BAC 10:** **Checkout** button must redirect to the Checkout Page in the same tab.

### **BAC 11:** Clicking a product image must navigate to the Product Detail Page.

### **BAC 12:** When cart is empty, an empty state must be displayed with clear messaging and a call-to-action to continue shopping.

### **BAC 13:** All quantity changes and removals must update the cart state instantly without page refresh.

### **BAC 14:** Cart page must be responsive and functional on mobile, tablet, and desktop devices.
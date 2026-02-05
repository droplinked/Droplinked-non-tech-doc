# Cart Overlay – View & Modify Cart

**Feature ID:** STF-CART-OVERLAY-202

**Category:** Storefront | Cart System

**Actors:** Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Customers need a fast way to view cart contents directly from the storefront without navigating to a full cart page. The cart overlay provides a quick preview of items and allows customers to either clear the cart or proceed to the full cart page for modifications.

### **Desired Outcome**

- Clicking the cart icon opens a cart overlay showing all items currently in the cart.
- Customer can view cart items with basic information.
- Customer can clear the entire cart.
- Customer can proceed to the Cart Page by clicking the Checkout button.
- Cart expires automatically after 48 hours of inactivity.

---

# **2) Scope – In / Out**

### **In Scope**

- Displaying cart items in an overlay modal.
- Displaying product thumbnail (default thumbnail), title, selected SKU attributes (e.g., **XL / Black**), and quantity.
- Clearing the entire cart.
- Updating cart state and header cart counter.
- Redirecting user to Cart Page via Checkout button.
- Auto-expiring cart after 48 hours.

### **Out of Scope**

- Checkout form UI (separate feature).
- Promotional prices, discounts, coupons.
- Saved carts, abandoned cart emails.
- Guest → account cart merging.

---

# **3) Key User Journeys**

---

## **Journey 1 – Open Cart Overlay**

**Step 1:** Customer clicks the cart icon in the header.

**Step 2:** Cart overlay opens showing:

- Product thumbnail
- Title
- SKU info (e.g., *XL / Black*)
- Quantity (view only, no modification)

**Step 3:** Total cart quantity is shown on header icon.

---

## **Journey 2 – Clear Entire Cart**

**Step 1:** Customer clicks "Clear Cart" button.

**Step 2:** System:

- Validates available inventory
- Updates quantity
- Recalculates totals
- Syncs updated cart
    
    **Step 3:** Header cart counter updates.
    

If stock is insufficient → quantity cannot increase.

---

## **Journey 3 – Remove a Single Product**

**Step 1:** Customer clicks “Remove” on a product line.

**Step 2:** System removes the product from cart.

**Step 3:** Overlay updates + header counter updates.

If last product removed → cart becomes empty.

---

## **Journey 4 – Clear Entire Cart**

**Step 1:** Customer clicks “Clear Cart”.

**Step 2:** System deletes all items.

**Step 3:** Overlay shows empty state.

**Step 4:** Header counter resets to 0.

---

## **Journey 3 – Proceed to Cart Page**

**Step 1:** Customer clicks **Checkout** button in the overlay.

**Step 2:** Overlay closes.

**Step 3:** Customer is redirected to the Cart Page to review and modify items.

---

## **Journey 4 – Cart Auto-Expiration**

**Step 1:** Cart remains idle for 48 hours.

**Step 2:** System automatically deletes the cart.

**Step 3:** Next time customer opens cart → empty state.

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Clicking the cart icon must always open the cart overlay with up-to-date cart data.

### **BAC 2:** Each cart item must display:

- Default product thumbnail
- Product title
- SKU attributes (e.g., “XL / Black”)
- Quantity (display only, no modification controls)

### **BAC 3:** Cart overlay must NOT allow quantity modification or individual item removal.

### **BAC 4:** Customer can only clear the entire cart via "Clear Cart" button.

### **BAC 5:** Clicking "Clear Cart" must remove all items and update the cart state instantly.

### **BAC 6:** Header cart counter must always reflect the **total number of items (sum of quantities)**.

### **BAC 7:** Clicking "Checkout" button must navigate the customer to the **Cart Page** (not Checkout Page).

### **BAC 8:** Cart must automatically expire and delete itself after **48 hours** of inactivity.
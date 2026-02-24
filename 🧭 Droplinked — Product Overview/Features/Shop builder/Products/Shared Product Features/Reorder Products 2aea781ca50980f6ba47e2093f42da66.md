# Reorder Products

# **Feature ID:** IAA-PROD-404

# **Category:** Product Management | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need the ability to control the display order of their products in the shop. Reordering ensures priority products are shown first, improves merchandising, and provides better control over customer experience on both the Shop Builder and Shop Frontend.

### **Desired Outcome**

- Allow merchants to view their product list in the current display order.
- Enable drag-and-drop functionality to rearrange product order.
- Save and apply the new order after closing the reorder modal.
- Reflect the updated order on both Shop Builder and Shop Frontend.

---

# **2) Scope – In / Out**

### **In Scope**

- Reorder functionality for all product types (Physical, Digital, POD).
- Drag-and-drop UI to rearrange products in a modal or list.
- Display current product order to the merchant.
- Persist reordered sequence in the database.
- Apply updated order to both Shop Builder and Shop Frontend immediately after changes.

### **Out of Scope**

- Bulk reordering across multiple pages (pagination not supported in modal).
- Undo / restore previous order (separate feature if needed).
- Product filtering/search within the reorder modal.

---

# **3) Key User Journeys**

---

### **Journey 1 – Reorder Product List**

As a **Merchant**, I can rearrange the order of my products to control how they are displayed in the shop.

**Step 1:** Merchant opens the product list.

**Step 2:** Merchant clicks the “Reorder” button.

**Step 3:** System opens a modal showing all products in current order.

**Step 4:** Merchant drags and drops products to reorder them.

**Step 5:** Merchant closes the modal or clicks “Save”.

**Step 6:** System saves the new order in the database.

**Step 7:** Shop Builder and Shop Frontend display products according to the new order.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** The Reorder modal must display all products in their current order.

**BAC 2:** Merchant must be able to drag and drop any product to a new position in the list.

**BAC 3:** Changes must persist after closing the modal and reflect immediately in both Shop Builder and Shop Frontend.

**BAC 4:** Reordering must work consistently across all product types (Physical, Digital, POD).

**BAC 5:** No product data (price, title, images) is affected by reordering—only display order changes.
# Soft Delete Product

# **Feature ID:** IAA-PROD-402

# **Category:** Product Management | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need the ability to remove products they no longer want to see in their product list without permanently deleting data. Soft delete ensures that product records remain in the database for reporting or recovery purposes, maintaining data integrity while keeping the merchant interface clean.

### **Desired Outcome**

- Allow merchants to remove products from their interface without permanently deleting them.
- Ensure soft-deleted products are hidden from all merchant views.
- Maintain product data in the database for future reporting, auditing, or recovery.

---

# **2) Scope – In / Out**

### **In Scope**

- Soft delete functionality for all product types (Physical, Digital, POD).
- Confirmation prompt before soft deletion.
- Hidden display in the merchant’s product list after deletion.
- Maintain all product data in the database after soft deletion.

### **Out of Scope**

- Hard delete (permanent deletion).
- Bulk delete operations.
- Undo / restore functionality (handled in separate feature if needed).

---

# **3) Key User Journeys**

---

### **Journey 1 – Soft Delete a Product**

As a **Merchant**, I can remove a product from my view without deleting it from the database.

**Step 1:** Merchant opens the product list.

**Step 2:** Merchant clicks the “Delete” button on a product.

**Step 3:** System displays a confirmation prompt.

**Step 4:** Merchant confirms the action.

**Step 5:** Product is hidden from the merchant interface but remains in the database.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Clicking “Delete” must show a confirmation modal before any action.

**BAC 2:** After confirmation, the product must disappear from all merchant-facing product lists.

**BAC 3:** Soft-deleted products must remain in the database with all original data intact.

**BAC 4:** Soft delete must apply consistently to all product types (Physical, Digital, POD).
# Product Publish & Visibility Status

# **Feature ID:** IAA-PROD-405

# **Category:** Product Management | **Actors:** Merchant | **Channel:** Web

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need control over which products are visible in the Shop Frontend and how they behave for customers. Products may be in **Draft** or **Published** status, and can also have **Visibility Status** as Public or Private. This ensures merchants can manage product availability and restrict customer interactions for private products.

### **Desired Outcome**

- Allow merchants to set product status as **Draft** or **Published**.
- Ensure Draft products are only visible in Shop Builder, not in Shop Frontend.
- Allow merchants to set Visibility Status as **Public** or **Private**.
- Private products remain visible in Shop Frontend but disable the “Add to Cart” button.

---

# **2) Scope – In / Out**

### **In Scope**

- Product status: Draft / Published.
- Visibility status: Public / Private.
- Status and visibility apply consistently to all product types (Physical, Digital, POD).
- Draft products visible only in Shop Builder.
- Published products appear in Shop Frontend if Visibility is Public.
- Private products are visible in Shop Frontend but cannot be added to cart.

### **Out of Scope**

- Scheduling product publish dates (separate PRD).
- Affiliate revenue calculations or tracking (handled in separate feature).
- Product-level user permissions or access control beyond Public/Private.

---

# **3) Key User Journeys**

---

### **Journey 1 – Set Product Status**

As a **Merchant**, I can control whether a product is Draft or Published.

**Step 1:** Merchant opens the product editor.

**Step 2:** Merchant selects product status (Draft / Published).

**Step 3:** If Draft → product visible only in Shop Builder.

**Step 4:** If Published → product visible in Shop Frontend according to Visibility Status.

---

### **Journey 2 – Set Visibility Status**

As a **Merchant**, I can control whether a product is Public or Private.

**Step 1:** Merchant opens the product editor.

**Step 2:** Merchant sets Visibility Status (Public / Private).

**Step 3:** If Public → product fully available in Shop Frontend (can be added to cart).

**Step 4:** If Private → product visible in Shop Frontend but “Add to Cart” is disabled.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Draft products must never appear in Shop Frontend.

**BAC 2:** Published products must appear in Shop Frontend if Visibility is Public.

**BAC 3:** Private products are visible in Shop Frontend but the “Add to Cart” button is disabled.

**BAC 4:** Status and Visibility must work consistently for Physical, Digital, and POD products.

**BAC 5:** Changing product status or visibility must immediately reflect in both Shop Builder and Shop Frontend.
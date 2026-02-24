# Default Product Image Selection

# **Feature ID:** IAA-PROD-407

# **Category:** Product Management | **Actors:** Merchant | **Channel:** Web

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need the ability to choose which product image is displayed as the default across all interfaces. A default image ensures consistent branding, improves user experience, and determines which image is shown in product listings, Shop Builder, Shop Frontend, and shopping carts.

### **Desired Outcome**

- Allow merchants to select a default product image during product creation or editing.
- If no default is selected, the system automatically uses the first uploaded image.
- Default image is displayed consistently in:
    - Product lists in Shop Builder
    - Shop Frontend product listings
    - Shopping cart previews

---

# **2) Scope – In / Out**

### **In Scope**

- Default image selection for all product types (Physical, Digital, POD).
- Automatic fallback to the first uploaded image if no default is chosen.
- Display of default image across all relevant product views: Shop Builder, Shop Frontend, and shopping cart.
- Updating default image during product editing.

### **Out of Scope**

- Image editing (cropping, filters, resizing) – handled separately.
- Bulk default image assignment across multiple products.
- Product image versioning or historical tracking.

---

# **3) Key User Journeys**

---

### **Journey 1 – Set Default Product Image**

As a **Merchant**, I can select which product image is used as default.

**Step 1:** Merchant opens product creation or edit page.

**Step 2:** Merchant uploads one or more images.

**Step 3:** Merchant selects one image as the default.

**Step 4:** Merchant saves the product.

**Step 5:** Default image is applied across all product views.

---

### **Journey 2 – System Fallback**

As a **Merchant**, if I do not select a default image:

**Step 1:** Merchant uploads one or more images.

**Step 2:** Merchant does not choose a default.

**Step 3:** System automatically selects the first uploaded image as the default.

**Step 4:** Default image is applied across all product views.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Merchant must be able to select any uploaded image as the default.

**BAC 2:** If no default is selected, the first uploaded image is automatically assigned.

**BAC 3:** Default image must appear in all relevant product views: Shop Builder, Shop Frontend, and shopping cart.

**BAC 4:** Updating the default image must immediately reflect across all interfaces.

**BAC 5:** Feature must work consistently for Physical, Digital, and POD products.
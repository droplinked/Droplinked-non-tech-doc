# Duplicate Product

# **Feature ID:** IAA-PROD-403

# **Category:** Product Management | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a fast and reliable way to create new products by duplicating existing ones. Duplication saves time for similar products while ensuring consistency. For POD products, duplication must reset all Printful design data, requiring merchants to reselect and redesign the product.

### **Desired Outcome**

- Allow merchants to duplicate any existing product (Physical, Digital, POD).
- Set duplicated product status to **Draft** by default.
- Preserve all relevant product data for Physical and Digital products.
- For POD products, reset design, images, and SKUs to require re-design via Printful.

---

# **2) Scope – In / Out**

### **In Scope**

- Duplicate product functionality for all product types.
- Duplicated product is always created with **Draft** status.
- Physical and Digital products retain all original product attributes.
- POD products reset all design-related data (images, SKUs, Printful links).
- Merchant can edit duplicated product before publishing.

### **Out of Scope**

- Bulk duplication.
- Automatic publishing of duplicated products.
- Undo duplication (handled separately if needed).

---

# **3) Key User Journeys**

---

### **Journey 1 – Duplicate a Physical or Digital Product**

As a **Merchant**, I can duplicate an existing Physical or Digital product to create a new product draft.

**Step 1:** Merchant opens the product list.

**Step 2:** Merchant clicks the “Duplicate” action on a product.

**Step 3:** System creates a new product with all attributes copied.

**Step 4:** Duplicated product status is set to **Draft**.

**Step 5:** Merchant can edit fields (title, description, variants, SKUs, images, etc.).

**Step 6:** Merchant publishes the duplicated product when ready.

---

### **Journey 2 – Duplicate a POD Product**

As a **Merchant**, I can duplicate a POD product, which resets all design-related data.

**Step 1:** Merchant opens the product list.

**Step 2:** Merchant clicks the “Duplicate” action on a POD product.

**Step 3:** System creates a new product draft, copying only non-design fields.

**Step 4:** Printful design, images, and SKUs are cleared.

**Step 5:** Merchant must select a blank product and open Printful designer to recreate the design.

**Step 6:** After design completion, SKU and images are generated automatically.

**Step 7:** Merchant can edit Title, Description, Collection, and SKU prices.

**Step 8:** Merchant publishes the product when ready.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Duplicated product must always be created with **Draft** status.

**BAC 2:** Physical and Digital products retain all original attributes during duplication.

**BAC 3:** POD product duplication must reset all design-related fields:

- Images
- SKUs
- Printful design data

**BAC 4:** Merchant must be able to edit all duplicated product fields before publishing.

**BAC 5:** Duplication must work consistently across all product types (Physical, Digital, POD).
# Shop Builder – Designer Canvas

**Feature ID:** IAA-DES-101

**Category:** Shop Builder | **Actors:** Merchant

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a flexible and visual interface to design their storefronts. The Designer allows merchants to customize layout, add sections, and manage components, ensuring the storefront reflects their brand and product strategy. Accurate saving and publishing of changes is critical to maintain consistency between Shop Builder and Storefront.

### **Desired Outcome**

- Provide a drag-and-drop canvas for merchants to build their shop.
- Enable adding, editing, and removing components such as Hero sections, text blocks, product lists, and collections.
- Allow merchants to save their design and publish it to Storefront, ensuring all changes are applied correctly.
- Support a preview of edits before publishing where feasible.

---

# **2) Scope – In / Out**

### **In Scope**

- Adding components to the canvas.
- Editing component properties (text, image, button links, product/collection bindings).
- Reordering sections via drag-and-drop.
- Deleting sections or components (direct deletion without confirmation).
- Publishing changes to Storefront with a **Publish** button (which also saves the design).
- Linking components dynamically to products, collections, and promotions.

### **Out of Scope**

- Advanced AI-assisted design suggestions.
- Multi-user collaboration on the same canvas.
- Undo/redo history beyond the last save.
- Responsive layout editing for different screen sizes (optional future feature).
- Integration with third-party design tools.

---

# **3) Key User Journeys**

### **Journey 1 – Add a Component**

As a **Merchant**, I can drag a new component onto the canvas and configure it.

**Step 1:** Merchant opens the Designer.

**Step 2:** Drag a Hero section from the component library onto the canvas.

**Step 3:** Edit text, image, and button link.

**Step 4:** Click **Publish** to save and push the changes to Storefront.

**Step 5:** Verify that the updated design appears correctly in Storefront.

---

### **Journey 2 – Reorder Sections**

As a **Merchant**, I can rearrange sections to define the storefront layout.

**Step 1:** Merchant selects a section on the canvas.

**Step 2:** Drag the section to a new position.

**Step 3:** Click **Publish** to save and update Storefront.

**Step 4:** Confirm that Storefront reflects the new layout.

---

### **Journey 3 – Delete a Component or Section**

As a **Merchant**, I can remove a section or component from my shop.

**Step 1:** Merchant selects the section/component to delete.

**Step 2:** Click the **Delete** button.

**Step 3:** Component is deleted immediately without confirmation.

**Step 4:** Click **Publish** to save and push the updated design to Storefront.

**Step 5:** Verify the component is removed in Storefront.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** All added or edited components must render correctly in Storefront after publishing.

**BAC 2:** Layout changes (reorder, add, delete) must persist after saving and be applied after publishing.

**BAC 3:** Merchant cannot break Storefront rendering by misconfiguring components.

**BAC 4:** Only components enabled in the Shop Builder are available in the Designer.

**BAC 5:** All component actions (add/edit/delete/reorder) must be **saved and applied to Storefront when Publish is clicked**; no manual refresh required.
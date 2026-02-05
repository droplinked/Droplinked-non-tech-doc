# Collections Management

# **Feature ID:** IAA-COL-101

# **Category:** Product Management | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a centralized interface to create, view, edit, and manage product collections. Collections help organize products, control visibility, and define roles for team members managing specific collections. Proper handling of empty states, editing, and deletion ensures a smooth user experience.

### **Desired Outcome**

- Allow merchants to create new collections with required and optional fields.
- Display all collections in a list with key information.
- Allow editing, deletion (with last collection protection), and role assignments for collections.

---

# **2) Scope – In / Out**

### **In Scope**

- **Empty State:**
    - Display when no collections exist.
    - Show “New Collection” button.
- **Create Collection:**
    - Modal opens when “New Collection” clicked.
    - Required field: Collection Name.
    - Optional fields: Description, Collection Cover Image.
    - Save collection and display in list after creation.
- **Collection List Display:**
    - Show Collection Cover (or default if none).
    - Show Collection Name.
    - Show Product Count in the collection.
    - Show assigned roles (if any) by role title.
- **Collection Actions:**
    - Edit: update name, description, cover image, roles.
    - Delete: remove collection (cannot delete last collection).
    - Assign / Edit Roles: allow role assignment per collection.

### **Out of Scope**

- Bulk collection creation.
- Advanced filtering or sorting of collections.
- Role management outside collection context.
- Analytics for collections.

---

# **3) Key User Journeys**

---

### **Journey 1 – Create a New Collection**

As a **Merchant**, I can create a new collection.

**Step 1:** Merchant clicks “New Collection” button.

**Step 2:** Modal opens.

**Step 3:** Merchant enters Collection Name (required).

**Step 4:** Merchant optionally enters Description and uploads Cover Image.

**Step 5:** Merchant clicks “Save”.

**Step 6:** Collection is added to the list with provided details.

---

### **Journey 2 – Edit a Collection**

As a **Merchant**, I can edit existing collection details.

**Step 1:** Merchant clicks “Edit” on a collection.

**Step 2:** Modal opens with current collection information.

**Step 3:** Merchant updates Name, Description, Cover Image, or Roles.

**Step 4:** Merchant clicks “Save”.

**Step 5:** Changes are applied and reflected in the collection list.

---

### **Journey 3 – Delete a Collection**

As a **Merchant**, I can delete a collection.

**Step 1:** Merchant clicks “Delete” on a collection.

**Step 2:** System prompts for confirmation.

**Step 3:** If not the last collection, deletion proceeds.

**Step 4:** Collection is removed from the list.

**Step 5:** If last collection, system blocks deletion and shows a warning.

---

### **Journey 4 – Assign / Edit Roles for a Collection**

As a **Merchant**, I can assign roles to a collection.

**Step 1:** Merchant selects “Edit Roles” on a collection.

**Step 2:** Modal opens with available roles.

**Step 3:** Merchant selects or updates roles.

**Step 4:** Merchant clicks “Save”.

**Step 5:** Roles are saved and displayed in collection list.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** “New Collection” modal must enforce Collection Name as required.

**BAC 2:** Collection list must display cover image, name, product count, and roles.

**BAC 3:** Merchant must be able to edit collection details and roles.

**BAC 4:** Merchant must be able to delete a collection unless it is the last remaining collection.

**BAC 5:** Role assignments must persist and display correctly in collection list.

**BAC 6:** Default cover image must be shown if none uploaded.
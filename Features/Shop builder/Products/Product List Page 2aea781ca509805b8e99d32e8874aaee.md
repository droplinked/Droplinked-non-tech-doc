# Product List Page

# **Feature ID:** IAA-PROD-101

# **Category:** Product Management | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a central place to view, navigate, and manage all their products. The Product List Page provides a simple inventory overview with essential actions such as editing, sharing, publishing/unpublishing, and viewing products on the store. A clean and minimal interface ensures fast discovery and efficient management.

### **Desired Outcome**

- Display all merchant products in a scrollable list with infinite loading.
- Allow merchants to search products by title.
- Provide essential product actions (edit, publish/unpublish, view, share).
- Show an empty state if the merchant has no products.
- Ensure easy navigation to the product edit page.

---

# **2) Scope – In / Out**

### **In Scope**

- Infinite scroll product list.
- Server-side loading of products.
- Product table with:
    - Product thumbnail (main image)
    - Product title
    - Collection name
    - Minimum product price
    - Status (Published / Draft)
    - **Unit Quantity**:
        - Display "Unlimited" if product has unlimited stock
        - Display total sum of all SKU quantities if all are finite
        - Display "Unlimited" if ANY SKU has unlimited stock
- Action menu (3-dot menu) with:
    - **View on Store** (opens product page in new tab; **only visible for Published and Scheduled products**)
    - **Share** (opens simple share modal)
    - **Edit** (redirects to Edit Product page)
    - **Change Status** (options: **Unlisted** or **Public** only)
    - **Remove** (handled in separate Delete PRD)
- Search by product title via server-side endpoint.
- Empty state component when no products exist.

### **Out of Scope**

- Sorting and filtering.
- Bulk actions (publish, delete, etc.).
- Pagination (numeric pages).
- Collection filtering.
- Delete product logic (separate PRD).

---

# **3) Key User Journeys**

### **Journey 1 – View Products**

As a **Merchant**, when I open the Product List page, I can see all my products in a scrollable list.

**Step 1:** Merchant clicks **Inventory Management** from the sidebar.

**Step 2:** Product List page loads.

**Step 3:** If no products exist → Empty State appears.

**Step 4:** If products exist → List is displayed.

**Step 5:** As merchant scrolls → additional products load automatically (infinite scroll).

---

### **Journey 2 – Search Products**

As a **Merchant**, I want to find a product by its title.

**Step 1:** Merchant types a search term into the search field.

**Step 2:** Server processes the query and returns matching products.

**Step 3:** Product list refreshes with the results.

**Step 4:** If no results → show “No matching products” state.

---

### **Journey 3 – Manage Product (Actions Menu)**

As a **Merchant**, I want to manage each product quickly.

**Step 1:** Merchant clicks the 3-dot action menu on a product row.

**Step 2:** Menu options appear:

- View on Store (only for Published/Scheduled products)
- Share
- Edit
- Change Status (Unlisted / Public)
- Remove
    
    **Step 3:** Merchant selects the desired action.
    
    **Step 4:** System performs the requested operation.
    

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Product list must support infinite scroll and load products server-side.

**BAC 2:** Search must work server-side and return results based on product title only.

**BAC 3:** Product row must display thumbnail, title, collection, minimum price, status, and Unit Quantity.

**BAC 3.1:** Unit Quantity logic:
- Display "Unlimited" if product has unlimited stock (continue_selling = true)
- Display sum of all SKU quantities if all SKUs are finite
- Display "Unlimited" if ANY SKU has unlimited stock

**BAC 4:** Action menu must include View on Store, Share, Edit, Change Status, and Remove.

**BAC 4.1:** Change Status must only allow switching between **Unlisted** and **Public** statuses.

**BAC 5:** “View on Store” must **only be visible** for products with status **Published** or **Scheduled**; completely hidden for Draft and Unlisted products.

**BAC 6:** When merchant scrolls, product batches must load seamlessly without page reload.

**BAC 7:** Empty state must appear when no products exist.

**BAC 8:** Share modal must display a simple modal for copying the product URL.
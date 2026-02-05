# Product Listing Page (PLP)

**Feature ID:** IAA-PLP-101

**Category:** Storefront | **Actors:** Customer, Merchant

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Customers need a clear and functional view of all products in a shop. Merchants require a listing that displays products correctly, allows search, filtering, and navigation to product detail pages. Proper handling of empty states ensures a professional storefront experience even when no products exist.

### **Desired Outcome**

- Display only products with **Public** or **Scheduled Release (past date)** status.
- Products with **Unlisted** or **Draft** status are excluded from PLP entirely.
- Handle empty states gracefully.
- Show product thumbnail (or default image if no image exists), title, and lowest SKU price (or product price if no SKUs).
- Products with zero or no price are displayed with "$0" as the price.
- Allow users to search and filter products by title, price range, and collection.
- Clicking a product navigates to the corresponding Product Detail Page.

---

# **2) Scope – In / Out**

### **In Scope**

- Displaying only **Public** and **Scheduled Release (past date)** products in a list or grid according to the merchant's design.
- Excluding **Unlisted** and **Draft** products from display (not visible in listing, search, or any filter).
- Showing product thumbnail (or **default placeholder image** if product has no images), title, and lowest SKU price (or product-level price if no SKUs exist).
- Displaying products with zero or no price with "$0" shown as price.
- Empty state display when no products exist.
- Product search by title.
- **Sorting products by:**
    - Price: Low to High
    - Price: High to Low
    - Name: A to Z
    - Name: Z to A
    - Default (clear all sorting)
- **Filtering products by:**
    - Price range (min-max slider or input)
    - Collection(s)
    - Product Type: Physical, Digital (POD products are categorized as Physical)
- Navigation to Product Detail Page on product click.
- Ability to apply multiple filters simultaneously.
- Ability to clear all filters and return to default view.

### **Out of Scope**

- Infinite scroll or pagination (handled in separate PRD if needed).
- Product recommendations or upsell features.
- Analytics tracking for PLP views.
- Multi-language support.

---

# **3) Key User Journeys**

### **Journey 1 – Customer Browsing Products**

As a **Customer**, I can browse the product listing page to find items of interest.

**Step 1:** Customer opens the shop front.

**Step 2:** PLP displays only **Public** and **Scheduled Release (past date)** products according to merchant design. Products with **Unlisted** or **Draft** status are not shown.

**Step 3:** Each product shows thumbnail (or default placeholder if no image), title, and price (lowest SKU price or product-level price if no SKUs). Products with zero/no price display "$0".

**Step 4:** Customer clicks a product → navigates to Product Detail Page.

---

### **Journey 2 – Product Search**

As a **Customer**, I can search products by title.

**Step 1:** Customer enters a search query in the search bar.

**Step 2:** PLP filters products based on title.

**Step 3:** Matching products are displayed; empty state appears if no results found.

---

### **Journey 3 – Customer Sorts Products**

As a **Customer**, I can sort products to organize the listing according to my preferences.

**Step 1:** Customer opens the sort dropdown/menu.

**Step 2:** Sees available sort options:

- **Default** (no sorting applied)
- **Price: Low to High**
- **Price: High to Low**
- **Name: A to Z**
- **Name: Z to A**

**Step 3:** Customer selects a sort option.

**Step 4:** PLP instantly reorders products based on the selected sort.

**Step 5:** Customer can change sort option at any time.

**Step 6:** Selecting **Default** removes all sorting and returns to the original product order.

---

### **Journey 4 – Customer Filters Products by Price Range**

As a **Customer**, I can filter products within a specific price range.

**Step 1:** Customer opens the price filter section.

**Step 2:** Sets minimum and maximum price values (via slider or input fields).

**Step 3:** PLP updates to show only products within the specified price range.

**Step 4:** Customer can adjust the range at any time.

**Step 5:** Removing the price filter shows all products again.

---

### **Journey 5 – Customer Filters Products by Collection**

As a **Customer**, I can filter products by specific collections.

**Step 1:** Customer opens the collection filter section.

**Step 2:** Sees a list of all available collections created by the merchant.

**Step 3:** Selects one or more collections.

**Step 4:** PLP updates to show only products belonging to the selected collection(s).

**Step 5:** Customer can deselect collections to broaden results.

**Step 6:** Clearing all collection selections shows all products.

---

### **Journey 6 – Customer Filters Products by Product Type**

As a **Customer**, I can filter products by their type (Physical or Digital).

**Step 1:** Customer opens the product type filter section.

**Step 2:** Sees available product types:

- **Physical** (includes standard physical products and POD products)
- **Digital**

**Step 3:** Selects one or both product types.

**Step 4:** PLP updates to show only products of the selected type(s).

**Step 5:** POD (Print on Demand) products are included in the **Physical** category.

**Step 6:** Deselecting all types shows all products.

---

### **Journey 7 – Customer Applies Multiple Filters and Sorting**

As a **Customer**, I can combine multiple filters and sorting to find exactly what I need.

**Step 1:** Customer selects a collection filter (e.g., "Summer Collection").

**Step 2:** Adds a price range filter (e.g., $10 - $50).

**Step 3:** Selects product type filter (e.g., "Physical").

**Step 4:** Applies sorting (e.g., "Price: Low to High").

**Step 5:** PLP displays products that match ALL selected filters, sorted accordingly.

**Step 6:** Customer can see the number of products matching the current filters.

**Step 7:** Customer clicks "Clear All Filters" or "Reset" to remove all filters and sorting.

**Step 8:** PLP returns to the default view showing all products.

---

### **Journey 8 – No Results After Filtering**

As a **Customer**, I see a clear message when my filters return no results.

**Step 1:** Customer applies filters (e.g., price range $200-$500, Digital products only).

**Step 2:** No products match the selected criteria.

**Step 3:** PLP displays an empty state message:

- "No products found matching your filters."
- "Try adjusting your filters or clearing them."

**Step 4:** Customer can adjust filters or click "Clear Filters" to see all products again.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** PLP displays product thumbnail (or default placeholder image if none exists), title, and price (lowest SKU price or product-level price if no SKUs) for all eligible products.

**BAC 2:** Clicking on a product navigates to the corresponding Product Detail Page.

**BAC 3:** Search functionality correctly filters products by title.

**BAC 4:** Sorting functionality must support 5 options:

- Default (no sorting)
- Price: Low to High
- Price: High to Low
- Name: A to Z
- Name: Z to A

**BAC 5:** Selecting "Default" sort option must clear all active sorting and return products to their original order.

**BAC 6:** Price range filter must allow customers to set minimum and maximum price boundaries.

**BAC 7:** Collection filter must display all available collections and allow single or multiple selection.

**BAC 8:** Product type filter must include two options: Physical and Digital.

**BAC 9:** POD (Print on Demand) products must be categorized and filtered as Physical products.

**BAC 10:** Multiple filters can be applied simultaneously and products must match ALL selected criteria.

**BAC 11:** Sorting must work independently and can be combined with any active filters.

**BAC 12:** A "Clear All Filters" or "Reset" button must be available to remove all filters and sorting at once.

**BAC 13:** The product count must update dynamically to show how many products match the current filters.

**BAC 14:** Empty state is displayed when no products exist or when search/filter returns no results.

**BAC 15:** Filter and sort changes must update the product display instantly without requiring page refresh.

**BAC 16:** Products with **Unlisted** or **Draft** status must NOT appear in PLP under any circumstances (listing, search, or filtering).

**BAC 17:** Products without images must display a **default placeholder image** instead of breaking the layout.

**BAC 18:** Products with zero or no price must display "$0" as the price in the listing.
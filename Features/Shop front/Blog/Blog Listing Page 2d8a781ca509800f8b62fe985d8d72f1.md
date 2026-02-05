# Blog Listing Page

**Feature ID:** STF-BLOG-101

**Category:** Storefront / Content

**Actors:** Merchant, Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a way to publish blog content to engage customers, improve SEO, and provide valuable information about products, brand story, and industry insights. Customers need a clear and organized way to discover and browse available blog posts on the storefront.

### **Desired Outcome**

- Provide a dedicated blog listing page where customers can browse all published blog posts.
- Display blog posts with essential information (thumbnail, title, category, date, summary).
- Show the blog link in the footer only if the merchant has published blog posts.
- Handle empty states gracefully when no blogs are available.
- Ensure the page is fully responsive across all device sizes.

---

# **2) Scope – In / Out**

### **In Scope**

- Blog listing page accessible at `/blogs` route.
- Display all published blogs in a responsive grid/list layout.
- Blog link appears in storefront footer only when merchant has published blogs.
- Each blog card displays:
    - Blog featured image
    - Blog title
    - Category
    - Creation date
    - Search Engine Summary (preview text)
    - Keywords/tags
- Empty state page when no blogs exist.
- Responsive design for mobile, tablet, and desktop.
- Clicking a blog card navigates to the blog detail page.

### **Out of Scope**

- Blog search functionality.
- Blog filtering by category or tags.
- Pagination (if blog count is low; can be added later).
- Blog commenting system.
- Social sharing buttons.
- Related blogs suggestions.
- Blog author information display.
- Draft or scheduled blog posts (only published blogs are shown).

---

# **3) Key User Journeys**

---

### **Journey 1 – Customer Discovers Blog Section**

1. Customer visits the storefront.
2. Scrolls to the **footer**.
3. **If merchant has published blogs:** Sees "Blog" link in the footer.
4. **If merchant has no blogs:** No blog link appears in the footer.

---

### **Journey 2 – Customer Views Blog Listing Page**

1. Customer clicks on the **Blog** link in the footer.
2. Navigates to `/blogs` route.
3. Sees a responsive grid/list of all published blog posts.
4. Each blog card displays:
    - **Featured image**
    - **Blog title**
    - **Category** (e.g., "Product Updates", "Industry News")
    - **Creation date** (formatted, e.g., "December 15, 2025")
    - **Search Engine Summary** (preview/excerpt text)
    - **Keywords** (displayed as tags)
5. Customer can scroll through all available blogs.

---

### **Journey 3 – Customer Clicks on a Blog Card**

1. Customer is on the blog listing page.
2. Clicks on any blog card.
3. Navigates to `/blogs/:slug` (blog detail page).
4. Reads the full blog content.

---

### **Journey 4 – No Blogs Available (Empty State)**

1. Customer navigates to `/blogs` (either via direct URL or footer link).
2. **If no blogs are published:**
    - Page displays an **empty state** message:
        - "No blog posts available yet."
        - "Check back soon for updates!"
    - No blog cards are shown.
3. Customer can navigate back to the main storefront.

---

### **Journey 5 – Merchant Publishes First Blog**

1. Merchant creates and publishes their first blog post via the dashboard.
2. Blog link automatically appears in the storefront footer.
3. `/blogs` page now displays the newly published blog.
4. Customers can see and access the blog.

---

### **Journey 6 – Merchant Deletes All Blogs**

1. Merchant deletes all published blogs from the dashboard.
2. Blog link automatically disappears from the storefront footer.
3. If a customer navigates to `/blogs` via direct URL, they see the empty state.

---

# **4) Business Acceptance Criteria (BAC)**

### **BAC 1:** The blog link must appear in the storefront footer ONLY if the merchant has at least one published blog.

### **BAC 2:** The blog listing page must be accessible at the `/blogs` route.

### **BAC 3:** Each blog card must display: featured image, title, category, creation date, search engine summary, and keywords.

### **BAC 4:** The blog listing page must be fully responsive and render correctly on mobile, tablet, and desktop devices.

### **BAC 5:** When no blogs are published, the `/blogs` page must display a clear empty state message.

### **BAC 6:** Clicking a blog card must navigate to the correct blog detail page at `/blogs/:slug`.

### **BAC 7:** Blog listing must display only published blogs (no drafts or scheduled posts).

### **BAC 8:** The creation date must be formatted in a user-friendly way (e.g., "December 15, 2025" or "15 Dec 2025").

### **BAC 9:** Blog cards must maintain consistent styling and alignment across the grid/list layout.

### **BAC 10:** If the featured image is missing, a placeholder image or default thumbnail must be displayed.
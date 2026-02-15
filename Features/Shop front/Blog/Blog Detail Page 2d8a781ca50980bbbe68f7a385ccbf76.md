# Blog Detail Page

**Feature ID:** STF-BLOG-102

**Category:** Storefront / Content

**Actors:** Merchant, Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

After customers discover blog posts on the listing page, they need a dedicated page to read the full blog content. The blog detail page provides the complete reading experience with rich content, images, links, and metadata that helps customers engage deeply with the merchant's content.

### **Desired Outcome**

- Provide a dedicated blog detail page accessible via unique slug URL.
- Display complete blog content including text, images, and links.
- Show blog metadata (title, tags, creation date, shop branding).
- Ensure fully responsive reading experience across all devices.
- Enable customers to read, engage with, and share valuable content.

---

# **2) Scope – In / Out**

### **In Scope**

- Blog detail page accessible at `/blogs/:slug` route.
- Display complete blog content with rich formatting.
- Blog header section showing:
    - Featured image
    - Blog title
    - Tags
    - Creation date
    - Shop name and logo
- Full blog content rendering:
    - Rich text content
    - Embedded images
    - Hyperlinks
    - Formatted text (headings, lists, bold, italic, etc.)
    - Markdown content from public APIs (parsed and rendered as HTML)
- Responsive design for mobile, tablet, and desktop.
- SEO-optimized page structure (meta tags, Open Graph).

### **Out of Scope**

- Comments section.
- Social sharing buttons.
- Related blog posts section.
- Print blog functionality.
- Reading time estimation.
- Table of contents for long blogs.
- Author bio section.
- Previous/Next blog navigation.
- Blog likes or reactions.
- Newsletter signup embedded in blog.

---

# **3) Key User Journeys**

---

### **Journey 1 – Customer Accesses Blog Detail Page**

1. Customer is on the blog listing page (`/blogs`).
2. Clicks on a blog card.
3. Navigates to `/blogs/:slug` (e.g., `/blogs/introducing-new-product-line`).
4. Blog detail page loads with full content.

---

### **Journey 2 – Customer Reads Blog Content**

1. Customer lands on `/blogs/:slug`.
2. Sees the **blog header** section containing:
    - **Featured image** (full-width or hero style)
    - **Blog title** (large, prominent heading)
    - **Tags** (displayed as clickable pills/badges, e.g., "Product Launch", "News")
    - **Creation date** (formatted, e.g., "Published on December 15, 2025")
    - **Shop name and logo** (merchant branding)
3. Scrolls down to read the **full blog content**:
    - Rich text with proper formatting
    - Embedded images within the content
    - Clickable hyperlinks (internal or external)
    - Headings, paragraphs, lists, blockquotes, etc.
4. Content is displayed in a clean, readable format with appropriate spacing and typography.

---

### **Journey 3 – Customer Shares Blog URL**

1. Customer reads a blog they find valuable.
2. Copies the page URL from the browser address bar.
3. Shares the link via messaging apps or social media.
4. Recipients can access the same blog via the shared link.

---

### **Journey 4 – Customer Accesses Invalid Blog Slug**

1. Customer navigates to `/blogs/non-existent-slug`.
2. **Expected behavior:**
    - Display a **404 error page** or "Blog not found" message.
    - Provide a link back to the blog listing page or homepage.

---

### **Journey 5 – Merchant Updates Blog Content**

1. Merchant edits and republishes a blog via the dashboard.
2. Customer visits the blog detail page.
3. Sees the **updated content** immediately (no caching issues).

---

### **Journey 6 – Responsive Reading Experience**

1. Customer accesses blog detail page on mobile device.
2. Page layout adjusts:
    - Featured image scales responsively.
    - Text is readable without horizontal scrolling.
    - Images within content resize appropriately.
    - Links are easily tappable.
3. Customer has a smooth reading experience on any device.

---

### **Journey 7 – Customer Views Markdown Blog from Public API**

1. Customer accesses a blog that was created from a public API source.
2. Blog content is in markdown format received from the API.
3. System parses the markdown content and renders it as properly formatted HTML:
    - Markdown headings (#, ##, ###) convert to HTML heading tags
    - Markdown lists (-, *, 1.) convert to HTML list elements
    - Markdown links convert to clickable hyperlinks
    - Markdown images display properly with alt text
    - Bold (**text**) and italic (*text*) formatting render correctly
    - Code blocks and inline code display with proper formatting
4. Customer sees the blog content formatted consistently with rich-text blogs.
5. All links open appropriately (external links in new tab).

---

# **4) Business Acceptance Criteria (BAC)**

### **BAC 1:** Blog detail page must be accessible at `/blogs/:slug` where `:slug` is the unique blog identifier.

### **BAC 2:** The page must display the following in the header section:

- Featured image
- Blog title
- Tags (if available)
- Creation date
- Shop name and logo

### **BAC 3:** The full blog content must render with proper formatting including:

- Rich text (headings, paragraphs, lists)
- Embedded images
- Hyperlinks (clickable and functional)
- Text styling (bold, italic, underline, etc.)

### **BAC 4:** The blog detail page must be fully responsive and provide optimal reading experience on mobile, tablet, and desktop devices.

### **BAC 5:** If a customer navigates to an invalid or non-existent blog slug, the page must display a 404 or "Blog not found" error.

### **BAC 6:** The page must be SEO-optimized with proper meta tags (title, description, keywords, Open Graph tags).

### **BAC 7:** Shop branding (name and logo) must be clearly visible on the blog page.

### **BAC 8:** All images within the blog content must load properly and scale responsively.

### **BAC 9:** External links in blog content must open in a new tab.

### **BAC 10:** The creation date must be formatted in a user-friendly, readable format.

### **BAC 11:** Blogs created from public APIs with markdown format must be supported:

- Markdown content from API sources must be parsed and rendered as HTML.
- Standard markdown syntax must be supported (headings, lists, links, images, bold, italic, code blocks).
- The rendered markdown must match the styling and layout of rich-text blogs.
- External links in markdown must open in new tabs.
- Images from markdown must load properly and scale responsively.

---

# **5) Changelog**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.1 | 2025-02-15 | - | Added support for markdown content from public APIs (BAC 11, Journey 7) |
| 1.0 | - | - | Initial release |
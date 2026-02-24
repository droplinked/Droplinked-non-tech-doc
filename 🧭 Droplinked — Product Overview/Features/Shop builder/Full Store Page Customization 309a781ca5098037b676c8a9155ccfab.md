# Full Store Page Customization

### Feature ID:

**[IAA-SBF-004]**

### Title:

**Full Store Page Customization**

### Category:

**Shop Builder | Storefront Design** | **Actors**: Merchant | **Channel**: Web Dashboard, Shopfront

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants need the ability to create custom pages and customize existing pages beyond the standard templates. They want to create blog pages, landing pages, about pages, and contact pages with custom layouts, as well as modify system pages like product detail pages to match their brand.

**Desired Outcome:**

- Merchants can create up to 20 custom pages per shop
- Page builder supports 15+ component types
- Each page has unique URL slug
- SEO settings include: title, meta description, OG image, canonical URL
- Mobile, tablet, and desktop preview modes
- Version history allows restoring previous versions

---

### 2) **Scope – In / Out**

**In Scope:**

- **Page Creation**
    - Create unlimited custom pages (limit: 20 per shop)
    - Pre-built page templates (Landing, About, Contact, Blog Index)
    - Page duplication
    - Version history (last 10 saves)
- **Page Builder**
    - Drag-and-drop page builder interface
    - Component library (Hero, Text, Image, Gallery, Products, Blog Grid, Form)
    - 15+ component types supported
    - Components can be dragged to reorder within a page
- **Page Customization**
    - Customize existing system pages (Product, Cart, Checkout wrapper)
    - Custom URL slug configuration (/page-name)
    - Page-level SEO settings (title, meta description, OG tags, canonical URL)
    - Page visibility controls (published, draft, password-protected)
    - Mobile-responsive design preview (mobile, tablet, desktop)
- **Navigation**
    - Navigation menu management
    - Navigation menu automatically includes published custom pages

**Out of Scope:**

- Custom CSS/HTML editing (merchant-facing code access)
- Third-party widget integrations
- A/B testing for page variants
- Dynamic content from external APIs
- Multi-language page variants
- Page-level analytics (uses global analytics)
- Custom checkout page design (checkout is separate system)
- Page templates marketplace
- AI-generated page designs

---

### 3) **Key User Journeys**

---

**Journey 1: Create Custom Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Designer | Page builder loads |
| 2 | Merchant | Clicks "Create New Page" | Template selection shown |
| 3 | Merchant | Selects Template (Landing/About/Contact/Blog) | Template loaded into builder |
| 4 | Merchant | Adds Components from Library | Components added to page |
| 5 | Merchant | Configures Component Styles | Styles applied |
| 6 | Merchant | Names Page and Sets URL Slug | URL validated |
| 7 | Merchant | Configures SEO Settings | SEO fields shown |
| 8 | Merchant | Publishes Page | Page goes live on storefront |

---

**Journey 2: Customize Existing Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Designer | Page builder loads |
| 2 | Merchant | Selects Existing Page (e.g., Product Detail Page) | Page loaded |
| 3 | Merchant | Enters "Advanced Edit Mode" | Advanced editing enabled |
| 4 | Merchant | Reorders/Adds/Removes Sections | Changes reflected in preview |
| 5 | Merchant | Customizes Component Styles | Styles updated |
| 6 | Merchant | Saves Changes | Updates saved to live page |

---

**Journey 3: Build Blog Landing Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Creates "Blog" Page | New page created |
| 2 | Merchant | Adds Blog Grid Component | Grid component added |
| 3 | Merchant | Configures Layout (Masonry, Grid, List) | Layout applied |
| 4 | Merchant | Sets Filtering Options | Filters configured |
| 5 | Merchant | Customizes Article Card Design | Design customized |
| 6 | Merchant | Adds Navigation Link to Footer/Header | Navigation updated |
| 7 | Merchant | Publishes | Blog content auto-populates |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Page Creation** |  |
| BAC-1 | Merchant can create up to 20 custom pages per shop (limit configurable) |
| BAC-2 | Page builder supports 15+ component types |
| BAC-3 | Components can be dragged to reorder within a page |
| BAC-4 | Each page has unique URL slug (/page-name) |
| BAC-5 | Pre-built page templates available (Landing, About, Contact, Blog Index) |
| **SEO & Visibility** |  |
| BAC-6 | SEO settings include: title, meta description, OG image, canonical URL |
| BAC-7 | Pages can be set as: Published, Draft, or Scheduled |
| BAC-8 | Mobile, tablet, and desktop preview modes available |
| BAC-9 | Changes publish immediately or scheduled for future date |
| **Navigation & Management** |  |
| BAC-10 | Navigation menu automatically includes published custom pages |
| BAC-11 | Page duplication creates copy with "(Copy)" suffix |
| BAC-12 | Version history allows restoring previous versions |
| BAC-13 | System pages (Home, Product, Cart) show "Customize" option |
| BAC-14 | Blog page type auto-connects to merchant's blog posts |
| **Performance** |  |
| BAC-15 | Page builder loads in < 3 seconds |
| BAC-16 | Auto-save every 30 seconds during editing |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-004 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Page Builder** | Visual drag-and-drop interface for creating custom pages |
| **Component** | Reusable UI element (Hero, Text, Image, etc.) |
| **System Page** | Built-in pages (Product, Cart, Checkout) |
| **Custom Page** | Merchant-created pages with unique content |
| **Page Template** | Pre-designed page layout |
| **URL Slug** | Human-readable URL path segment |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Component Library**

```
COMPONENT_LIBRARY = {
    layout: [
        { id: 'container', name: 'Container', config: ['width', 'padding', 'background'] },
        { id: 'grid', name: 'Grid', config: ['columns', 'gap', 'responsive'] },
        { id: 'divider', name: 'Divider', config: ['style', 'color', 'spacing'] }
    ],
    content: [
        { id: 'heading', name: 'Heading', config: ['level', 'text', 'alignment'] },
        { id: 'text', name: 'Text Block', config: ['content', 'formatting', 'alignment'] },
        { id: 'image', name: 'Image', config: ['src', 'alt', 'size', 'caption'] },
        { id: 'gallery', name: 'Image Gallery', config: ['images', 'layout', 'lightbox'] },
        { id: 'video', name: 'Video', config: ['url', 'autoplay', 'controls'] }
    ],
    commerce: [
        { id: 'product_grid', name: 'Product Grid', config: ['collection', 'limit', 'layout'] },
        { id: 'product_carousel', name: 'Product Carousel', config: ['products', 'autoplay'] },
        { id: 'featured_product', name: 'Featured Product', config: ['product_id', 'style'] }
    ],
    blog: [
        { id: 'blog_grid', name: 'Blog Grid', config: ['layout', 'posts_per_page', 'filters'] },
        { id: 'recent_posts', name: 'Recent Posts', config: ['count', 'show_excerpt'] },
        { id: 'blog_categories', name: 'Blog Categories', config: ['display_style'] }
    ],
    forms: [
        { id: 'contact_form', name: 'Contact Form', config: ['fields', 'recipient'] },
        { id: 'newsletter', name: 'Newsletter Signup', config: ['provider', 'list_id'] }
    ],
    hero: [
        { id: 'hero_section', name: 'Hero Section', config: ['background', 'content', 'cta'] },
        { id: 'hero_slider', name: 'Hero Slider', config: ['slides', 'autoplay', 'transition'] }
    ]
}

FUNCTION renderComponent(component_type, config):
    component_def = FIND COMPONENT_LIBRARY WHERE id == component_type

    // Validate required config
    FOR each required_field IN component_def.required:
        IF NOT config[required_field]:
            THROW validation_error

    // Render component
    RETURN component_def.renderer(config)
```

---

### **FL-2: Page Creation Flow**

```
ON CreateNewPageRequest:
    // Check page limit
    current_count = COUNT pages WHERE shop_id == merchant.shop_id
    IF current_count >= 20:
        RETURN error("Maximum 20 pages reached")

    // Show template selection
    templates = GET all_templates
    SHOW template_selection_modal(templates)

ON SelectTemplate(template_id):
    page = CREATE new Page()
    page.template_id = template_id
    page.components = GET template_components(template_id)
    page.shop_id = merchant.shop_id
    page.status = 'draft'

    // Open builder
    OPEN page_builder(page)

ON SavePage(page_id, components, settings):
    page = GET page WHERE id == page_id

    // Validate slug uniqueness
    IF settings.slug:
        existing = FIND page WHERE slug == settings.slug AND id != page_id
        IF existing:
            RETURN error("URL slug already in use")
        page.slug = settings.slug

    // Save components as JSON
    page.components = JSON.stringify(components)
    page.settings = {
        title: settings.title,
        meta_description: settings.meta_description,
        og_image: settings.og_image,
        canonical_url: settings.canonical_url,
        visibility: settings.visibility // 'published', 'draft', 'password_protected'
    }

    // Version history
    CREATE page_version {
        page_id: page_id,
        components: page.components,
        created_at: NOW,
        created_by: merchant.id
    }

    // Keep only last 10 versions
    DELETE oldest versions WHERE page_id == page_id LIMIT (total_versions - 10)

    SAVE page

    // Update navigation if published
    IF settings.visibility == 'published':
        UPDATE navigation_links ADD page

ON PublishPage(page_id, schedule_date = null):
    page = GET page WHERE id == page_id

    IF schedule_date AND schedule_date > NOW:
        page.scheduled_publish = schedule_date
        page.status = 'scheduled'
    ELSE:
        page.status = 'published'
        page.published_at = NOW

        // Invalidate cache
        CLEAR_CACHE page.url

    SAVE page
```

---

### **FL-3: Page Rendering**

```
ON PageRequest(slug):
    // Check if custom page
    page = FIND page WHERE slug == slug AND status == 'published'

    IF page:
        // Render custom page
        components = JSON.parse(page.components)

        html = ""
        FOR each component IN components:
            html += renderComponent(component.type, component.config)

        RETURN render_page({
            content: html,
            seo: page.settings,
            layout: page.template.layout
        })

    ELSE:
        // Check system routes
        RETURN system_router(slug)

FUNCTION renderSystemPage(page_type):
    // Get customization for system page
    customization = GET customization WHERE page_type == page_type

    IF customization:
        // Apply merchant's customizations
        components = JSON.parse(customization.components)
        RETURN renderWithCustomizations(page_type, components)
    ELSE:
        // Return default system page
        RETURN renderDefault(page_type)
```

---

### **FL-4: SEO and Meta Tags**

```
FUNCTION generateSEOMeta(page):
    meta_tags = []

    // Basic meta
    meta_tags.push({
        tag: 'title',
        content: page.settings.title OR page.name
    })

    meta_tags.push({
        tag: 'meta',
        name: 'description',
        content: page.settings.meta_description OR shop.default_description
    })

    // Open Graph
    meta_tags.push({
        tag: 'meta',
        property: 'og:title',
        content: page.settings.title
    })

    meta_tags.push({
        tag: 'meta',
        property: 'og:description',
        content: page.settings.meta_description
    })

    meta_tags.push({
        tag: 'meta',
        property: 'og:image',
        content: page.settings.og_image OR shop.default_og_image
    })

    meta_tags.push({
        tag: 'meta',
        property: 'og:url',
        content: page.settings.canonical_url OR page.full_url
    })

    // Canonical
    meta_tags.push({
        tag: 'link',
        rel: 'canonical',
        href: page.settings.canonical_url OR page.full_url
    })

    RETURN meta_tags
```

---

### B3) Behavioral Flow

```
[Merchant Opens Designer]
    ↓
[Choose Action]
    ├─ Create New Page → [Select Template]
    │                       ↓
    │                   [Load Builder]
    │                       ↓
    │                   [Add/Edit Components]
    │                       ↓
    │                   [Configure Settings]
    │                       ↓
    │                   [Save/Publish]
    │
    └─ Edit Existing Page → [Load Page]
                            ↓
                        [Make Changes]
                            ↓
                        [Auto-save]
                            ↓
                        [Publish Updates]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Draft | Publish clicked | Published | Page live, navigation updated |
| Draft | Schedule set | Scheduled | Will auto-publish on date |
| Scheduled | Date reached | Published | Page goes live |
| Published | Unpublish clicked | Draft | Page removed from storefront |
| Published | Update saved | Published | New version live |
| Any | Duplicate clicked | Draft (copy) | New page created with "(Copy)" suffix |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Slug conflicts with system route | Show error, suggest alternative |
| **EC-2:** Component fails to render | Show placeholder, log error |
| **EC-3:** Image URL broken | Show placeholder image |
| **EC-4:** Version history full | Delete oldest version automatically |
| **EC-5:** Scheduled publish fails | Notify merchant, keep as draft |
| **EC-6:** Page limit reached | Block creation, suggest deletion |
| **EC-7:** Navigation update fails | Page published but not in menu |
| **EC-8:** Auto-save fails | Show warning, manual save required |

---

### B6) Data Requirements

**Page Storage:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Unique identifier |
| shop_id | UUID | Reference to shop |
| name | String | Page name |
| slug | String | URL slug |
| template_id | String | Template used |
| components | JSON | Component structure |
| settings | JSON | SEO and visibility settings |
| status | Enum | draft, published, scheduled |
| scheduled_publish | DateTime | If scheduled |
| published_at | DateTime | When published |
| created_at | DateTime | Creation timestamp |
| updated_at | DateTime | Last update |

**Page Versions:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Version identifier |
| page_id | UUID | Reference to page |
| components | JSON | Component state |
| created_at | DateTime | When version saved |
| created_by | UUID | Who created version |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Create new page from template | Page created with template components |
| TC-2 | Add component to page | Component rendered in preview |
| TC-3 | Set unique URL slug | Slug saved and accessible |
| TC-4 | Configure SEO settings | Meta tags present in page HTML |
| TC-5 | Publish page | Page live on storefront |
| TC-6 | Duplicate page | Copy created with "(Copy)" suffix |
| TC-7 | View version history | Last 10 versions listed |
| TC-8 | Restore previous version | Page reverted to selected version |
| TC-9 | Mobile preview | Mobile layout displayed |
| TC-10 | Schedule publish | Page auto-publishes on date |
| TC-11 | Blog grid auto-populate | Blog posts displayed in grid |
| TC-12 | Page limit reached | Creation blocked at 20 pages |
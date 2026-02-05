# Shop front

[ Customer Login](Shop%20front/Customer%20Login%202b0a781ca5098016b7d1cea9a52cf1ae.md)

[Product Listing Page (PLP)](Shop%20front/Product%20Listing%20Page%20(PLP)%202b0a781ca5098007a7dac3ba5a6ab8a0.md)

[Product Detail Page (PDP)](Shop%20front/Product%20Detail%20Page%20(PDP)%202b0a781ca50980f491e8cd50e60f4c39.md)

[Cart](Shop%20front/Cart%202b3a781ca5098041ad94d8bbc332619c.md)

[Blog](Shop%20front/Blog%202d8a781ca50980939425fde75c21732f.md)

[Designer](Shop%20front/Designer%202d8a781ca50980dd83b6dd2a1534a6d2.md)

**Document Type:** High-Level Overview

**Last Updated:** December 29, 2025

---

# **1) System Overview**

The Droplinked Storefront is a fully customizable e-commerce platform that enables merchants to create, design, and manage their online shops. Customers can browse products, read blogs, manage their carts, and complete purchases through an intuitive interface.

### **Key Components**

1. **Shop Builder & Designer** – Merchant creates and customizes storefront
2. **Customer Authentication** – Login via Google or Web3 wallets
3. **Product Discovery** – Browse, search, filter, and sort products
4. **Blog Content** – Merchants publish blogs; customers read content
5. **Shopping Cart** – Add products, review, and modify before checkout
6. **Checkout** – Complete purchase (separate system)

---

# **2) High-Level User Flows**

## **Flow 1: Merchant Setup Flow**

```
Merchant → Shop Builder → Designer Canvas
  ↓
Configure Footer (Social Media + Custom Links)
  ↓
Add/Edit/Reorder Components
  ↓
Publish → Storefront Goes Live

```

**Key Features:**

- Drag-and-drop canvas editing
- Direct deletion (no confirmation modals)
- Publish button saves and deploys changes
- Footer customization with social media icons and custom link sections

---

## **Flow 2: Customer Browse & Discovery Flow**

```
Customer Lands on Storefront
  ↓
Browse Products (PLP)
  ↓
Apply Filters:
  • Price Range
  • Collection
  • Product Type (Physical/Digital)
  ↓
Sort Products:
  • Price (Low→High, High→Low)
  • Name (A→Z, Z→A)
  • Default
  ↓
Click Product → Product Detail Page (PDP)
  ↓
Select Variant → Add to Cart

```

**Key Features:**

- Responsive product listing with thumbnails
- Advanced filtering and sorting (4 sort options + default)
- POD products categorized as Physical
- Empty state handling

---

## **Flow 3: Customer Authentication Flow**

```
Customer Clicks Profile Icon
  ↓
Login Modal Opens (if not logged in)
  ↓
Select Login Method:
  • Google
  • Web3 Wallet (MetaMask, Phantom, Unstoppable Domains, etc.)
  ↓
Authenticate
  ↓
Profile Popup Menu Shows:
  • Wallet Address or Email
  • Avatar (first letter of email for email logins)
  • Logout Button
  ↓
Click Logout → Session Cleared

```

**Key Features:**

- Merchant controls which login methods are enabled
- First-time login creates customer account
- Returning users: cart restoration
- Profile menu accessible from header

---

## **Flow 4: Shopping Cart Flow**

```
Customer Adds Product to Cart (from PDP)
  ↓
Cart Icon Updates (item count)
  ↓
Click Cart Icon → Cart Overlay Opens
  ↓
View Items (thumbnail, title, SKU, quantity)
  ↓
Two Options:
  1. Clear Cart → All Items Removed
  2. Checkout Button → Navigate to Cart Page
  ↓
Cart Page (/cart)
  ↓
Review Items in Detail:
  • Product images (clickable → PDP)
  • Variant info
  • Cost per unit
  • Quantity controls (+ / -)
  • Line totals
  ↓
Modify Cart:
  • Increase/Decrease Quantity
  • Remove Individual Items
  • Clear Entire Cart
  ↓
Options:
  1. Continue Shopping → Return to Shop
  2. Checkout → Proceed to Checkout

```

**Key Features:**

- **Cart Overlay:** Quick view only (no editing)
- **Cart Page:** Full editing capabilities
- Quantity = 1 and click (-) → item removed
- Empty state for empty cart
- Cart auto-expires after 48 hours

---

## **Flow 5: Blog Discovery Flow**

```
Merchant Publishes Blogs
  ↓
Blog Link Appears in Footer (if blogs exist)
  ↓
Customer Clicks Blog Link
  ↓
Blog Listing Page (/blogs)
  ↓
View All Blogs:
  • Featured image
  • Title
  • Category
  • Creation date
  • Summary
  • Keywords
  ↓
Click Blog Card → Blog Detail Page (/blogs/:slug)
  ↓
Read Full Content:
  • Hero image
  • Title
  • Tags
  • Creation date
  • Shop branding
  • Rich content (text, images, links)

```

**Key Features:**

- Blog link only appears if merchant has published blogs
- Empty state when no blogs available
- Responsive grid layout
- SEO-optimized blog pages
- All external links open in new tabs

---

# **3) Feature Breakdown by Actor**

## **Merchant Features**

| Feature | Description |
| --- | --- |
| **Designer Canvas** | Drag-and-drop storefront builder with direct publish |
| **Footer Customization** | Add social media links (8 platforms) + 2 custom sections |
| **Blog Management** | Create and publish blog content (managed via dashboard) |
| **Login Methods** | Enable/disable authentication options for customers |

## **Customer Features**

| Feature | Description |
| --- | --- |
| **Product Browsing** | Filter, sort, search products across collections |
| **Authentication** | Login via Google or Web3 wallets |
| **Product Details** | View product info, select variants, add to cart |
| **Cart Management** | View in overlay, edit on cart page, proceed to checkout |
| **Blog Reading** | Browse and read merchant blog content |
| **Profile Menu** | View account info, logout |

---

# **4) Page Routes**

| Route | Page | Description |
| --- | --- | --- |
| `/` | Storefront Home | Main shop page (customized via Designer) |
| `/products` or `/shop` | Product Listing Page (PLP) | Browse all products with filters/sort |
| `/product/:slug` | Product Detail Page (PDP) | View individual product details |
| `/cart` | Cart Page | Review and modify cart before checkout |
| `/blogs` | Blog Listing Page | Browse all published blogs |
| `/blogs/:slug` | Blog Detail Page | Read full blog content |
| `/checkout` | Checkout | Complete purchase (separate feature) |

---

# **5) Key Business Rules**

### **Designer Canvas**

- Components deleted immediately without confirmation
- Publish button both saves and deploys changes
- No separate save button

### **Authentication**

- At least one login method must be enabled
- Profile popup shows wallet address or email after login
- Email login displays first letter as avatar

### **Cart System**

- Cart Overlay: view only + clear cart option
- Cart Page: full editing (add/remove/modify quantities)
- Cart expires after 48 hours of inactivity
- Decreasing quantity to 0 removes item

### **Blog System**

- Blog link appears in footer only if blogs exist
- All external links open in new tabs
- Empty state for no blogs available

### **Product Filtering & Sorting**

- 5 sort options: Default, Price (Low/High), Name (A-Z/Z-A)
- Filters: Price range, Collection, Product Type
- POD products included in Physical category
- Multiple filters combine with AND logic

---

# **6) Integration Points**

```
┌─────────────────────────────────────────────────┐
│           Droplinked Storefront                 │
│                                                 │
│  ┌──────────────┐      ┌──────────────┐        │
│  │   Designer   │      │  Storefront  │        │
│  │   (Merchant) │ ───► │  (Customer)  │        │
│  └──────────────┘      └──────────────┘        │
│         │                      │                │
│         │                      │                │
│         ▼                      ▼                │
│  ┌──────────────────────────────────┐          │
│  │      Product Database            │          │
│  │      Customer Accounts           │          │
│  │      Cart System                 │          │
│  │      Blog Content                │          │
│  └──────────────────────────────────┘          │
│                                                 │
└─────────────────────────────────────────────────┘
                     │
                     ▼
              External Systems:
              • Checkout/Payment
              • Web3 Wallets
              • Google Auth
              • Social Media Platforms

```

---

# **7) Responsive Design**

All pages and components are fully responsive:
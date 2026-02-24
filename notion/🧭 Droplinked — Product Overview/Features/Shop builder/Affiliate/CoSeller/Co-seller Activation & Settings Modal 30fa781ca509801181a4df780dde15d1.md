# Co-seller Activation & Settings Modal

**Feature ID:** IAA-AFF-006

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Co-seller)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

When merchants want to become Co-sellers and import their first product, they need to configure their Co-seller profile. Unlike Affiliators who have a settings page, Co-sellers configure their profile through a modal that appears when they try to import their first product. This profile information helps Affiliators understand who is selling their products.

### User Stories

- As a Merchant, I want to activate Co-seller mode so that I can import and sell other merchants' products.
- As a Merchant, I want to provide an introduction and strategy so that Affiliators can understand my sales approach.
- As a Merchant, I want to add my social media links so that Affiliators can see my marketing channels.
- As a Merchant, I want to specify my active sales channels so that Affiliators know where I sell.

### Key User Journeys

**Journey 1: First-Time Co-seller Activation (USD Required)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Browses Marketplace, finds product | Product detail page shown |
| 2 | Merchant | Clicks "Add Product" | System checks shop currency |
| 3 | System | Validates currency | IF not USD: Show error |
| 4 | Merchant (USD shop) | System opens Settings Modal | Modal with profile form |
| 5 | Merchant | Fills Introduction & Strategy | Text area, optional |
| 6 | Merchant | Adds Social Media Links | Instagram, Twitter, Facebook, etc. |
| 7 | Merchant | Selects Active Channels | Website, Social Media, Email, etc. |
| 8 | Merchant | Clicks "Save & Import" | Profile saved, product imported |
| 9 | System | Sets role to Co-seller | Role locked permanently |
| 10 | System | Product appears in shop | Available for sale |

**Journey 2: Edit Co-seller Settings (After First Import)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Navigates to Affiliate Network | Co-seller dashboard shown |
| 2 | Co-seller | Clicks "Settings" or "Edit Profile" | Settings modal opens |
| 3 | Co-seller | Updates Introduction, Social Links, Channels | Form fields editable |
| 4 | Co-seller | Clicks "Save Changes" | Profile updated |
| 5 | System | Updates partner info for Affiliators | Changes reflected immediately |

### Scope

**✅ In Scope:**

- **Activation Modal (First Import)**
    - Introduction & Strategy (textarea, optional)
    - Social Media Links (multiple: Instagram, Twitter, Facebook, LinkedIn, etc.)
    - Active Channels (checkboxes: Website, Social Media, Email Marketing, etc.)
    - Save & Import button
    - Cancel button
- **Settings Modal (Edit Mode)**
    - Same fields as activation
    - Save Changes button
    - Accessible from Co-seller dashboard
- **Validation**
    - USD currency check before opening modal
    - Role assignment after first import

**❌ Out of Scope:**

- Profile picture upload (uses shop logo)
- Contact information editing (uses shop email)
- Commission rate editing (set by Affiliator)
- Non-USD shops (blocked)
- Multiple imports at once (one by one)

### Acceptance Criteria

- [ ]  Modal opens when first-time merchant clicks "Add Product"
- [ ]  USD currency validated before modal opens
- [ ]  Introduction & Strategy field:
    - Textarea, optional
    - Max 1000 characters
    - Character counter shown
- [ ]  Social Media Links:
    - Instagram (URL)
    - Twitter (URL)
    - Facebook (URL)
    - LinkedIn (URL)
    - YouTube (URL)
    - All optional, URL validation
- [ ]  Active Channels (checkboxes, at least one required):
    - Website
    - Social Media
    - Email Marketing
    - Online Marketplaces
    - Other
- [ ]  "Save & Import" button disabled until at least one channel selected
- [ ]  Clicking "Save & Import" saves profile and imports product
- [ ]  Role set to "Co-seller" permanently after first import
- [ ]  Settings accessible from Co-seller dashboard after activation
- [ ]  Changes saved immediately and visible to Affiliators

### Technical Notes

- Modal is the entry point for Co-sellers (different from Affiliators who have a full page)
- Currency check must happen before showing modal
- Profile stored in merchant/shop table
- Changes propagate to Affiliator partner views
- Form validation on client and server

### Dependencies

- Shop currency validation
- Product import API
- Merchant profile update API
- Social media URL validation

---

## UI Flow (Source of Truth)

```
[Marketplace - Product Detail]
    ↓
[Click Add Product]
    ↓
[Check Currency]
    ├─ Not USD → [Error: "USD currency required"]
    └─ USD → [Continue]
        ↓
[Check Role]
    ├─ No Role (First Time) → [Settings Modal - Activation]
    │                           ├─ Introduction & Strategy
    │                           ├─ Social Media Links
    │                           └─ Active Channels
    │                               ↓
    │                           [Click Save & Import]
    │                               ↓
    │                           [Set Role = Co-seller]
    │                               ↓
    │                           [Product Imported]
    └─ Co-seller (Existing) → [Simple Confirm] → [Product Imported]

[Co-seller Dashboard]
    ↓
[Click Settings/Edit Profile]
    ↓
[Settings Modal - Edit Mode]
    ├─ Introduction & Strategy
    ├─ Social Media Links
    └─ Active Channels
        ↓
    [Click Save Changes]
        ↓
    [Profile Updated]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-007
- 🎨 **Design:** [Figma - Co-seller Settings Modal]
- 🧪 **Test Cases:** TC-AFF-071 through TC-AFF-085

---

## Change Log

- 2026-02-22 — Behdad — Created based on [init.md](http://init.md/) requirements (Modal instead of page)

---

##
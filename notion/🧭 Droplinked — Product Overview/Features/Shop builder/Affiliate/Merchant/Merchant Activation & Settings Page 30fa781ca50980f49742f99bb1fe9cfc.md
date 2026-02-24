# Merchant Activation & Settings Page

# Merchant Activation & Settings Page

**Feature ID:** IAA-AFF-001

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Affiliator)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Product owners want to leverage other merchants to sell their products without managing additional sales channels. They need a simple way to activate affiliate selling, configure their shop profile for the marketplace, and set commission rates for their products. This is the entry point for merchants who want to become Affiliators.

### User Stories

- As a Merchant, I want to activate Affiliate Network so that other merchants can sell my products.
- As a Merchant, I want to configure my shop profile (category, description) so that Co-sellers can find and trust my shop.
- As a Merchant, I want to set commission rates for my products so that Co-sellers are incentivized to sell them.
- As a Merchant, I want to enable/disable affiliate status so that I can control when my products are available.

### Key User Journeys

**Journey 1: First-Time Activation (USD Currency Required)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Affiliate Network" in sidebar | System checks shop currency |
| 2 | System | Validates currency | IF not USD: Show currency error page |
| 3 | Merchant (USD shop) | Views activation page | Message explaining affiliate + "Learn More" link |
| 4 | Merchant | Clicks "Activate Affiliate Network" | Modal opens with detailed explanation |
| 5 | Merchant | Reads modal content | Explains benefits and how it works |
| 6 | Merchant | Clicks "Activate Affiliate Network" in modal | System validates USD currency |
| 7 | System | Currency validation passed | Affiliate activated |
| 8 | System | Sets merchant role to Affiliator | Role locked permanently |
| 9 | System | Redirects to settings page | Settings page with configuration options |

**Journey 2: Configure General Information**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views General Information section | Shop URL, Category, Description shown |
| 2 | Affiliator | Clicks copy icon on URL | Marketplace URL copied to clipboard |
| 3 | Affiliator | Selects category from dropdown | Category saved |
| 4 | Affiliator | Enters description (max 500 chars) | Character count shown, saved on blur |
| 5 | System | Updates marketplace profile | Changes reflect on marketplace |

**Journey 3: Set Fixed Commission for All Products**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views Commission section | Two options displayed |
| 2 | Affiliator | Selects "Fixed commission for all products" | Input field appears |
| 3 | Affiliator | Enters commission rate (e.g., 15%) | Validates 1-99 range |
| 4 | Affiliator | Saves | All products now available on marketplace with 15% commission |
| 5 | System | Lists all products | Products appear on marketplace |

**Journey 4: Manual Product Selection with Custom Commissions**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Selects "Manual Selection" | Browse Inventory button appears |
| 2 | Affiliator | Clicks "Browse Inventory" | Product selection modal opens |
| 3 | Affiliator | Searches or filters products | Products filtered by title or collection |
| 4 | Affiliator | Selects products | Products added to selection list |
| 5 | Affiliator | For each product, enters commission rate | Input validates 1-99% |
| 6 | Affiliator | Saves selection | Selected products appear on marketplace |
| 7 | Affiliator | Can add more or remove products | List updates accordingly |

**Journey 5: Enable/Disable Affiliate**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views Additional Settings | Enable/Disable toggle shown |
| 2 | Affiliator | Toggles OFF | Confirmation shown |
| 3 | System | Disables affiliate | Products become "Out of Stock" for Co-sellers |
| 4 | System | Keeps sales history | Historical data preserved |
| 5 | Affiliator | Toggles ON | Products available again on marketplace |

### Scope

**✅ In Scope:**

- **Activation Flow**
    - First-time activation page with explanation
    - Activation modal with details
    - One-click activation (only for shops with USD currency)
    - Currency validation before activation
    - Redirect to settings page after activation
- **General Information Section**
    - Shop URL display with copy functionality
    - Category selection (dropdown with 10 categories)
    - Description field (max 500 characters with counter)
    - Auto-save or explicit save
- **Commission and Products Section**
    - Fixed commission mode (all products, one rate 1-99%)
    - Manual selection mode (per-product rates 1-99%)
    - Product browser modal with search and filter
    - Add/remove products from affiliate
    - Commission validation (1-99%)
- **Additional Settings**
    - Enable/disable affiliate toggle
    - Confirmation modal when disabling
- **Marketplace Visibility**
    - Products appear on marketplace when affiliate is enabled
    - Profile appears on marketplace

**❌ Out of Scope:**

- Multiple commission tiers
- Time-limited commissions
- Automatic commission adjustments
- Product bundles for affiliate
- Co-seller approval workflow
- Non-USD currency shops
- Trial plans

### Acceptance Criteria

- [ ]  Activation page shows for first-time users
- [ ]  Activation modal explains affiliate benefits clearly
- [ ]  Only USD shops can activate (validated)
- [ ]  After activation, merchant role is set to "Affiliator" permanently
- [ ]  Settings page loads after activation
- [ ]  Shop marketplace URL is displayed and copyable
- [ ]  Category dropdown shows 10 predefined categories
- [ ]  Description field allows max 500 characters with counter
- [ ]  Two commission modes available: Fixed and Manual
- [ ]  Commission rate must be between 1-99%
- [ ]  Product browser modal supports search by title
- [ ]  Product browser modal supports filter by collection
- [ ]  Selected products can be added or removed
- [ ]  Toggle controls affiliate visibility on marketplace
- [ ]  Disabling makes products "Out of Stock" for Co-sellers

### Technical Notes

- Currency validation must happen before any activation action
- Settings auto-save or show explicit save button
- Product selection modal should handle pagination for large inventories
- Commission validation on both client and server side
- When disabling affiliate, mark products as Out of Stock (don't delete)

### Dependencies

- Shop currency system
- Product catalog API
- Affiliate product listing API
- Category system

---

## UI Flow (Source of Truth)

```
[Sidebar: Affiliate Network]
    ↓
[Check Currency]
    ├─ Not USD → [Currency Error Page]
    └─ USD → [Continue]
        ↓
[Check Activation Status]
    ├─ Not Activated → [Activation Page]
    │                       ↓
    │                  [Click Activate]
    │                       ↓
    │                  [Show Modal]
    │                       ↓
    │                  [Confirm Activate]
    │                       ↓
    │                  [Set Role = Affiliator]
    │                       ↓
    └─ Activated → [Settings Page]
                        ↓
    [Configure General Info]
        ├─ Shop URL (copyable)
        ├─ Category (dropdown)
        └─ Description (500 chars max)
        ↓
    [Select Commission Mode]
        ├─ Fixed → [Set Rate 1-99% for All]
        └─ Manual → [Select Products + Set Individual Rates]
            ↓
    [Toggle Enable/Disable]
        ├─ Disable → [Confirm Modal] → [Products = Out of Stock]
        └─ Enable → [Products Available]
        ↓
    [Products Listed on Marketplace]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-002
- 🎨 **Design:** [Figma - Merchant Activation & Settings]
- 🧪 **Test Cases:** TC-AFF-001 through TC-AFF-020

---

## Change Log

- 2026-02-22 — Behdad — Complete rewrite based on [init.md](http://init.md/) requirements
- 2026-02-16 — Behdad — Added USD currency requirement
- 2026-02-01 — Behdad — Initial document creation

---

##
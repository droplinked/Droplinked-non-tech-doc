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

**Important Constraints:**
- Merchant must have at least one payment method connected (Stripe, PayPal, or Crypto wallet) before activating affiliate
- Single role per user: Once activated as Affiliator, merchant cannot become Co-seller

### User Stories

- As a Merchant, I want to activate Affiliate Network so that other merchants can sell my products.
- As a Merchant, I want to configure my shop profile (category, description) so that Co-sellers can find and trust my shop.
- As a Merchant, I want to set commission rates for my products so that Co-sellers are incentivized to sell them.
- As a Merchant, I want to enable/disable affiliate status so that I can control when my products are available.

### Key User Journeys

**Journey 1: First-Time Activation (USD Currency + Payment Method Required)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Affiliate Network" in sidebar | System checks shop currency |
| 2 | System | Validates currency | IF not USD: Show currency error page |
| 3 | System | Validates payment methods | IF no payment method connected: Show payment method required error |
| 4 | Merchant (USD + payment method) | Views activation page | Message explaining affiliate + "Learn More" link |
| 5 | Merchant | Clicks "Activate Affiliate Network" | Modal opens with detailed explanation |
| 6 | Merchant | Reads modal content | Explains benefits and how it works |
| 7 | Merchant | Clicks "Activate Affiliate Network" in modal | System validates USD currency and payment method |
| 8 | System | Validation passed | Affiliate activated |
| 9 | System | Sets merchant role to Affiliator | Role locked permanently |
| 10 | System | Redirects to settings page | Settings page with configuration options |

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

**Journey 4B: Update Commission Rate**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views Commission section | Current commission rate displayed |
| 2 | Affiliator | Clicks "Edit" or commission field | Commission input becomes editable |
| 3 | Affiliator | Enters new commission rate (e.g., changes 15% to 20%) | Validates 1-99 range |
| 4 | Affiliator | Saves changes | Commission updated immediately |
| 5 | System | Updates all products | New commission rate applied to all affiliate products instantly |
| 6 | System | Syncs with Co-seller shops | Commission rate updated in real-time on all Co-seller storefronts |
| 7 | System | Handles active orders | Orders currently in checkout/payment process use OLD commission rate |
| 8 | System | New orders use new rate | All new orders after update use NEW commission rate |

**Example Scenario:**
- 10:00 AM: Commission is 15%, Customer A adds product to checkout
- 10:05 AM: Affiliator changes commission to 20%
- 10:06 AM: Customer B adds product to checkout (sees 20%)
- 10:10 AM: Customer A completes purchase (uses 15% - old rate)
- 10:15 AM: Customer B completes purchase (uses 20% - new rate) |

**Journey 5: Deactivate Affiliate Sales**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views Additional Settings section | "Deactivate Affiliate Sales" toggle shown |
| 2 | Affiliator | Toggles OFF (Deactivate) | Warning modal opens |
| 3 | System | Shows warning modal | "All collaborations will be cancelled. Products will be removed from Co-seller shops. This cannot be undone." |
| 4 | Affiliator | Reads warning | Sees list of affected Co-sellers and product count |
| 5 | Affiliator | Clicks "Deactivate" in modal | System processes deactivation |
| 6 | System | Cancels all collaborations | All Co-seller partnerships ended |
| 7 | System | Removes all products | All affiliate products removed from Co-seller shops immediately |
| 8 | System | Saves settings | Affiliate sales deactivated |
| 9 | System | Shows success message | "Affiliate sales deactivated. All collaborations cancelled." |

**Journey 6: Remove Product from Affiliate**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views Products section in Settings | List of affiliate products shown |
| 2 | Affiliator | Clicks delete (×) icon next to product | Confirmation dialog opens |
| 3 | System | Shows confirmation | "This product will be removed from all Co-seller shops immediately." |
| 4 | Affiliator | Clicks "Remove" | Product removed from affiliate list |
| 5 | System | Removes product | Product immediately removed from all Co-seller shops |
| 6 | System | Updates settings | Product no longer available for affiliate |
| 7 | Affiliator | Can re-add product later | Product can be added back to affiliate list |

### Scope

**✅ In Scope:**

- **Activation Flow**
    - First-time activation page with explanation
    - Activation modal with details
    - One-click activation (only for shops with USD currency AND at least one payment method)
    - Currency validation before activation
    - Payment method validation (Stripe, PayPal, or Crypto wallet required)
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
    - **Remove Product functionality**: Remove specific products from affiliate network (immediate effect on all Co-seller shops)
- **Deactivate Affiliate Sales**
    - "Deactivate Affiliate Sales" toggle in Additional Settings
    - Warning modal with impact explanation
    - Cancels all collaborations on deactivation
    - Removes all products from Co-seller shops immediately
    - Irreversible action (can reactivate later but collaborations must be rebuilt)
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
- [ ]  **At least one payment method must be connected (Stripe, PayPal, or Crypto wallet)**
- [ ]  **If no payment method connected, show error: "Please connect at least one payment method (Stripe, PayPal, or Crypto wallet) to receive payouts"**
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
- [ ]  **"Deactivate Affiliate Sales" toggle available in Additional Settings**
- [ ]  **Clicking deactivate shows warning modal with list of affected Co-sellers**
- [ ]  **Deactivation cancels all collaborations and removes all products from Co-seller shops immediately**
- [ ]  **Commission rate can be updated at any time from settings**
- [ ]  **New commission rate applies immediately to all products**
- [ ]  **Co-seller shops sync immediately with new commission rate**
- [ ]  **Active checkouts/orders in progress use the OLD commission rate (snapshot at time of checkout start)**
- [ ]  **New orders after commission update use the NEW commission rate**
- [ ]  **Commission history is preserved for each order (stores the rate used at time of purchase)**
- [ ]  **Changing commission does NOT affect orders already completed or in payment processing**
- [ ]  **Removing a product immediately removes it from all Co-seller shops**
- [ ]  **Removed products can be re-added to affiliate later**
- [ ]  **Products in active checkout/cart can still be purchased even after removal (no disruption to ongoing purchases)**

### Technical Notes

- Currency validation must happen before any activation action
- Payment method validation: Check for connected Stripe account OR PayPal account OR crypto wallet
- Settings auto-save or show explicit save button
- Product selection modal should handle pagination for large inventories
- Commission validation on both client and server side (1-99%)
- **Commission Rate Updates:**
    - Commission rate stored in `affiliate_products.commission_rate`
    - When updated, new rate applies immediately to all products
    - Real-time sync to Co-seller shops via WebSocket or polling
    - **Active checkouts preserve commission rate at checkout start time (snapshot)**
    - Order stores `affiliate_commission_rate` field to preserve historical rate
    - Orders in progress use the rate that was active when checkout began
- **When deactivating affiliate: Set all collaborations to cancelled status, mark all imported_products as inactive**
- **When removing single product: Set is_active = false on affiliate_products record, trigger removal from all Co-seller shops**
- **Products in active cart/checkout are NOT affected by removal or commission changes (handled at order creation time)**

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
    └─ USD → [Check Payment Methods]
        ├─ No Payment Method → [Payment Method Required Error]
        └─ Has Payment Method → [Continue]
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
    [Manage Products List]
        ├─ Remove Product (× icon) → [Confirm] → [Remove from all Co-seller shops]
        └─ Add More Products
        ↓
    [Deactivate Affiliate Sales]
        ├─ Click Deactivate → [Warning Modal] → [Show Affected Partners]
        └─ Confirm → [Cancel all collaborations] → [Remove all products]
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

- 2026-02-25 — Behdad — Added Commission Rate Update feature (Journey 4B) with real-time sync and active checkout handling (uses old rate for in-progress orders)
- 2026-02-25 — Behdad — Added Payment Method requirement, Deactivate Affiliate Sales, and Remove Product functionality
- 2026-02-22 — Behdad — Complete rewrite based on [init.md](http://init.md/) requirements
- 2026-02-16 — Behdad — Added USD currency requirement
- 2026-02-01 — Behdad — Initial document creation

---
- 2026-02-01 — Behdad — Initial document creation

---

##
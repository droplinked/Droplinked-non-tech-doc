# Multi-Store Management

# Multi-Store Management

### Feature ID:

**[IAA-STM-402]**

### Title:

**Multi-Store Management – Create & Switch Stores**

### Category:

**Store Management** | **Actors**: Merchant | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants may need to operate multiple stores for different brands, product lines, or business ventures. Currently, each merchant account is tied to a single store. This feature enables merchants to create additional stores and switch between them seamlessly, while also displaying stores they've been invited to as team members.

**Desired Outcome:**

- Merchants can create up to 5 stores from their account.
- Merchants can switch between their owned stores and stores they're members of.
- Each store is independent with its own products, orders, settings, and team.
- Store creation is simplified (Name, URL, Logo only – no full onboarding).
- Clear visual distinction between owned stores and member stores.
- Logical default store selection on login.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Store Switcher UI**
    - Located in sidebar profile card section
    - Shows current active store with visual indicator (circle around profile picture)
    - Click on profile card reveals store list dropdown
    - Visual indicator for currently selected store (circle around profile picture)
- **Store List**
    - Display all stores user owns (Admin) - maximum 5 stores
    - Display all stores user is member of (Member) - unlimited
    - "New Store" option at the bottom (only if user has < 5 owned stores)
    - Store limit indicator when approaching limit
- **Create New Store**
    - Modal with 3 fields: Store Name, Store URL, Store Logo
    - Same validation rules as onboarding
    - Creates new store with user as Admin
    - Maximum 5 stores per user
- **Switch Store**
    - Click on store to switch
    - Loading state during switch
    - Dashboard reloads with new store context
- **Default Store on Login**
    - Priority: Oldest owned store → Oldest member store

**Out of Scope:**

- Store deletion
- Transfer of store ownership
- Different permission levels for members
- Store archiving
- Store duplication/cloning
- Bulk store management

---

### 3) **Key User Journeys**

---

**Journey 1: User Views Store List**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks on profile card in sidebar | Store list dropdown appears |
| 2 | System | Displays stores | Owned stores shown first, then member stores |
| 3 | System | Shows current store | Circle indicator around active store's profile picture |
| 4 | System | Shows store URLs | Each store shows name and URL subdomain |
| 5 | Merchant | Sees "+ New Store" option | Option visible if user has < 5 owned stores |

---

**Journey 2: User Switches to Another Store**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens store switcher | List of stores displayed |
| 2 | Merchant | Clicks on a different store | Loading state begins |
| 3 | System | Loads new store context | Fetches store data, products, orders, etc. |
| 4 | System | Updates UI | Dashboard reflects new store |
| 5 | System | Updates sidebar | Store name/logo in sidebar updates |
| 6 | Merchant | Continues working | All actions now apply to new store |

---

**Journey 3: User Creates New Store**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens store switcher | Store list displayed |
| 2 | Merchant | Clicks "+ New Store" | Create Store modal opens |
| 3 | Merchant | Enters Store Name | Validation: required, max 50 chars |
| 4 | Merchant | Enters Store URL | Validation: unique, lowercase, no special chars |
| 5 | Merchant | Uploads Store Logo | Validation: JPEG/PNG, max 5MB |
| 6 | Merchant | All fields valid | "Create Store" button enabled |
| 7 | Merchant | Clicks "Create Store" | Loading state |
| 8 | System | Creates new store | Store created with user as Admin |
| 9 | System | Switches to new store | Dashboard loads with new store |
| 10 | System | Data isolation | All data (products, orders, settings) are from new store only |
| 11 | Merchant | Begins using new store | Can add products, configure settings |

---

**Journey 4: User at Store Limit**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Has 5 owned stores | Maximum reached |
| 2 | Merchant | Opens store switcher | List shows 5 stores |
| 3 | System | Hides "+ New Store" | Option not visible |
| 4 | Merchant | Cannot create more stores | Limit enforced |

---

**Journey 5: User Logs In with Multiple Stores**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Logs into Droplinked | Authentication successful |
| 2 | System | Determines default store | Applies priority logic |
| 3 | System | Priority 1: Oldest owned store | If user owns stores, select oldest |
| 4 | System | Priority 2: Oldest member store | If no owned stores, select oldest member store |
| 5 | System | Loads default store | Dashboard opens with selected store |
| 6 | Merchant | Can switch if needed | Store switcher available |

---

**Journey 6: New User Creates First Store (Post-Onboarding)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Completes onboarding | First store created |
| 2 | System | Stores count = 1 | User is Admin of first store |
| 3 | Merchant | Opens store switcher | Shows 1 store + New Store option |
| 4 | Merchant | Can create up to 4 more | Total limit is 5 |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Store Switcher** |  |
| BAC-1 | Store switcher is accessible by clicking profile card in sidebar |
| BAC-2 | Clicking profile card reveals list of all accessible stores |
| BAC-3 | Current active store has circle indicator around profile picture |
| BAC-4 | Each store entry shows Store Name and Store URL |
| BAC-5 | Owned stores (Admin) are listed before member stores |
| **Store Switching** |  |
| BAC-6 | Clicking a store initiates switch with loading indicator |
| BAC-7 | After switch, dashboard reflects the new store's data |
| BAC-8 | Sidebar updates to show new store name/logo |
| BAC-9 | All subsequent actions apply to the newly selected store |
| **Store Creation** |  |
| BAC-10 | "+ New Store" option appears in store list if user has < 5 owned stores |
| BAC-11 | Clicking "+ New Store" opens creation modal |
| BAC-12 | Modal requires: Store Name, Store URL, Store Logo |
| BAC-13 | Store Name: required, max 50 characters |
| BAC-14 | Store URL: required, unique, lowercase alphanumeric and hyphens only, max 50 chars |
| BAC-15 | Store URL validates against reserved words (same as onboarding) |
| BAC-16 | Store Logo: JPEG/JPG/PNG only, max 5MB |
| BAC-17 | "Create Store" button disabled until all validations pass |
| BAC-18 | On creation, user is set as Admin of new store |
| BAC-19 | After creation, user is automatically switched to the new store |
| BAC-20 | After switching to new store, all data displayed must be from new store only (no data from other stores) |
| **Store Limit** |  |
| BAC-20 | Maximum 5 owned stores per user |
| BAC-21 | "+ New Store" option hidden when limit reached |
| BAC-22 | Being a member of other stores does NOT count toward the 5-store limit |
| **Default Store** |  |
| BAC-23 | On login, default store is selected automatically |
| BAC-24 | Priority: Oldest owned store first, then oldest member store |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature |
| 2026-02-19 | Behdad | Updated flow: profile card click, active store indicator (circle), data isolation requirement, unlimited member stores | UX Improvements |

---

---

## UI Flow (Source of Truth)

```
[Sidebar Profile Card with Active Store Indicator]
    ↓
[Click Profile Card]
    ↓
[Store List Dropdown Opens]
    ↓
[Display: Owned Stores (oldest first) → Member Stores (oldest first)]
    │
    ├─ [Active Store: Circle around profile picture]
    │
    ├─ [Click Different Store] → [Loading State] → [Switch Context] → [Reload Dashboard with New Store Data]
    │
    └─ [Click "+ New Store"] (if user has < 5 owned stores)
        ↓
    [New Store Modal Opens]
        ↓
    [Fill: Store Name, URL, Logo]
        ↓
    [All Validations Pass] → [Create Store Button Enabled]
        ↓
    [Click Create Store]
        ↓
    [Create Store] → [Auto-Switch to New Store] → [Dashboard Loads with New Store Data Only]
```

---

## Attachments

- 🎨 **Design:** [Figma - Multi-Store Management]
- 🧪 **Test Cases:** [Test Cases Document]

---

### Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** User has no stores (new user) | Redirect to onboarding |
| **EC-2:** User only has member stores (no owned) | Show member stores, New Store option available |
| **EC-3:** User at 5 owned store limit | Hide "+ New Store" option |
| **EC-4:** URL already taken | Show error, disable create button |
| **EC-5:** Logo upload fails | Show error, allow retry, create button stays disabled |
| **EC-6:** Network error during store creation | Show error, allow retry |
| **EC-7:** User removed from member store while active | On next API call, redirect to default store |
| **EC-8:** Last owned store deleted (if ever allowed) | Switch to member store or redirect to onboarding |
| **EC-9:** Browser refresh during switch | Page reloads with last known active store |
| **EC-10:** Concurrent store creation with same URL | Second request fails with "URL taken" error |
| **EC-11:** User enters URL with spaces | Auto-stripped, show clean URL |
| **EC-12:** User enters URL with uppercase | Auto-converted to lowercase |
| **EC-13:** Data from other stores visible after switch | All data must be from current store only |
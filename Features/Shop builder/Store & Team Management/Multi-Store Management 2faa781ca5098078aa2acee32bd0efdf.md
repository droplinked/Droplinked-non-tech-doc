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
    - Located in sidebar profile section
    - Shows current active store
    - Click reveals store list dropdown
    - Visual indicator for currently selected store
- **Store List**
    - Display all stores user owns (Admin)
    - Display all stores user is member of (Member)
    - "New Store" option at the bottom
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
| 1 | Merchant | Clicks on profile/store section in sidebar | Store switcher panel opens |
| 2 | Merchant | Clicks "Switch Store" | Store list dropdown appears |
| 3 | System | Displays stores | Owned stores shown first, then member stores |
| 4 | System | Shows current store | Checkmark or highlight on active store |
| 5 | System | Shows store URLs | Each store shows name and URL subdomain |
| 6 | Merchant | Sees "+ New Store" option | Option visible if under 5 owned stores |

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
| 10 | Merchant | Begins using new store | Can add products, configure settings |

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
| BAC-1 | Store switcher is accessible from sidebar profile section |
| BAC-2 | Clicking "Switch Store" reveals list of all accessible stores |
| BAC-3 | Current active store is visually highlighted (checkmark/highlight) |
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

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Owned Store** | A store where the user is the Admin (original creator) |
| **Member Store** | A store where the user was invited and accepted as a team member |
| **Active Store** | The currently selected store in the user's session |
| **Store Limit** | Maximum number of stores a user can own (5) |
| **Store Switcher** | UI component for viewing and changing active store |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Load Store List**

```
ON OpenStoreSwitcher:
    allStores = []

    // Get owned stores (user is admin)
    ownedStores = API.getStoresByOwner(currentUser.id)
    ownedStores.sort(by: created_at ASC)  // Oldest first
    FOR EACH store IN ownedStores:
        store.role = 'admin'
        store.displayRole = 'Admin'
        allStores.push(store)

    // Get member stores (user is invited member)
    memberStores = API.getStoresByMember(currentUser.id)
    memberStores.sort(by: added_at ASC)  // Oldest first
    FOR EACH store IN memberStores:
        store.role = 'member'
        store.displayRole = 'Member'
        allStores.push(store)

    // Determine if can create new store
    canCreateNew = ownedStores.length < 5

    RENDER storeSwitcher(allStores, canCreateNew, activeStoreId)

```

---

### **FL-2: Store Switcher UI Rendering**

```
FUNCTION renderStoreSwitcher(stores, canCreateNew, activeId):
    FOR EACH store IN stores:
        RENDER StoreItem:
            - logo: store.logo_url OR defaultLogo
            - name: store.name
            - url: store.url + ".droplinked.io"
            - isActive: store.id == activeId
            - roleTag: store.displayRole (shown subtly)

    IF canCreateNew:
        RENDER NewStoreButton("+ New Store")
    ELSE:
        // Don't render new store option

```

---

### **FL-3: Switch Store**

```
ON ClickStore(storeId):
    IF storeId == activeStoreId:
        CLOSE switcher
        RETURN

    SET loading = true
    SHOW loadingOverlay("Switching store...")

    response = API.switchStore(storeId)

    IF response.success:
        SET activeStoreId = storeId
        SET SESSION.currentStore = response.storeData

        // Update UI
        UPDATE sidebar.storeName = response.storeData.name
        UPDATE sidebar.storeLogo = response.storeData.logo_url

        // Reload dashboard data
        REFRESH dashboardData()

        CLOSE switcher
    ELSE:
        SHOW error "Failed to switch store"

    SET loading = false

```

---

### **FL-4: Open Create Store Modal**

```
ON ClickNewStore:
    IF ownedStoresCount >= 5:
        SHOW error "You've reached the maximum of 5 stores"
        RETURN

    CLOSE storeSwitcher
    OPEN CreateStoreModal:
        - storeNameInput (empty)
        - storeURLInput (empty)
        - storeLogoUpload (empty)
        - createButton (disabled)
        - discardButton

```

---

### **FL-5: Store Name Validation**

```
ON StoreNameChange(value):
    IF value.trim() == "":
        SET nameError = "Store name is required"
    ELSE IF value.length > 50:
        SET nameError = "Maximum 50 characters allowed"
    ELSE:
        SET nameError = null

    validateCreateButton()

```

---

### **FL-6: Store URL Validation**

```
reservedWords = [
    'admin', 'api', 'support', 'droplinked', 'www',
    'mail', 'ftp', 'help', 'shop', 'store',
    'checkout', 'cart', 'account', 'login',
    'signup', 'dashboard'
]

ON StoreURLChange(value):
    cleanValue = value.toLowerCase().replace(/[^a-z0-9-]/g, '')

    // Update input with cleaned value
    storeURLInput.value = cleanValue

    IF cleanValue == "":
        SET urlError = "Store URL is required"
        validateCreateButton()
        RETURN

    IF cleanValue.length > 50:
        SET urlError = "Maximum 50 characters allowed"
        validateCreateButton()
        RETURN

    IF cleanValue IN reservedWords:
        SET urlError = "This URL is reserved"
        validateCreateButton()
        RETURN

    // Check uniqueness (debounced)
    DEBOUNCE 300ms:
        response = API.checkURLUniqueness(cleanValue)
        IF response.available == false:
            SET urlError = "This URL is already taken"
        ELSE:
            SET urlError = null
            SET urlIsUnique = true

        validateCreateButton()

```

---

### **FL-7: Store Logo Validation**

```
ON LogoUpload(file):
    IF file.type NOT IN ['image/jpeg', 'image/jpg', 'image/png']:
        SHOW error "Only JPEG, JPG, PNG files are allowed"
        RETURN

    IF file.size > 5MB:
        SHOW error "File size must be less than 5MB"
        RETURN

    // Upload to server
    response = API.uploadLogo(file)

    IF response.success:
        SET storeLogo = response.url
        SET logoError = null
        SHOW logoPreview(response.url)
    ELSE:
        SET logoError = "Failed to upload logo"

    validateCreateButton()

```

---

### **FL-8: Create Button State**

```
FUNCTION validateCreateButton():
    isValid = (
        storeName.value.trim() != "" AND
        storeName.error == null AND
        storeURL.value.trim() != "" AND
        storeURL.error == null AND
        storeURL.isUnique == true AND
        storeLogo != null AND
        storeLogo.error == null
    )

    createButton.disabled = !isValid

```

---

### **FL-9: Create Store**

```
ON ClickCreateStore:
    SET loading = true

    payload = {
        name: storeName.value.trim(),
        url: storeURL.value,
        logo_url: storeLogo,
        owner_id: currentUser.id
    }

    response = API.createStore(payload)

    IF response.success:
        newStoreId = response.store.id

        CLOSE modal

        // Switch to new store
        SET activeStoreId = newStoreId
        SET SESSION.currentStore = response.store

        // Update UI
        UPDATE sidebar.storeName = response.store.name
        UPDATE sidebar.storeLogo = response.store.logo_url

        REFRESH dashboardData()

        SHOW success "Store created successfully"
    ELSE:
        SHOW error response.message

    SET loading = false

```

---

### **FL-10: Default Store Selection on Login**

```
ON SuccessfulLogin:
    stores = API.getAllUserStores(currentUser.id)

    IF stores.length == 0:
        // New user, hasn't completed onboarding
        REDIRECT to /onboarding
        RETURN

    // Separate owned and member stores
    ownedStores = stores.filter(s => s.role == 'admin')
                        .sort(by: created_at ASC)
    memberStores = stores.filter(s => s.role == 'member')
                         .sort(by: added_at ASC)

    defaultStore = null

    IF ownedStores.length > 0:
        defaultStore = ownedStores[0]  // Oldest owned
    ELSE IF memberStores.length > 0:
        defaultStore = memberStores[0]  // Oldest member

    IF defaultStore:
        SET activeStoreId = defaultStore.id
        SET SESSION.currentStore = defaultStore
        REDIRECT to /dashboard
    ELSE:
        // Edge case: all stores were deleted/removed
        REDIRECT to /onboarding

```

---

### **FL-11: Discard Store Creation**

```
ON ClickDiscard:
    IF anyFieldHasValue():
        SHOW confirmDialog("Discard changes? Your store won't be created.")
        IF confirmed:
            CLOSE modal
    ELSE:
        CLOSE modal

```

---

### B3) Behavioral Flow

```
[User Logged In]
    ↓
[Click Profile/Store Section in Sidebar]
    ↓
[Click "Switch Store"]
    ↓
[Load Store List]
    ↓
[Display: Owned Stores (oldest first) → Member Stores (oldest first)]
    │
    ├─ [Click Existing Store] → [Loading] → [Switch Context] → [Reload Dashboard]
    │
    └─ [Click "+ New Store"] (if < 5 owned)
        ↓
    [Open Create Store Modal]
        ↓
    [Fill: Name, URL, Logo]
        ↓
    [All Valid] → [Create Button Enabled]
        ↓
    [Click Create]
        ↓
    [Create Store] → [Switch to New Store] → [Dashboard Loads]

```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Dashboard (Store A) | Switch to Store B | Dashboard (Store B) | All data reloads for Store B |
| Dashboard | Open switcher | Switcher Open | Load store list |
| Switcher Open | Click store | Switching | Loading overlay shown |
| Switching | Load complete | Dashboard (new store) | UI updated |
| Switcher Open | Click New Store | Modal Open | Create modal shown |
| Modal Open | Click Create | Creating | Loading state |
| Creating | Success | Dashboard (new store) | New store active |
| Creating | Failure | Modal Open | Error shown, can retry |
| Modal Open | Click Discard | Switcher Open | Modal closed |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** User has no stores (new user) | Redirect to onboarding |
| **EC-2:** User only has member stores (no owned) | Show member stores, New Store option available |
| **EC-3:** User at 5 store limit | Hide "+ New Store" option |
| **EC-4:** URL already taken | Show error, disable create button |
| **EC-5:** Logo upload fails | Show error, allow retry, create button stays disabled |
| **EC-6:** Network error during store creation | Show error, allow retry |
| **EC-7:** User removed from member store while active | On next API call, redirect to default store |
| **EC-8:** Last owned store deleted (if ever allowed) | Switch to member store or redirect to onboarding |
| **EC-9:** Browser refresh during switch | Page reloads with last known active store |
| **EC-10:** Concurrent store creation with same URL | Second request fails with "URL taken" error |
| **EC-11:** User enters URL with spaces | Auto-stripped, show clean URL |
| **EC-12:** User enters URL with uppercase | Auto-converted to lowercase |

---

### B6) Data Requirements

**Store Table (relevant fields):**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Unique store ID |
| name | String | Store display name |
| url | String | Unique URL subdomain |
| logo_url | String | Store logo URL |
| owner_id | UUID | User ID of store creator (Admin) |
| created_at | DateTime | When store was created |
| theme_id | String | Selected theme (default if new) |

**User Store Access (for member stores):**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Unique record ID |
| store_id | UUID | Store ID |
| user_id | UUID | User ID |
| role | Enum | admin, member |
| added_at | DateTime | When access was granted |

**API Endpoints:**

| Endpoint | Method | Purpose |
| --- | --- | --- |
| `/api/users/{id}/stores` | GET | Get all stores user has access to |
| `/api/stores` | POST | Create new store |
| `/api/stores/check-url` | POST | Check URL uniqueness |
| `/api/stores/{id}/switch` | POST | Switch active store context |
| `/api/upload/logo` | POST | Upload store logo |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Open store switcher | Shows list of all accessible stores |
| TC-2 | Owned stores listed first | Owned stores appear above member stores |
| TC-3 | Current store highlighted | Active store has visual indicator |
| TC-4 | Switch to different store | Dashboard reloads with new store data |
| TC-5 | Create new store with valid data | Store created, user switched to it |
| TC-6 | Create store with taken URL | Error shown, cannot create |
| TC-7 | Create store with reserved URL | Error shown, cannot create |
| TC-8 | User has 5 stores | "+ New Store" option hidden |
| TC-9 | User has 4 stores | "+ New Store" option visible |
| TC-10 | Upload invalid logo format | Error shown |
| TC-11 | Upload oversized logo | Error shown |
| TC-12 | Login with multiple stores | Default to oldest owned store |
| TC-13 | Login with only member stores | Default to oldest member store |
| TC-14 | Discard store creation with data | Confirmation shown |
| TC-15 | Discard store creation empty form | Modal closes immediately |
| TC-16 | URL with uppercase entered | Auto-converted to lowercase |
| TC-17 | Member stores don't count toward limit | User can create 5 stores regardless of member stores |
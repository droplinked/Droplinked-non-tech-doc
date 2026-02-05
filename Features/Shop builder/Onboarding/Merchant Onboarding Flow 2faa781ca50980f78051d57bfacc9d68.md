# Merchant Onboarding Flow

# Merchant Onboarding Flow

### Feature ID:

**[IAA-ONB-300]**

### Title:

**Merchant Onboarding Flow (2-Step)**

### Category:

**Onboarding** | **Actors**: Merchant | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
New merchants need a quick and simple onboarding experience to set up their store and start selling. The previous multi-step onboarding was lengthy and caused drop-offs. A streamlined 2-step flow reduces friction, captures essential business information, and gets merchants to their dashboard faster.

**Desired Outcome:**

- Complete onboarding in just 2 steps.
- **Step 1:** Capture merchant segment/category for tracking and customization.
- **Step 2:** Set up store identity (theme, logo, name, URL) with real-time preview.
- After completion, show a welcome modal with tutorial video on first dashboard visit.
- Prevent users from returning to onboarding after completion.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Step 1 - What Best Describes You?**
    - Display 5 business category options
    - Store selection for tracking/segmentation
    - Disable "Next" until selection is made
- **Step 2 - Store Identity**
    - Theme selection with real-time preview
    - Logo upload with validation
    - Store Name (mandatory)
    - Store URL (mandatory, unique, validated)
    - Real-time preview panel
    - "Finish" button enabled only when all mandatory fields valid
- **Post-Onboarding**
    - Redirect to dashboard
    - Show welcome modal with tutorial video (first time only)
    - Mark onboarding as complete
    - Block access to onboarding pages after completion

**Out of Scope:**

- Currency selection (removed from onboarding)
- Subscription plan selection (moved to post-onboarding)
- Social media connection
- Inspirational inventory creation
- AI-assisted generation of shop details
- Import from existing website

---

### 3) **Key User Journeys**

---

**Journey 1: New User Completes Onboarding**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Completes registration | Redirected to Onboarding Step 1 |
| 2 | Merchant | Views Step 1 "What Best Describes You?" | 5 options displayed, Next disabled |
| 3 | Merchant | Selects one option | Next button becomes enabled |
| 4 | Merchant | Clicks Next | Proceeds to Step 2 |
| 5 | Merchant | Views Step 2 "Store Identity" | Theme selector + form + preview panel shown |
| 6 | Merchant | Selects theme | Preview updates immediately |
| 7 | Merchant | Uploads logo | Logo validated and shown in preview |
| 8 | Merchant | Enters Store Name | Preview updates with name |
| 9 | Merchant | Enters Store URL | System validates uniqueness in real-time |
| 10 | Merchant | All fields valid | Finish button becomes enabled |
| 11 | Merchant | Clicks Finish | Onboarding marked complete, redirect to dashboard |
| 12 | System | First dashboard visit | Welcome modal with tutorial video opens |
| 13 | Merchant | Watches/closes video | Modal closes, flag set to not show again |

---

**Journey 2: User Returns Before Completing Onboarding**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Leaves onboarding mid-way | Session ends |
| 2 | Merchant | Logs in again | Redirected to onboarding (where they left off) |
| 3 | Merchant | Completes remaining steps | Normal flow continues |

---

**Journey 3: User Tries to Access Onboarding After Completion**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Completed onboarding previously | `onboarding_completed = true` |
| 2 | Merchant | Tries to navigate to /onboarding | System redirects to dashboard |
| 3 | Merchant | Cannot access onboarding | Route guard blocks access |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| **Step 1** |  |  |
| BAC-1 | Step 1 displays exactly 5 category options |  |
| BAC-2 | "Next" button is DISABLED until one option is selected |  |
| BAC-3 | Selected category is stored in user/shop profile for tracking |  |
| **Step 2** |  |  |
| BAC-4 | Theme selector is displayed with one theme pre-selected |  |
| BAC-5 | Changing theme updates the preview panel immediately |  |
| BAC-6 | Logo upload accepts only JPEG/JPG/PNG, max 5MB |  |
| BAC-7 | Store Name is mandatory, max 50 characters |  |
| BAC-8 | Store URL is mandatory, max 50 characters, lowercase alphanumeric and hyphens only |  |
| BAC-9 | Store URL is validated for uniqueness in real-time |  |
| BAC-10 | Reserved words (admin, api, support, droplinked, etc.) are blocked for URL |  |
| BAC-11 | "Finish" button is DISABLED until Name and URL are valid |  |
| **Post-Onboarding** |  |  |
| BAC-12 | On Finish, user is redirected to dashboard |  |
| BAC-13 | Welcome modal with tutorial video opens on first dashboard visit |  |
| BAC-14 | Welcome modal does NOT appear on subsequent visits or refreshes |  |
| BAC-15 | User cannot navigate back to /onboarding after completion |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Complete rewrite - 2-step flow replaces multi-step | Major UX Improvement |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Business Category** | Merchant's self-selected segment (e.g., Retailer, Designer, Affiliate) |
| **Store URL** | Unique subdomain identifier for the merchant's shop (e.g., [mystore.droplinked.com](http://mystore.droplinked.com/)) |
| **Reserved Words** | URLs that are blocked for system use (admin, api, support, etc.) |
| **Onboarding Complete Flag** | Boolean flag indicating user has finished onboarding |
| **Tutorial Seen Flag** | Boolean flag indicating user has seen the welcome video |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Onboarding Entry Logic**

```
ON User Login/Registration:
    IF user.onboarding_completed == false:
        REDIRECT to /onboarding/step-1
    ELSE:
        REDIRECT to /dashboard

```

---

### **FL-2: Step 1 - Business Category Selection**

**Options Displayed:**

| # | Option Text |
| --- | --- |
| 1 | Existing Merchant or Retailer |
| 2 | Designer, Developer or Vibe Coder |
| 3 | Affiliate Seller |
| 4 | Manufacturer |
| 5 | Other |

**Button State Logic:**

```
nextButton.disabled = (selectedCategory == null)

ON OptionSelect(category):
    selectedCategory = category
    nextButton.disabled = false

ON NextClick:
    SAVE user.business_category = selectedCategory
    NAVIGATE to /onboarding/step-2

```

---

### **FL-3: Step 2 - Store Identity Form**

**Layout:**

| Left Side | Right Side |
| --- | --- |
| Theme Selector | Live Preview Panel |
| Logo Upload |  |
| Store Name Input |  |
| Store URL Input |  |
| Finish Button |  |

---

### **FL-4: Theme Selection Logic**

```
themes = [Theme1, Theme2, Theme3, ...]
selectedTheme = themes[0]  // Default first theme

ON ThemeSelect(theme):
    selectedTheme = theme
    UPDATE previewPanel.theme = theme  // Real-time update

```

---

### **FL-5: Logo Upload Validation**

| Rule | Validation |
| --- | --- |
| File Type | JPEG, JPG, PNG only |
| Max Size | 5 MB |
| Recommended Dimensions | 500x500 px (not enforced, just recommended) |

```
ON LogoUpload(file):
    IF file.type NOT IN ['image/jpeg', 'image/jpg', 'image/png']:
        SHOW error "Only JPEG, JPG, PNG files are allowed"
        RETURN

    IF file.size > 5MB:
        SHOW error "File size must be less than 5MB"
        RETURN

    UPLOAD file
    UPDATE previewPanel.logo = file.url
    SHOW success

```

---

### **FL-6: Store Name Validation**

| Rule | Value |
| --- | --- |
| Required | Yes |
| Max Length | 50 characters |
| Allowed Characters | Letters, numbers, spaces, basic punctuation |
| Blocked | Special characters that break SEO |

```
ON StoreNameChange(value):
    IF value.trim() == "":
        SET nameError = "Store name is required"
    ELSE IF value.length > 50:
        SET nameError = "Maximum 50 characters allowed"
    ELSE IF containsInvalidChars(value):
        SET nameError = "Invalid characters"
    ELSE:
        SET nameError = null
        UPDATE previewPanel.storeName = value

    validateFinishButton()

```

---

### **FL-7: Store URL Validation**

| Rule | Value |
| --- | --- |
| Required | Yes |
| Max Length | 50 characters |
| Allowed Characters | Lowercase a-z, 0-9, hyphens (-) |
| Uniqueness | Must be unique (API check) |
| Reserved Words | admin, api, support, droplinked, www, mail, ftp, help, shop, store |

```
ON StoreURLChange(value):
    cleanValue = value.toLowerCase().replace(/[^a-z0-9-]/g, '')

    IF cleanValue == "":
        SET urlError = "URL is required"
        RETURN

    IF cleanValue.length > 50:
        SET urlError = "Maximum 50 characters allowed"
        RETURN

    IF cleanValue IN reservedWords:
        SET urlError = "This URL is reserved"
        RETURN

    // Debounce 300ms then check uniqueness
    DEBOUNCE 300ms:
        response = API.checkURLUniqueness(cleanValue)
        IF response.available == false:
            SET urlError = "This URL is already taken"
        ELSE:
            SET urlError = null
            UPDATE previewPanel.storeURL = cleanValue

    validateFinishButton()

```

**Reserved Words List:**

- admin
- api
- support
- droplinked
- www
- mail
- ftp
- help
- shop
- store
- checkout
- cart
- account
- login
- signup
- dashboard

---

### **FL-8: Finish Button State Logic**

```
FUNCTION validateFinishButton():
    isValid = (
        storeName.value.trim() != "" AND
        storeName.error == null AND
        storeURL.value.trim() != "" AND
        storeURL.error == null AND
        storeURL.isUnique == true
    )

    finishButton.disabled = !isValid

```

---

### **FL-9: Onboarding Completion Logic**

```
ON FinishClick:
    SET loading = true

    payload = {
        business_category: selectedCategory,
        theme_id: selectedTheme.id,
        logo_url: uploadedLogo.url,
        store_name: storeName.value,
        store_url: storeURL.value
    }

    response = API.completeOnboarding(payload)

    IF response.success:
        SET user.onboarding_completed = true
        SET user.tutorial_seen = false
        REDIRECT to /dashboard
    ELSE:
        SHOW error response.message
        SET loading = false

```

---

### **FL-10: Welcome Modal Logic**

```
ON DashboardLoad:
    IF user.onboarding_completed == true AND user.tutorial_seen == false:
        SHOW WelcomeModal

ON WelcomeModalClose:
    SET user.tutorial_seen = true
    API.updateTutorialSeen(true)

```

---

### **FL-11: Route Guard Logic**

```
ON NavigateTo('/onboarding'):
    IF user.onboarding_completed == true:
        REDIRECT to /dashboard
        RETURN false
    ELSE:
        ALLOW navigation
        RETURN true

```

---

### B3) Behavioral Flow

```
[User Registers/Logs In]
    ↓
[Check onboarding_completed flag]
    ├─ TRUE → [Redirect to Dashboard]
    │           ↓
    │         [Check tutorial_seen]
    │           ├─ FALSE → [Show Welcome Modal]
    │           └─ TRUE → [Normal Dashboard]
    │
    └─ FALSE → [Redirect to Onboarding]
                ↓
[Step 1: What Best Describes You?]
    ↓
[User Selects Category] → [Next Enabled]
    ↓
[Step 2: Store Identity]
    ↓
[User Fills Form + Preview Updates]
    ↓
[All Fields Valid] → [Finish Enabled]
    ↓
[User Clicks Finish]
    ↓
[Save Data + Mark Complete]
    ↓
[Redirect to Dashboard + Show Welcome Modal]

```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Not Started | User registers | Step 1 | Load Step 1 UI |
| Step 1 | Category selected | Step 1 (Ready) | Enable Next button |
| Step 1 (Ready) | Click Next | Step 2 | Save category, load Step 2 |
| Step 2 | All fields valid | Step 2 (Ready) | Enable Finish button |
| Step 2 (Ready) | Click Finish | Completed | Save all, redirect to dashboard |
| Completed | Dashboard load (first time) | Modal Shown | Show welcome modal |
| Modal Shown | Close modal | Tutorial Seen | Hide modal permanently |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** User closes browser mid-onboarding | On next login, redirect back to onboarding |
| **EC-2:** User tries to skip Step 1 directly to Step 2 | Redirect to Step 1 |
| **EC-3:** Logo upload fails | Show error, allow retry, don't block progress (logo optional) |
| **EC-4:** URL uniqueness check fails (network error) | Show "Unable to verify", disable Finish |
| **EC-5:** User enters URL with uppercase letters | Auto-convert to lowercase |
| **EC-6:** User enters URL with spaces/special chars | Auto-strip invalid characters |
| **EC-7:** API call to complete onboarding fails | Show error, allow retry |
| **EC-8:** User refreshes on Step 2 | Preserve entered data if possible |
| **EC-9:** Welcome modal video fails to load | Show fallback message, allow close |
| **EC-10:** User bookmarks /onboarding URL | Route guard redirects to dashboard |

---

### B6) Data Requirements

**User/Shop Profile Fields:**

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| onboarding_completed | Boolean | false | Has user completed onboarding |
| tutorial_seen | Boolean | false | Has user seen welcome modal |
| business_category | Enum | null | Selected category from Step 1 |
| store_name | String | null | Shop display name |
| store_url | String | null | Unique shop subdomain |
| logo_url | String | null | Uploaded logo URL |
| theme_id | String | null | Selected theme identifier |

**API Endpoints:**

| Endpoint | Method | Purpose |
| --- | --- | --- |
| `/api/onboarding/check-url` | POST | Check URL uniqueness |
| `/api/onboarding/complete` | POST | Save all onboarding data |
| `/api/user/tutorial-seen` | PATCH | Mark tutorial as seen |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | New user login | Redirected to onboarding Step 1 |
| TC-2 | Step 1 without selection | Next button disabled |
| TC-3 | Step 1 with selection | Next button enabled |
| TC-4 | Step 2 theme change | Preview updates immediately |
| TC-5 | Upload valid logo (PNG, 2MB) | Upload succeeds, preview updates |
| TC-6 | Upload invalid logo (PDF) | Error shown |
| TC-7 | Upload oversized logo (10MB) | Error shown |
| TC-8 | Enter store name | Preview updates |
| TC-9 | Enter reserved URL (admin) | Error: "This URL is reserved" |
| TC-10 | Enter taken URL | Error: "This URL is already taken" |
| TC-11 | Enter valid unique URL | No error, Finish enabled |
| TC-12 | Click Finish | Data saved, redirected to dashboard |
| TC-13 | First dashboard visit | Welcome modal appears |
| TC-14 | Refresh dashboard | Welcome modal does NOT appear |
| TC-15 | Try to access /onboarding after completion | Redirected to dashboard |
| TC-16 | Enter URL with uppercase | Auto-converted to lowercase |
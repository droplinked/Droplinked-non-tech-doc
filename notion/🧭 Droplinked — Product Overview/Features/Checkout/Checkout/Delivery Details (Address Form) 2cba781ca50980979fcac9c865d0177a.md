# Delivery Details (Address Form)

# Delivery Details (Address Form)

### Feature ID:

**[CHK-DELIVERY-403]**

### Title:

**Delivery Details - Address Form**

### Category:

**Checkout Domain** | **Actors**: Customer | **Channel**: Storefront

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
For orders containing physical or print-on-demand (POD) products, customers must provide a delivery address. The system should support both logged-in users with saved addresses and guest users. Address management should be intuitive with add/edit/delete capabilities.

**Desired Outcome:**

- **Email First**: Email is ALWAYS required and checked first. If email exists in system, load user data.
- **Address Book**: Logged-in users see their saved addresses list with ability to select, add, edit, or delete.
- **First Address Flow**:
    - If no saved addresses: Show inline address form
    - If addresses exist: Show address list with "Add New Address" button (opens modal)
- **Guest Flow**: Enter email → if exists, load user data and addresses; if not, show inline form.
- Display the Delivery Details form only when the cart contains shippable products (Physical or POD).
- **Digital Products**: For digital-only carts, show only First Name, Last Name, Phone Number (no address).
- Collect all necessary address fields without enforcing external API validation.
- Allow customers to add an optional note with their order.
- Automatically trigger shipping rate lookup when address is selected or form is completed.
- **User Data Persistence**: First Name, Last Name, Phone Number attach to user profile.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Email Field (Always Required):**
    - Email is the first required field
    - Pre-filled for logged-in users
    - Guest enters email → system checks if email exists in database
    - If email exists: Load user data, First Name, Last Name, Phone, and saved addresses
- **Digital Products Only:**
    - Show only: First Name, Last Name, Phone Number
    - No address form
    - Pre-fill from user profile if logged in
- **Form Visibility Condition:** Display only if cart contains Physical or POD items.
- **Address Book (Logged-in or Guest with existing email):**
    - Display list of saved addresses
    - Select an existing address
    - Add new address (opens modal)
    - Edit existing address (opens modal)
    - Delete address
- **First Address Flow:**
    - No saved addresses: Show inline address form
    - Has saved addresses: Show address list + "Add New Address" button
- **Contact Information Fields:**
    - Email Address (required, always)
    - First Name (required, pre-filled if user exists)
    - Last Name (required, pre-filled if user exists)
    - Phone Number (required, pre-filled if user exists)
- **Delivery Details Fields:**
    - Address Line 1 (required)
    - Address Line 2 (optional)
    - City (required)
    - State/Province (required)
    - Zip/Postal Code (required)
    - Country (required, dropdown)
- **Additional Note:**
    - Checkbox: "Add Additional Note"
    - Text area (appears when checkbox is checked)
- **Auto-trigger:** When address is selected or all required fields are filled, automatically initiate shipping rate lookup.
- **No Validation Blocking:** Accept user input as-is without Google Maps or third-party address validation.
- **User Data Attachment:** Save First Name, Last Name, Phone Number to user profile.

**Out of Scope:**

- Address autocomplete via Google Places API.
- Address validation that blocks submission.
- Multiple shipping addresses for split shipments.
- Billing address (assumed same as shipping for now).

---

### 3) **Key User Journeys**

---

**Journey 1: Guest Customer - Email Not in System**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Guest | Arrives at checkout | Email field empty |
| 2 | Guest | Enters email address | System checks if email exists |
| 3 | System | Email not found | No user data loaded, show inline address form |
| 4 | Guest | Fills First Name, Last Name, Phone | Fields remain empty (no pre-fill) |
| 5 | Guest | Fills all address fields | Form validates locally |
| 6 | System | Detects form complete | Automatically triggers shipping lookup |
| 7 | Guest | Sees shipping options load | Can proceed to select shipping |

---

**Journey 2: Guest Customer - Email Exists in System**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Guest | Arrives at checkout | Email field empty |
| 2 | Guest | Enters email address | System checks if email exists |
| 3 | System | Email found | Load user data: First Name, Last Name, Phone, saved addresses |
| 4 | Guest | Sees pre-filled name/phone | Can edit if needed |
| 5 | Guest | Sees saved addresses list | Can select existing or add new |
| 6 | Guest | Selects saved address | Triggers shipping lookup |
| 7 | Guest | OR clicks "Add New Address" | Modal opens with empty form |

---

**Journey 3: Logged-in Customer with Saved Addresses**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Arrives at checkout | Email pre-filled from account |
| 2 | System | Load user profile | First Name, Last Name, Phone pre-filled |
| 3 | System | Load saved addresses | Display address list |
| 4 | Customer | Views address book | Select, Add, Edit, or Delete addresses |
| 5 | Customer | Selects existing address | Triggers shipping lookup |
| 6 | Customer | OR clicks "Add New Address" | Modal opens with address form |
| 7 | Customer | OR clicks "Edit" on address | Modal opens with pre-filled form |
| 8 | Customer | OR clicks "Delete" on address | Confirmation dialog, then remove from list |

---

**Journey 4: Logged-in Customer - No Saved Addresses**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Arrives at checkout | Email pre-filled |
| 2 | System | Check saved addresses | None found |
| 3 | System | Display inline form | First Name, Last Name, Phone pre-filled |
| 4 | Customer | Fills address fields | Form validates locally |
| 5 | System | Detects form complete | Triggers shipping lookup |
| 6 | Customer | Can save address to profile | Address added to account for future use |

---

**Journey 5: Add/Edit Address via Modal**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Clicks "Add New Address" or "Edit" | Modal window opens |
| 2 | Customer | Sees address form | Empty for Add, pre-filled for Edit |
| 3 | Customer | Fills/Modifies fields | All required fields: First Name, Last Name, Phone, Address, City, State, Zip, Country |
| 4 | Customer | Clicks "Save Address" | Address saved to user account |
| 5 | System | Close modal | Address list refreshes |
| 6 | Customer | Sees updated list | New/edited address appears in list |

---

**Journey 6: Digital-Only Cart**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Has only digital products in cart | Delivery Details section shows contact fields only |
| 2 | Customer | Sees Email field | Pre-filled if logged in |
| 3 | Customer | Sees First Name, Last Name, Phone | Pre-filled if user data exists |
| 4 | Customer | Can edit contact info | No address required |
| 5 | Customer | Proceeds directly to payment | No shipping calculation needed |
| 6 | System | On order completion | Save First Name, Last Name, Phone to user profile |

---

**Journey 7: Add Additional Note**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Address selected/form complete | Form complete |
| 2 | Customer | Checks "Add Additional Note" | Text area expands below |
| 3 | Customer | Types note (e.g., "Leave at door") | Note stored with order |
| 4 | Customer | Unchecks checkbox | Text area collapses, note cleared |

---

**Journey 8: Edit Address After Shipping Selected**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Has already selected shipping | Shipping shows selected option |
| 2 | Customer | Edits address via modal | Address updated in list |
| 3 | System | Detects address modification | Resets and re-fetches shipping options |
| 4 | Customer | Selects new shipping option | New rates applied |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | **Email is ALWAYS required** as the first field in checkout |  |
| BAC-2 | For **guest users**: After entering email, system checks if email exists and loads user data + addresses if found |  |
| BAC-3 | For **logged-in users**: Email, First Name, Last Name, Phone are pre-filled from profile |  |
| BAC-4 | **Address Book**: Logged-in users (or guests with existing email) see saved addresses list with Select/Add/Edit/Delete options |  |
| BAC-5 | **First Address**: If no saved addresses, show inline form. If addresses exist, show list + "Add New Address" button |  |
| BAC-6 | **Add/Edit Address**: Opens in modal window with full address form |  |
| BAC-7 | **Delete Address**: Confirmation dialog shown before deletion |  |
| BAC-8 | **Digital Products Only**: Show only First Name, Last Name, Phone fields (no address form) |  |
| BAC-9 | Delivery Details section is shown ONLY when cart contains Physical or POD products |  |
| BAC-10 | All required fields (Name, Phone, Address, City, State, Zip, Country) must be present for physical products |  |
| BAC-11 | Address Line 2 is optional |  |
| BAC-12 | "Add Additional Note" checkbox reveals a text area when checked |  |
| BAC-13 | No third-party address validation blocks form submission |  |
| BAC-14 | Shipping lookup is triggered automatically when address is selected or all required fields are completed |  |
| BAC-15 | Changing address after shipping is selected resets and re-fetches shipping options |  |
| BAC-16 | Country field is a dropdown with supported countries |  |
| BAC-17 | **User Data**: First Name, Last Name, Phone Number attach to user profile on order completion |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature Definition |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Shippable Product** | A product that requires physical delivery (Physical type or POD type) |
| **Required Field** | A form field that must be filled before proceeding |
| **Form Complete** | All required fields have non-empty values |
| **Additional Note** | An optional message from customer to merchant about the order |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Section Visibility Logic**

```
// Email is ALWAYS shown first
SHOW EmailField

IF cart.hasPhysicalItems OR cart.hasPODItems:
    SHOW DeliveryDetailsSection (Full Address)
ELSE:
    SHOW ContactDetailsOnly (FirstName, LastName, Phone)
```

| Cart Contains | Show Full Address Form? | Show Contact Fields? |
| --- | --- | --- |
| Physical products only | ✅ Yes | ✅ Yes |
| POD products only | ✅ Yes | ✅ Yes |
| Digital products only | ❌ No | ✅ Yes (Names + Phone only) |
| Mixed (Physical + Digital) | ✅ Yes | ✅ Yes |
| Mixed (POD + Digital) | ✅ Yes | ✅ Yes |

---

### **FL-2: Email Field & User Lookup**

| Field | Type | Required | Pre-fill Logic | Editable |
| --- | --- | --- | --- | --- |
| Email Address | Email input | Yes | If logged in, use account email | Yes, always |

**Email Lookup Flow:**

```
ON EmailField.Blur OR Debounce(2 seconds):
    IF email.isValidFormat THEN
        userData = API.checkEmailExists(email)
        IF userData.found THEN
            Pre-fill FirstName, LastName, Phone from userData
            Load saved addresses from userData
            Show address list
        ELSE
            Clear pre-filled fields (keep email)
            Show inline address form (if no addresses)
        ENDIF
    ENDIF
```

| Scenario | Email Check Result | Action |
| --- | --- | --- |
| Logged-in user | N/A (already known) | Pre-fill all fields, load addresses |
| Guest - new email | Not found | Show empty inline form |
| Guest - existing email | Found | Load user data and addresses |

---

### **FL-3: Contact Fields (Always Shown)**

| Field | Input Type | Required | Pre-fill Logic | Validation |
| --- | --- | --- | --- | --- |
| First Name | Text | Yes | From user profile if exists | Non-empty |
| Last Name | Text | Yes | From user profile if exists | Non-empty |
| Phone Number | Tel | Yes | From user profile if exists | Non-empty (no format validation) |

**User Data Attachment:**

```
ON OrderComplete:
    Save FirstName, LastName, Phone to UserProfile
    Attach UserID to Order
```

---

### **FL-4: Address Book Logic**

**Address List Display:**

```
IF user.hasSavedAddresses:
    Render AddressList(addresses)
    Render "Add New Address" Button
    Render "Or fill address below" Text
ELSE:
    Render InlineAddressForm()
```

**Address List Actions:**

| Action | Trigger | Behavior |
| --- | --- | --- |
| Select Address | Click on address card | Mark as selected, trigger shipping lookup |
| Add New Address | Click "Add New Address" button | Open modal with empty form |
| Edit Address | Click "Edit" icon on address | Open modal with pre-filled form |
| Delete Address | Click "Delete" icon | Show confirmation dialog, then remove |

**Address Card Display:**

- Full name
- Phone number
- Full address (Line 1, Line 2, City, State, Zip, Country)
- "Edit" and "Delete" buttons
- Radio button or highlight for selection

---

### **FL-5: Address Form Fields (Physical/POD Products)**

| Field | Input Type | Required | Validation |
| --- | --- | --- | --- |
| Address Line 1 | Text | Yes | Non-empty |
| Address Line 2 | Text | No | None |
| City | Text | Yes | Non-empty |
| State/Province | Text or Dropdown | Yes | Non-empty |
| Zip/Postal Code | Text | Yes | Non-empty (no format validation) |
| Country | Dropdown | Yes | Must select from list |

**Modal Form (Add/Edit):**

```
ModalTitle: "Add New Address" OR "Edit Address"
Fields:
  - First Name (pre-filled)
  - Last Name (pre-filled)
  - Phone (pre-filled)
  - Address Line 1
  - Address Line 2
  - City
  - State
  - Zip
  - Country
Buttons:
  - "Save Address" (primary)
  - "Cancel" (secondary)
```

---

### **FL-6: Additional Note Logic**

```
ON CheckboxChange("Add Additional Note"):
    IF checkbox.checked:
        SHOW TextArea(additionalNote)
        SET additionalNote.maxLength = 500
    ELSE:
        HIDE TextArea
        CLEAR additionalNote.value
```

| Checkbox State | Text Area | Note Value |
| --- | --- | --- |
| Unchecked (default) | Hidden | null/empty |
| Checked | Visible | User input |
| Unchecked after being checked | Hidden | Cleared |

---

### **FL-7: Form Completion Detection**

**For Digital Products:**

```
FUNCTION isDigitalFormComplete():
    requiredFields = [email, firstName, lastName, phone]

    FOR each field in requiredFields:
        IF field.value.trim() == "":
            RETURN false
    RETURN true
```

**For Physical Products:**

```
FUNCTION isAddressComplete():
    IF selectedAddress.exists:
        RETURN true

    requiredFields = [firstName, lastName, phone, address1, city, state, zip, country]
    FOR each field in requiredFields:
        IF field.value.trim() == "":
            RETURN false
    RETURN true

ON FieldChange(anyField) OR AddressSelected:
    IF isAddressComplete():
        TRIGGER fetchShippingRates(getAddressObject())
```

---

### **FL-8: Shipping Trigger Logic**

| Event | Action |
| --- | --- |
| Existing address selected | Immediately call shipping API |
| Last required field filled (inline form) | Immediately call shipping API |
| Any address field changed after initial completion | Debounce 500ms, then re-fetch shipping |
| Country changed | Clear state field if state was country-specific |

---

### **FL-9: Address Change After Shipping Selected**

```
ON AddressModified():
    IF shippingRates.loaded AND shippingSelection.exists:
        RESET shippingSelection
        SET shippingStatus = "Loading"
        FETCH newShippingRates(newAddress)
        UPDATE sidebar.shipping = "Recalculating..."
```

---

### B3) Behavioral Flow

```
[Checkout Page Loads]
    ↓
[Check Cart Contents]
    ├─ Contains Physical/POD → [Show Full Address Section]
    └─ Digital Only → [Show Contact Fields Only]
    ↓
[Render Email Field]
    ├─ Logged in → Pre-fill email
    └─ Guest → Empty email field
    ↓
[Guest Enters Email]
    ↓
[Check Email Existence]
    ├─ Email exists → Load user data + addresses
    └─ Email new → Continue as guest
    ↓
[For Physical/POD Products]
    ↓
[Check Saved Addresses]
    ├─ Has addresses → Show Address List + "Add New" button
    └─ No addresses → Show inline address form
    ↓
[User Interaction]
    ├─ Select existing address → Trigger shipping
    ├─ Click "Add New" → Open modal → Save → Refresh list
    ├─ Click "Edit" → Open modal → Save → Refresh list
    ├─ Click "Delete" → Confirm → Remove from list
    └─ Fill inline form → Trigger shipping when complete
    ↓
[Shipping Section Activates]
    ↓
[Optional: User Checks "Add Additional Note"]
    ├─ YES → Show text area → User types note
    └─ NO → Continue without note
    ↓
[User Modifies Address?]
    ├─ YES → Reset shipping, re-fetch
    └─ NO → Continue to payment
    ↓
[Order Complete]
    ↓
[Save User Data]
    ├─ Attach FirstName, LastName, Phone to UserProfile
    └─ Attach UserID to Order
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Email Empty | User enters email | Email Filled | Check email existence |
| Email Filled | Email found in DB | User Data Loaded | Pre-fill names, phone, load addresses |
| Email Filled | Email not found | Guest Mode | Show empty form |
| No Addresses | First address added | Has Addresses | Show address list |
| Address List | Select address | Address Selected | Trigger shipping fetch |
| Address List | Click "Add New" | Modal Open | Show empty address form |
| Address List | Click "Edit" | Modal Open | Show pre-filled address form |
| Modal Open | Save clicked | Address List | Save to profile, refresh list |
| Modal Open | Cancel clicked | Address List | Close modal, no changes |
| Form Empty | User starts typing | Form Partial | None |
| Form Partial | All required fields filled | Form Complete | Trigger shipping fetch |
| Form Complete | User clears a field | Form Partial | Cancel shipping fetch |
| Form Complete | Address modified | Form Modified | Re-fetch shipping |
| Note Hidden | Checkbox checked | Note Visible | Show text area |
| Note Visible | Checkbox unchecked | Note Hidden | Clear note value |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Guest enters email that exists in system | Load user data and addresses seamlessly |
| **EC-2:** User has 10+ saved addresses | Scrollable address list |
| **EC-3:** User deletes all addresses | Switch to inline form automatically |
| **EC-4:** User edits address during shipping calculation | Cancel current fetch, re-fetch with new address |
| **EC-5:** User pastes full address in Address Line 1 | Accept as-is (no parsing) |
| **EC-6:** Invalid phone format (e.g., letters) | Accept as-is (no format validation) |
| **EC-7:** Zip code doesn't match city | Accept as-is (shipping API will handle) |
| **EC-8:** Country not in supported list | Show error, prevent selection |
| **EC-9:** User switches between tabs during input | Preserve field values |
| **EC-10:** Network error during shipping fetch | Show retry option in shipping section |
| **EC-11:** Very long address (>200 chars) | Accept, may truncate in display |
| **EC-12:** Additional note too long | Enforce 500 character limit |
| **EC-13:** Special characters in name | Accept (international names) |
| **EC-14:** User refreshes page mid-form | Clear form (no persistence for guests), logged-in users reload data |
| **EC-15:** Modal save fails (network error) | Show error, keep modal open, allow retry |
| **EC-16:** User deletes address that was selected | Clear selection, reset shipping |

---

### B6) Data Requirements

**Address Object Structure:**

| Field | Type | Required | Max Length |
| --- | --- | --- | --- |
| email | String | Yes | 254 |
| first_name | String | Yes | 50 |
| last_name | String | Yes | 50 |
| phone | String | Yes | 20 |
| address_line_1 | String | Yes | 200 |
| address_line_2 | String | No | 200 |
| city | String | Yes | 100 |
| state | String | Yes | 100 |
| zip_code | String | Yes | 20 |
| country | String | Yes | 2 (ISO code) |
| additional_note | String | No | 500 |

**Country Dropdown Data:**

```
[
    { code: "US", name: "United States" },
    { code: "CA", name: "Canada" },
    { code: "GB", name: "United Kingdom" },
    // ... all supported countries
]
```

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| **Email & User Lookup** |  |  |
| TC-1 | Logged-in user arrives | Email, First Name, Last Name, Phone pre-filled |
| TC-2 | Guest enters new email | No pre-fill, show empty form |
| TC-3 | Guest enters existing email | Load user data and addresses automatically |
| **Address Book** |  |  |
| TC-4 | Logged-in user with saved addresses | Address list displayed |
| TC-5 | Logged-in user with no addresses | Inline form displayed |
| TC-6 | Click "Add New Address" | Modal opens with empty form |
| TC-7 | Click "Edit" on address | Modal opens with pre-filled form |
| TC-8 | Click "Delete" on address | Confirmation dialog, then removed from list |
| TC-9 | Save new address in modal | Address added to list, modal closes |
| TC-10 | Delete all addresses | Switch to inline form automatically |
| **Digital Products** |  |  |
| TC-11 | Cart with digital only | Show only First Name, Last Name, Phone, Email |
| TC-12 | Digital + logged-in user | Names and phone pre-filled |
| **Form Completion** |  |  |
| TC-13 | Cart with physical product | Full address form displayed |
| TC-14 | Fill all required fields | Shipping fetch triggered |
| TC-15 | Leave one field empty | Shipping not triggered |
| TC-16 | Select existing address | Shipping fetch triggered immediately |
| **Additional Features** |  |  |
| TC-17 | Check Additional Note | Text area appears |
| TC-18 | Uncheck Additional Note | Text area hides, note cleared |
| TC-19 | Enter note with 600 chars | Truncated to 500 |
| TC-20 | Change country after shipping loaded | Shipping re-fetches |
| TC-21 | Enter invalid format phone | Accepted without error |
| TC-22 | Delete selected address | Selection cleared, shipping reset |
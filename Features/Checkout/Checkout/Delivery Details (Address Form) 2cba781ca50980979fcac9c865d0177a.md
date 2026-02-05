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
For orders containing physical or print-on-demand (POD) products, customers must provide a delivery address. The address form should be simple, fast to complete, and not block customers with unnecessary validation. Once completed, it triggers the shipping rate lookup automatically.

**Desired Outcome:**

- Display the Delivery Details form only when the cart contains shippable products (Physical or POD).
- Collect all necessary address fields without enforcing external API validation.
- Allow customers to add an optional note with their order.
- Automatically trigger shipping rate lookup when all required fields are completed.
- Pre-fill data for logged-in customers with saved addresses (future enhancement).

---

### 2) **Scope – In / Out**

**In Scope:**

- **Form Visibility Condition:** Display only if cart contains Physical or POD items.
- **Contact Information Section:**
    - Email Address (pre-filled if logged in, editable)
- **Delivery Details Fields:**
    - First Name (required)
    - Last Name (required)
    - Phone Number (required)
    - Address Line 1 (required)
    - Address Line 2 (optional)
    - City (required)
    - State/Province (required)
    - Zip/Postal Code (required)
    - Country (required, dropdown)
- **Additional Note:**
    - Checkbox: "Add Additional Note"
    - Text area (appears when checkbox is checked)
- **Auto-trigger:** When all required fields are filled, automatically initiate shipping rate lookup.
- **No Validation Blocking:** Accept user input as-is without Google Maps or third-party address validation.

**Out of Scope:**

- Address autocomplete via Google Places API.
- Address validation that blocks submission.
- Multiple shipping addresses for split shipments.
- Address book management (save/edit saved addresses).
- Billing address (assumed same as shipping for now).

---

### 3) **Key User Journeys**

---

**Journey 1: Guest Customer Fills Address**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Guest | Arrives at checkout | Email field empty, address form displayed |
| 2 | Guest | Enters email address | Email stored for order |
| 3 | Guest | Fills all address fields | Form validates locally (format only) |
| 4 | System | Detects form complete | Automatically triggers shipping lookup |
| 5 | Guest | Sees shipping options load | Can proceed to select shipping |

---

**Journey 2: Logged-in Customer**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Arrives at checkout | Email pre-filled from account |
| 2 | Customer | Can change email if needed | New email accepted |
| 3 | Customer | Fills address fields | Form validates locally |
| 4 | System | Detects form complete | Triggers shipping lookup |

---

**Journey 3: Add Additional Note**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Fills address form | Form complete |
| 2 | Customer | Checks "Add Additional Note" | Text area expands below |
| 3 | Customer | Types note (e.g., "Leave at door") | Note stored with order |
| 4 | Customer | Unchecks checkbox | Text area collapses, note cleared |

---

**Journey 4: Digital-Only Cart**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Has only digital products in cart | Delivery Details section is HIDDEN |
| 2 | Customer | Only sees Email field | No address required |
| 3 | Customer | Proceeds directly to payment | No shipping calculation needed |

---

**Journey 5: Edit Address After Shipping Selected**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Has already selected shipping | Shipping shows selected option |
| 2 | Customer | Changes country or zip code | Address marked as changed |
| 3 | System | Detects address modification | Resets and re-fetches shipping options |
| 4 | Customer | Selects new shipping option | New rates applied |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | Delivery Details section is shown ONLY when cart contains Physical or POD products |  |
| BAC-2 | Email field is pre-filled for logged-in users but remains editable |  |
| BAC-3 | All required fields (Name, Phone, Address, City, State, Zip, Country) must be present |  |
| BAC-4 | Address Line 2 is optional |  |
| BAC-5 | "Add Additional Note" checkbox reveals a text area when checked |  |
| BAC-6 | No third-party address validation blocks form submission |  |
| BAC-7 | Shipping lookup is triggered automatically when all required fields are completed |  |
| BAC-8 | Changing address after shipping is selected resets and re-fetches shipping options |  |
| BAC-9 | Digital-only carts do NOT show the Delivery Details section |  |
| BAC-10 | Country field is a dropdown with supported countries |  |

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
IF cart.hasPhysicalItems OR cart.hasPODItems:
    SHOW DeliveryDetailsSection
ELSE:
    HIDE DeliveryDetailsSection
    SHOW ContactEmailOnly

```

| Cart Contains | Show Delivery Details? |
| --- | --- |
| Physical products only | ✅ Yes |
| POD products only | ✅ Yes |
| Digital products only | ❌ No |
| Mixed (Physical + Digital) | ✅ Yes |
| Mixed (POD + Digital) | ✅ Yes |

---

### **FL-2: Contact Information Field**

| Field | Type | Required | Pre-fill Logic | Editable |
| --- | --- | --- | --- | --- |
| Email Address | Email input | Yes | If logged in, use account email | Yes, always |

---

### **FL-3: Address Form Fields**

| Field | Input Type | Required | Validation |
| --- | --- | --- | --- |
| First Name | Text | Yes | Non-empty |
| Last Name | Text | Yes | Non-empty |
| Phone Number | Tel | Yes | Non-empty (no format validation) |
| Address Line 1 | Text | Yes | Non-empty |
| Address Line 2 | Text | No | None |
| City | Text | Yes | Non-empty |
| State/Province | Text or Dropdown | Yes | Non-empty |
| Zip/Postal Code | Text | Yes | Non-empty (no format validation) |
| Country | Dropdown | Yes | Must select from list |

---

### **FL-4: Additional Note Logic**

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

### **FL-5: Form Completion Detection**

```
FUNCTION isFormComplete():
    requiredFields = [firstName, lastName, phone, address1, city, state, zip, country]

    FOR each field in requiredFields:
        IF field.value.trim() == "":
            RETURN false

    RETURN true

ON FieldChange(anyField):
    IF isFormComplete():
        TRIGGER fetchShippingRates(getAddressObject())

```

---

### **FL-6: Shipping Trigger Logic**

| Event | Action |
| --- | --- |
| Last required field filled | Immediately call shipping API |
| Any address field changed after initial completion | Debounce 500ms, then re-fetch shipping |
| Country changed | Clear state field if state was country-specific |

---

### **FL-7: Address Change After Shipping Selected**

```
ON AddressFieldChange():
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
    ├─ Contains Physical/POD → [Show Full Form]
    └─ Digital Only → [Show Email Only]
    ↓
[Render Contact Information]
    ├─ Logged in → Pre-fill email
    └─ Guest → Empty email field
    ↓
[Render Address Fields]
    ↓
[User Fills Fields]
    ↓
[On Each Field Change]
    ├─ Check if all required fields complete
    │   ├─ NO → Wait for more input
    │   └─ YES → Trigger Shipping Lookup
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

```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Form Empty | User starts typing | Form Partial | None |
| Form Partial | All required fields filled | Form Complete | Trigger shipping fetch |
| Form Complete | User clears a field | Form Partial | Cancel shipping fetch |
| Form Complete | Address field changed | Form Modified | Re-fetch shipping |
| Note Hidden | Checkbox checked | Note Visible | Show text area |
| Note Visible | Checkbox unchecked | Note Hidden | Clear note value |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** User pastes full address in Address Line 1 | Accept as-is (no parsing) |
| **EC-2:** Invalid phone format (e.g., letters) | Accept as-is (no format validation) |
| **EC-3:** Zip code doesn't match city | Accept as-is (shipping API will handle) |
| **EC-4:** Country not in supported list | Show error, prevent selection |
| **EC-5:** User switches between tabs during input | Preserve field values |
| **EC-6:** Network error during shipping fetch | Show retry option in shipping section |
| **EC-7:** Very long address (>200 chars) | Accept, may truncate in display |
| **EC-8:** Additional note too long | Enforce 500 character limit |
| **EC-9:** Special characters in name | Accept (international names) |
| **EC-10:** User refreshes page mid-form | Clear form (no persistence for guests) |

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
| TC-1 | Cart with physical product | Full address form displayed |
| TC-2 | Cart with digital only | Only email field shown |
| TC-3 | Logged-in user | Email pre-filled |
| TC-4 | Guest user | Email empty |
| TC-5 | Fill all required fields | Shipping fetch triggered |
| TC-6 | Leave one field empty | Shipping not triggered |
| TC-7 | Check Additional Note | Text area appears |
| TC-8 | Uncheck Additional Note | Text area hides, note cleared |
| TC-9 | Enter note with 600 chars | Truncated to 500 |
| TC-10 | Change country after shipping loaded | Shipping re-fetches |
| TC-11 | Enter invalid format phone | Accepted without error |
| TC-12 | Enter invalid format zip | Accepted without error |
| TC-13 | Select country | State field remains editable |
| TC-14 | Refresh page | Form clears (guest) |
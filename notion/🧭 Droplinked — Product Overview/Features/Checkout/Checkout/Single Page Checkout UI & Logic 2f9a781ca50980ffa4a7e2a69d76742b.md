# Single Page Checkout UI & Logic

# Single Page Checkout UI & Logic

## 

### Feature ID:

**[CHK-layout-401]**

### Title:

**Single Page Checkout Experience**

### Category:

**Checkout Domain** | **Actors**: Customer | **Channel**: Storefront

### Status:

**Defined**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**
The previous multi-step checkout process caused friction and drop-offs. A single-page application (SPA) approach simplifies the experience, allowing users to see their cart, input details, and select payment options in one unified view. This reduces clicks and provides immediate feedback on costs and shipping availability.

**Desired Outcome:**

- Consolidate Contact Information, Delivery Details, Shipping Options, and Payment into a single scrollable page.
- Display a persistent "Order Summary" sidebar on the right with real-time updates.
- **Address Book**: Logged-in users see saved addresses list with Add/Edit/Delete capabilities.
- **Guest Flow**: Guest enters email → if email exists in system, load user data and addresses automatically.
- **Digital Products**: For carts with only digital products, show only First Name, Last Name, Phone Number (no address form).
- Simplify address entry by removing strict third-party validation blocking (allow users to proceed).
- Provide immediate feedback on Shipping availability based on the entered address.
- Allow users to add optional notes to their order.
- Handle "Zero Cost" orders (due to discounts) by bypassing payment selection.
- **Payment**: Always visible but disabled until all required information is complete.

---

### 2) **Scope – In / Out**

**In:**

- **Layout:** Two-column layout (Left: Forms/Steps, Right: Sticky Sidebar).
- **Contact Info:** Email handling (Guest vs. Logged in) - **Email is ALWAYS required**.
- **Address Book (Logged-in Users):**
    - Display list of saved addresses
    - Select an existing address
    - Add new address (opens modal)
    - Edit existing address (opens modal)
    - Delete address
- **Guest Flow:** Email entry → check if email exists → load user data and addresses if found.
- **Delivery Details:**
    - For physical/POD products: Full address form (inline if first address, modal if adding new)
    - For digital products: Only First Name, Last Name, Phone Number
- **User Data:** First Name, Last Name, Phone Number attach to user profile (saved for future orders).
- **Payment Section Logic:** Always visible but disabled/locked until all required fields complete.
- **Payment Methods:**
    - Credit Card: Stripe component loads after selecting "Credit Card" option
    - PayPal: Redirect to PayPal page on click
    - Crypto: Redirect to crypto payment page on click
- **Sidebar Logic:** Real-time calculation of Subtotal, Shipping, Tax, Discount, Total.

**Out:**

- **Strict Address Validation:** No blocking validation (e.g., Google Maps API enforcement) preventing submission.
- **Multi-page navigation:** No "Next Step" buttons; distinct sections on one page instead.

---

### 3) **Key User Journeys**

**Journey 1: Guest Checkout (Physical Item)**

1. **Load:** Customer lands on Checkout. Sidebar shows cart items.
2. **Contact:** Enters Email Address.
3. **Email Check:** System checks if email exists in database.
    - If exists → Load user data and addresses automatically.
    - If not exists → Show empty address form inline.
4. **Address:**
    - If no saved addresses: Fills Delivery Details form inline (First Name, Last Name, Phone, Address).
    - If saved addresses exist: Select from address list or click "Add New Address" (opens modal).
5. **Note:** Checks "Add Additional Note", types a message.
6. **Shipping:** Upon address completion, Shipping section loads options. Customer selects one.
7. **Review:** Sidebar updates with Shipping cost and Total.
8. **Payment:** Payment methods are visible but locked until completion. Once complete, customer selects Payment Method.
    - Credit Card: Stripe component loads after selection.
    - PayPal/Crypto: Redirect on click.
9. **Submit:** Clicks Pay/Confirm.
10. **User Data:** First Name, Last Name, Phone Number saved to user profile.

**Journey 2: Logged-in User with Saved Addresses**

1. **Load:** Customer lands on Checkout. Sidebar shows cart items.
2. **Contact:** Email pre-filled from account.
3. **Address Book:** Display list of saved addresses.
4. **Selection:** Customer selects an existing address OR clicks "Add New Address" (opens modal).
5. **Edit/Delete:** Customer can edit or delete existing addresses (opens modal for edit).
6. **Note:** Optional additional note.
7. **Shipping:** Shipping options load based on selected address.
8. **Payment:** Select payment method (Credit Card, PayPal, or Crypto).
9. **Submit:** Complete order.

---

**Journey 3: Digital Products Only**

1. **Load:** Cart contains only digital products.
2. **Contact:** Email field (pre-filled if logged in).
3. **Details:** Show only First Name, Last Name, Phone Number fields.
    - Pre-filled for logged-in users from profile.
4. **No Address Form:** Delivery details section hidden.
5. **No Shipping:** Shipping section hidden.
6. **Payment:** Select payment method.
7. **Submit:** Complete order.
8. **User Data:** Names and phone saved to user profile.

---

**Journey 4: Discount to Zero**

1. **Coupon:** Customer applies a coupon in Sidebar.
2. **Calc:** Total becomes $0.00.
3. **Payment Hidden:** Payment methods section disappears.
4. **Confirm:** "Confirm Order" button appears directly.
5. **Submit:** Customer clicks Confirm.

**Journey 5: No Shipping Available**

1. **Address:** Customer enters an address where items cannot be shipped.
2. **Error:** Shipping section displays "These items can't be shipped to your address."
3. **Action:** Customer clicks "Remove Items" to clear conflicting products.

---

**Journey 6: Add/Edit Address via Modal**

1. **Trigger:** User clicks "Add New Address" or "Edit" on existing address.
2. **Modal Open:** Address form opens in modal window.
3. **Form Fields:** First Name, Last Name, Phone, Address Line 1, Address Line 2, City, State, Zip, Country.
4. **Save:** Click Save → Address saved to user account.
5. **List Update:** Address list refreshes automatically.
6. **Modal Close:** Return to checkout with updated address list.

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** The Checkout MUST be a single page with a right-side summary sidebar.
- **BAC 2:** Changing the address MUST automatically trigger a refresh of the Shipping section.
- **BAC 3:** If the Total Payment is $0, the Payment Methods section MUST be hidden.
- **BAC 4:** Address Form MUST NOT enforce external validation APIs (accept user input as-is).
- **BAC 5:** The sidebar MUST display: List of items, Subtotal, Shipping, Tax, Discount, and Total.
- **BAC 6:** **Email field is ALWAYS required** and is the first field in checkout.
- **BAC 7:** For **logged-in users**: Display address book with saved addresses, Add/Edit/Delete capabilities.
- **BAC 8:** For **guest users**: After entering email, if email exists in system, load user data and addresses automatically.
- **BAC 9:** First address entry: Show inline form if no addresses exist; Show address list with "Add New" button if addresses exist (opens modal).
- **BAC 10:** Address Add/Edit operations MUST open in a modal window.
- **BAC 11:** For **digital-only carts**: Hide address form, show only First Name, Last Name, Phone Number.
- **BAC 12:** First Name, Last Name, Phone Number MUST be saved to user profile for future orders.
- **BAC 13:** Payment methods MUST be visible but disabled/locked until all required information is complete.
- **BAC 14:** Credit Card option: Stripe component loads only AFTER user clicks "Credit Card" option.
- **BAC 15:** PayPal and Crypto options: Redirect to respective payment pages on click.

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-01-31 | Copy/Paste AI | Initial Draft | Creation |

---

---

# [PART 2: AI REFERENCE SPEC]

### B1) Definitions & Glossary

- **Single Page Checkout:** A layout where all steps (Info, Shipping, Payment) are stacked vertically, not on separate URLs.
- **Real-time Sidebar:** A summary component that updates instantly when inputs change (e.g., applying a coupon or selecting shipping).
- **Zero Cost Order:** An order where the final payable amount is 0 (due to 100% discount or free items).

### B2) Detailed Functional Rules (Numbered)

- **R1. Sidebar Content:** The Right Sidebar must always display:
    - List of Products (Thumbnail, Title, Quantity, Unit Price).
    - Coupon Code Input.
    - Financials: Subtotal (Sum of items), Shipping Cost (Sum of selected rates), Tax, Discount (Amount subtracted), Total (Final payable).
- **R2. Contact Info:**
    - Field: `Email Address`.
    - If user is logged in: Pre-filled.
    - If user is guest: Editable text field.
- **R3. Delivery Details Trigger:** This section appears if the cart contains Physical or POD items.
    - Fields: First Name, Last Name, Phone Number, Address Line 1, Address Line 2 (Optional), City, State, Zip/Postal Code, Country.
    - Validation: None (Client-side format checks only, no API blocking).
- **R4. Additional Note:**
    - Checkbox: "Add Additional Note".
    - Behavior: If checked, reveal a Text Area.
    - Output: Passed to order payload.
- **R5. Shipping Loading State:**
    - Trigger: When all required fields in "Delivery Details" are filled.
    - Behavior: The "Shipping" section (below Address) enters a `Loading` state (spinner/skeleton) while fetching rates.
- **R6. Shipping Error:**
    - Condition: No shipping rates returned for the specific address/products.
    - Display: Error message "These items can't be shipped to your address."
    - Action: Button "Remove Items" (Removes un-shippable items from cart).
- **R7. Discount Logic:**
    - Applied on: Subtotal (Item sum).
    - Exception: Does NOT discount Shipping or Tax unless specified.
    - Display: "Payment" section logic depends on the result.
- **R8. Payment Section Visibility:**
    - Payment methods are ALWAYS visible on checkout page
    - BUT disabled/locked until all required information is complete
    - Visual indicator: Grayed out or overlay with "Complete delivery details to proceed" message
- **R9. Payment Method Selection:**
    - Credit Card: Component loads AFTER user clicks "Credit Card" radio button
    - PayPal: Redirect to PayPal on button click
    - Crypto: Redirect to crypto payment page on button click
- **R10. Zero Payment Logic:**
    - Condition: If `Total == 0`.
    - Behavior: Hide "Payment Methods" selector. show "Confirm Order" button.
    - Action: Clicking Confirm creates the order immediately.

### B3) UI/UX States & Triggers

- **Sidebar Updates:** Any change in Quantity (if editable), Address (changes tax/shipping), or Coupon MUST trigger a recalculation of the Sidebar values.
- **Coupon Success:** Green text message: `{Value} discount code applied`.
- **Coupon Fail:** Red text message.
- **Address Complete:** Auto-advance focus or scroll isn't strictly necessary, but Shipping loading begins immediately.
- **Payment Section States:**
    - **Locked:** All payment options grayed out with message "Complete delivery details to proceed"
    - **Unlocked:** Payment options active and selectable
    - **Credit Card Selected:** Stripe component loads inline below the option
    - **Zero Total:** Payment section hidden, "Confirm Order" button shown instead

### B4) Edge Cases & Error Handling

- **EC1. Mixed Cart (Physical + Digital):** Address form is shown. Digital items are listed in sidebar but don't affect shipping calculation logic (unless they have weight/rules defined, usually 0).
- **EC2. Address Change:** If user modifies address after shipping is selected, Shipping section re-loads and resets selection to default. Sidebar updates.
- **EC3. Payment Failure:** If Payment fails (Credit/Crypto/PayPal), user remains on/returns to this page to retry.

### B5) Data Requirements & API Contracts

- **Input Payload:**
    - `contact`: { email }
    - `shipping_address`: { first_name, last_name, phone, address_1, address_2, city, state, zip, country }
    - `note`: string
    - `discount_code`: string
    - `items`: array
    - `selected_shipping_option`: id
- **Output (Sidebar):**
    - `subtotal`: float
    - `shipping_total`: float
    - `tax_total`: float
    - `discount_total`: float
    - `grand_total`: float

### B6) Logical Flow & Pseudo-code

```
IF Cart.isEmpty THEN
   Show EmptyState
ELSE
   Render Sidebar(Items, Totals, CouponInput)
   Render ContactInfo(Email)

   // Email is ALWAYS required
   IF Email.isEmpty THEN
       DisableAllSections()
   ELSE
       EnableContactSection()

       // Check if email exists in system
       IF Email.existsInDatabase THEN
           LoadUserData(Email)
           LoadSavedAddresses()
       ENDIF
   ENDIF

   IF Cart.hasPhysicalItems THEN
      IF User.hasSavedAddresses THEN
          Render AddressList()
          Render "Add New Address" Button
      ELSE
          Render DeliveryDetailsForm (inline)
      ENDIF

      Render AdditionalNoteCheckbox

      IF Address.isSelected OR DeliveryDetailsForm.isComplete THEN
         FetchShippingOptions(Address)
         IF ShippingOptions.found THEN
             Render ShippingMethodSelector
             SelectDefaultOption()
             UpdateSidebar(Total + ShippingCost)
         ELSE
             Render Error("Cannot ship") + Button("Remove Items")
         ENDIF
      ENDIF
   ELSE (Digital Products Only)
      Render ContactFields(FirstName, LastName, Phone)
      // Pre-fill if user data exists
   ENDIF

   CALC FinalTotal = Subtotal - Discount + Tax + Shipping

   IF FinalTotal == 0 THEN
       Render Button("Confirm Order")
   ELSE
       IF AllRequiredInfoComplete THEN
           Render PaymentMethodSelector(Stripe, PayPal, Crypto) [Enabled]
       ELSE
           Render PaymentMethodSelector(Stripe, PayPal, Crypto) [Disabled/Locked]
       ENDIF
   ENDIF

   // Save user data on order completion
   ON OrderSubmit:
       Save FirstName, LastName, Phone to UserProfile
       Attach UserID to Order
ENDIF
```

### B7) Testing Matrix for AI

- **T1:** Guest User > Fill Address > Verify Shipping Section Loads.
- **T2:** Enter Invalid Zip > Verify System Accepts it (No validation block).
- **T3:** Apply Coupon > Verify Sidebar Discount Row appears > Verify Total decreases.
- **T4:** Apply Huge Coupon > Total becomes 0 > Verify Payment Methods disappear > Verify "Confirm Order" appears.
- **T5:** Enter Address for unshippable item > Verify "Remove items" button appears > Click > Verify items removed.
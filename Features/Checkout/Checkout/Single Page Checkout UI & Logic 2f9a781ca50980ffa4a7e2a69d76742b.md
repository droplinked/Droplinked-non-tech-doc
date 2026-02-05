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
- Simplify address entry by removing strict third-party validation blocking (allow users to proceed).
- Provide immediate feedback on Shipping availability based on the entered address.
- Allow users to add optional notes to their order.
- Handle "Zero Cost" orders (due to discounts) by bypassing payment selection.

---

### 2) **Scope – In / Out**

**In:**

- **Layout:** Two-column layout (Left: Forms/Steps, Right: Sticky Sidebar).
- **Contact Info:** Email handling (Guest vs. Logged in).
- **Delivery Details:** Standard address form + Additional Note toggle.
- **Payment Section Logic:** Visibility conditions based on Order Total.
- **Sidebar Logic:** Real-time calculation of Subtotal, Shipping, Tax, Discount, Total.

**Out:**

- **Strict Address Validation:** No blocking validation (e.g., Google Maps API enforcement) preventing submission.
- **Multi-page navigation:** No "Next Step" buttons; distinct sections on one page instead.

---

### 3) **Key User Journeys**

**Journey 1: Guest Checkout (Physical Item)**

1. **Load:** Customer lands on Checkout. Sidebar shows cart items.
2. **Contact:** Enters Email Address.
3. **Address:** Fills Delivery Details (Name, Phone, Address).
4. **Note:** Checks "Add Additional Note", types a message.
5. **Shipping:** Upon address completion, Shipping section loads options. Customer selects one.
6. **Review:** Sidebar updates with Shipping cost and Total.
7. **Payment:** Customer selects Payment Method (e.g., Credit Card).
8. **Submit:** Clicks Pay/Confirm.

**Journey 2: Discount to Zero**

1. **Coupon:** Customer applies a coupon in Sidebar.
2. **Calc:** Total becomes $0.00.
3. **Payment Hidden:** Payment methods section disappears.
4. **Confirm:** "Confirm Order" button appears directly.
5. **Submit:** Customer clicks Confirm.

**Journey 3: No Shipping Available**

1. **Address:** Customer enters an address where items cannot be shipped.
2. **Error:** Shipping section displays "These items can't be shipped to your address."
3. **Action:** Customer clicks "Remove Items" to clear conflicting products.

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** The Checkout MUST be a single page with a right-side summary sidebar.
- **BAC 2:** Changing the address MUST automatically trigger a refresh of the Shipping section.
- **BAC 3:** If the Total Payment is $0, the Payment Methods section MUST be hidden.
- **BAC 4:** Address Form MUST NOT enforce external validation APIs (accept user input as-is).
- **BAC 5:** The sidebar MUST display: List of items, Subtotal, Shipping, Tax, Discount, and Total.

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
- **R8. Zero Payment Logic:**
    - Condition: If `Total == 0`.
    - Behavior: Hide "Payment Methods" selector. show "Confirm Order" button.
    - Action: Clicking Confirm creates the order immediately.

### B3) UI/UX States & Triggers

- **Sidebar Updates:** Any change in Quantity (if editable), Address (changes tax/shipping), or Coupon MUST trigger a recalculation of the Sidebar values.
- **Coupon Success:** Green text message: `{Value} discount code applied`.
- **Coupon Fail:** Red text message.
- **Address Complete:** Auto-advance focus or scroll isn't strictly necessary, but Shipping loading begins immediately.

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

   IF Cart.hasPhysicalItems THEN
      Render DeliveryDetailsForm
      Render AdditionalNoteCheckbox

      IF DeliveryDetailsForm.isComplete THEN
         FetchShippingOptions(Address)
         IF ShippingOptions.found THEN
             Render ShippingMethodSelector
             SelectDefaultOption()
             UpdateSidebar(Total + ShippingCost)
         ELSE
             Render Error("Cannot ship") + Button("Remove Items")
         ENDIF
      ENDIF
   ENDIF

   CALC FinalTotal = Subtotal - Discount + Tax + Shipping

   IF FinalTotal == 0 THEN
       Render Button("Confirm Order")
   ELSE
       Render PaymentMethodSelector(Stripe, PayPal, Crypto)
   ENDIF
ENDIF

```

### B7) Testing Matrix for AI

- **T1:** Guest User > Fill Address > Verify Shipping Section Loads.
- **T2:** Enter Invalid Zip > Verify System Accepts it (No validation block).
- **T3:** Apply Coupon > Verify Sidebar Discount Row appears > Verify Total decreases.
- **T4:** Apply Huge Coupon > Total becomes 0 > Verify Payment Methods disappear > Verify "Confirm Order" appears.
- **T5:** Enter Address for unshippable item > Verify "Remove items" button appears > Click > Verify items removed.
# Checkout Sidebar (Order Summary)

# Checkout Sidebar (Order Summary)

### Feature ID:

**[CHK-SIDEBAR-402]**

### Title:

**Checkout Sidebar - Order Summary**

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
During checkout, customers need continuous visibility of their cart contents and order costs. A sticky sidebar provides real-time feedback on how their selections (shipping method, discount codes) affect the final price, reducing confusion and building confidence in the purchase.

**Desired Outcome:**

- Display a persistent, sticky sidebar on the right side of the checkout page.
- Show all cart items with visual details (thumbnail, quantity, title, variants, price).
- Provide a coupon code input field for applying discounts.
- Display a clear financial breakdown: Items subtotal, Shipping, Tax, Discount, and Total.
- Update all values in real-time as the customer makes selections.
- Handle pending states gracefully (e.g., "Pending delivery details" for shipping before address is entered).

---

### 2) **Scope – In / Out**

**In Scope:**

- **Product List Display:**
    - Product thumbnail image
    - Quantity badge overlay on thumbnail
    - Product title
    - Variant options (e.g., "Green • Medium")
    - Line item price (unit price × quantity)
- **Coupon Code Section:**
    - Text input field with placeholder "Enter discount code"
    - Apply button
    - Success/Error feedback (See [CHK-PROMO-301] for detailed logic)
- **Financial Summary:**
    - Items (Subtotal of all products)
    - Shipping (Sum of selected shipping rates OR "Pending delivery details")
    - Tax (Calculated tax amount)
    - Discount (Applied discount amount, shown as negative)
    - Total (Final payable amount)
- **Real-time Updates:** All values update immediately on:
    - Shipping method selection change
    - Coupon code application
    - Item removal from cart
- **Sticky Behavior:** Sidebar remains visible while scrolling through checkout form.

**Out of Scope:**

- Quantity editing within checkout (handled in Cart page).
- Item removal buttons (only via Shipping "Remove Items" for unshippable products).
- Multiple currency display.
- Saved carts or wishlists.

---

### 3) **Key User Journeys**

---

**Journey 1: View Order Summary**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Navigates to checkout | Sidebar loads with all cart items |
| 2 | Customer | Scrolls down page | Sidebar remains sticky/visible |
| 3 | Customer | Reviews items | Each item shows thumbnail, qty, title, variant, price |

---

**Journey 2: See Pending Shipping State**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Has not filled address form yet | Shipping row shows "Pending delivery details" |
| 2 | Customer | Completes address form | Shipping row updates to loading state |
| 3 | System | Fetches shipping rates | Shipping row shows calculated amount |
| 4 | Customer | Sees updated total | Total reflects shipping cost |

---

**Journey 3: Apply Discount Code**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Types coupon code in input | Apply button becomes active |
| 2 | Customer | Clicks Apply | System validates code |
| 3a | System | Code valid | Success message, Discount row appears, Total updates |
| 3b | System | Code invalid | Error message, no changes to totals |

---

**Journey 4: Change Shipping Selection**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Selects different shipping option | Sidebar Shipping value updates |
| 2 | System | Recalculates | Total updates in real-time |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | Sidebar displays all cart items with thumbnail, quantity badge, title, variants, and price |  |
| BAC-2 | Quantity badge is overlaid on product thumbnail |  |
| BAC-3 | Coupon input field and Apply button are present |  |
| BAC-4 | Financial summary shows: Items, Shipping, Tax, Discount (if applied), Total |  |
| BAC-5 | Shipping row shows "Pending delivery details" before address is completed |  |
| BAC-6 | All values update in real-time when shipping selection changes |  |
| BAC-7 | All values update in real-time when coupon is applied |  |
| BAC-8 | Sidebar remains sticky while scrolling |  |
| BAC-9 | Discount row only appears when a valid coupon is applied |  |
| BAC-10 | Total calculation is accurate: (Items - Discount) + Shipping + Tax |  |

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
| **Sidebar** | The right-side panel in the checkout page that displays order summary |
| **Line Item Price** | Unit price × quantity for a single product |
| **Subtotal (Items)** | Sum of all line item prices |
| **Pending State** | A placeholder shown when data is not yet available |
| **Real-time Update** | Immediate UI refresh without page reload |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Product List Display Logic**

For each item in Cart:

| Element | Source | Display Format |
| --- | --- | --- |
| Thumbnail | product.images[0] | Small square image |
| Quantity Badge | cart_item.quantity | Number in small badge overlay (top-right of thumbnail) |
| Title | product.title | Text |
| Variants | cart_item.selected_options | "Option1 • Option2" format |
| Price | cart_item.unit_price × cart_item.quantity | "$XXX.XX USD" |

---

### **FL-2: Financial Summary Calculation**

| Row | Calculation | Display Format |
| --- | --- | --- |
| Items | SUM(item.unit_price × item.quantity) for all items | "$XX.XX USD" |
| Shipping | SUM(selected_shipping_rate.price) for all shipping groups | "$XX.XX USD" OR "Pending delivery details" |
| Tax | Calculated based on address and items | "$XX.XX USD" |
| Discount | coupon.discount_amount (if applied) | "-$XX.XX USD" |
| Total | Items + Shipping + Tax - Discount | "$XX.XX USD" |

---

### **FL-3: Shipping Row States**

| State | Condition | Display |
| --- | --- | --- |
| Pending | Address form not completed | "Pending delivery details" (gray text) |
| Loading | Address completed, fetching rates | Spinner or skeleton |
| Calculated | Rates received and selection made | "$XX.XX USD" |
| Error | No shipping available | Still shows "Pending" or last valid value |

---

### **FL-4: Discount Row Visibility**

| Condition | Behavior |
| --- | --- |
| No coupon applied | Discount row is HIDDEN |
| Valid coupon applied | Discount row VISIBLE with negative value |
| Coupon removed/invalidated | Discount row HIDDEN, totals recalculated |

---

### **FL-5: Real-time Update Triggers**

| Event | Sidebar Action |
| --- | --- |
| Shipping option changed | Update Shipping row, recalculate Total |
| Coupon applied successfully | Add/update Discount row, recalculate Total |
| Coupon removed | Remove Discount row, recalculate Total |
| Item removed (via shipping error) | Remove item from list, recalculate all rows |
| Address completed | Trigger shipping fetch, update Shipping row |

---

### **FL-6: Sticky Behavior Logic**

```
ON Scroll:
    IF viewport.scrollY > sidebar.initialTop:
        sidebar.position = "fixed"
        sidebar.top = "20px"
    ELSE:
        sidebar.position = "relative"

    IF sidebar.bottom > footer.top:
        sidebar.position = "absolute"
        sidebar.bottom = "0"

```

---

### B3) Behavioral Flow

```
[Checkout Page Loads]
    ↓
[Fetch Cart Data]
    ↓
[Render Product List]
    ├─ For each item: Thumbnail + Badge + Title + Variants + Price
    ↓
[Render Coupon Input Section]
    ↓
[Render Financial Summary]
    ├─ Items: Calculate subtotal
    ├─ Shipping: "Pending delivery details"
    ├─ Tax: Calculate if address available, else $0.00
    ├─ Discount: Hidden (no coupon yet)
    └─ Total: Items + Tax
    ↓
[Listen for Events]
    ├─ Address Complete → Fetch shipping → Update Shipping row
    ├─ Shipping Selected → Update Shipping amount → Recalculate Total
    ├─ Coupon Applied → Show Discount row → Recalculate Total
    └─ Item Removed → Remove from list → Recalculate all

```

---

### B4) State Transitions

| Current State | Event | New State | UI Change |
| --- | --- | --- | --- |
| Shipping: Pending | Address completed | Shipping: Loading | Show spinner |
| Shipping: Loading | Rates received | Shipping: Calculated | Show price |
| Shipping: Calculated | Shipping selection changed | Shipping: Calculated | Update price |
| Discount: Hidden | Valid coupon applied | Discount: Visible | Show discount row |
| Discount: Visible | Coupon removed | Discount: Hidden | Hide discount row |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Cart has 10+ items | Scrollable product list within sidebar |
| **EC-2:** Very long product title | Truncate with ellipsis (...) |
| **EC-3:** Product has no variants | Variant line hidden for that item |
| **EC-4:** Shipping fetch fails | Keep "Pending" state, show retry in shipping section |
| **EC-5:** Tax calculation fails | Show $0.00 or "Calculating..." |
| **EC-6:** Discount > Subtotal | Discount capped at Subtotal, Total = Shipping + Tax |
| **EC-7:** All items removed | Redirect to cart or show empty state |
| **EC-8:** Currency mismatch | All prices in shop's default currency (USD) |

---

### B6) Data Requirements

**Sidebar Data Model:**

| Field | Type | Description |
| --- | --- | --- |
| items | Array[CartItem] | List of products in cart |
| subtotal | Decimal | Sum of item prices |
| shipping_total | Decimal | Sum of shipping costs |
| tax_total | Decimal | Calculated tax |
| discount_total | Decimal | Applied discount |
| grand_total | Decimal | Final payable amount |
| coupon_code | String (nullable) | Applied coupon code |
| shipping_pending | Boolean | Whether shipping is pending address |

**CartItem Structure:**

| Field | Type |
| --- | --- |
| product_id | String |
| title | String |
| thumbnail_url | String |
| quantity | Integer |
| unit_price | Decimal |
| selected_options | Array[String] |
| line_total | Decimal |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Load checkout with 3 items | All 3 items displayed with correct info |
| TC-2 | Item with quantity 5 | Badge shows "5" on thumbnail |
| TC-3 | Product with variants "Red, Large" | Displays "Red • Large" |
| TC-4 | Product without variants | No variant line shown |
| TC-5 | Before address entry | Shipping shows "Pending delivery details" |
| TC-6 | After address entry | Shipping shows calculated amount |
| TC-7 | Change shipping option | Sidebar updates in real-time |
| TC-8 | Apply valid coupon | Discount row appears, total updates |
| TC-9 | Apply invalid coupon | Error shown, no sidebar changes |
| TC-10 | Scroll down page | Sidebar stays visible (sticky) |
| TC-11 | Remove item via shipping error | Item removed, totals recalculated |
| TC-12 | 100% discount | Discount equals subtotal, total = shipping + tax |
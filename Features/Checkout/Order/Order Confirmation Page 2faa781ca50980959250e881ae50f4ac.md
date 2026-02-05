# Order Confirmation Page

# Order Confirmation Page

### Feature ID:

**[CHK-ORD-001]**

### Title:

**Order Confirmation Page**

### Category:

**Checkout / Order** | **Actors**: Customer | **Channel**: Storefront

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
After completing a payment, customers need immediate confirmation that their order was successfully processed. They also need a central location to view all relevant order details including payment information, delivery address, purchased items, and access to any digital goods or NFTs associated with their purchase.

**Desired Outcome:**

- Customers receive visual confirmation of their order status (Pending, Success, Failed).
- All order-related information is displayed in a clear, organized layout.
- Customers can access digital product downloads directly from this page.
- Customers can claim NFTs associated with recorded products.
- An email notification is sent containing a link to this page.
- The page remains accessible via Order History for future reference.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Order Status Display:** Visual indicator showing Pending, Success, or Failed status.
- **Order Information Section:** Order ID, Order Date, Order Status.
- **Payment Details Sidebar:** Payment Method, Subtotal, Shipping, Tax, Discount, Total Paid.
- **Delivery Details Section:** Email, Full Address, Full Name, Phone Number, Additional Note.
- **NFT Section:** List of claimable NFTs with Claim/View functionality (See [WEB3-CNFT-005]).
- **Digital Goods Section:** List of downloadable files with direct download links (See [CHK-DIG-001]).
- **Product List:** All purchased items with thumbnail, title, variant info, quantity, and price.
- **Email Notification:** Triggered on order confirmation containing page link.
- **Public Access:** Page accessible without login via direct link.
- **Order History Access:** Page accessible through customer's Order History.

**Out of Scope:**

- Email content and template design (Separate feature).
- Order editing or cancellation functionality.
- Refund request initiation.
- Real-time order tracking/shipping updates.
- Payment retry from this page (handled in Checkout).

---

### 3) **Key User Journeys**

---

**Journey 1: View Order After Successful Payment**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Completes payment successfully | System redirects to Order Confirmation Page |
| 2 | Customer | Views page | Page displays Success status with green indicator |
| 3 | Customer | Reviews order details | All sections render with complete order data |

---

**Journey 2: View Order with Pending Status**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Payment processing takes time | System redirects to Order Confirmation Page |
| 2 | Customer | Views page | Page displays Pending status with appropriate indicator |
| 3 | Customer | Refreshes page later | Status updates when payment confirms |

---

**Journey 3: View Order with Failed Payment**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Payment fails but order created | System redirects to Order Confirmation Page |
| 2 | Customer | Views page | Page displays Failed status with error indicator |
| 3 | Customer | Needs to retry payment | Directed back to checkout to retry |

---

**Journey 4: Access Order via Email Link**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | System | Sends confirmation email | Email contains direct link to Order Confirmation Page |
| 2 | Customer | Clicks link in email | Browser opens Order Confirmation Page |
| 3 | Customer | Views order (logged in or guest) | Full order details displayed |

---

**Journey 5: Access Order via Order History**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Navigates to Order History | List of past orders displayed |
| 2 | Customer | Clicks on specific order | Redirects to Order Confirmation Page for that order |
| 3 | Customer | Views historical order | All original order details displayed |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | Order status (Pending/Success/Failed) is visually displayed with distinct indicators |  |
| BAC-2 | Order Information section displays Order ID, Date, and Status |  |
| BAC-3 | Payment Details sidebar shows Payment Method, Subtotal, Shipping, Tax, Discount, Total Paid |  |
| BAC-4 | Delivery Details section displays Email, Address, Name, Phone, Additional Note |  |
| BAC-5 | NFT section appears when order contains recorded NFT products |  |
| BAC-6 | Digital Goods section appears when order contains products with downloadable files |  |
| BAC-7 | Product list displays all items with image, title, variants, quantity, and price |  |
| BAC-8 | Confirmation email is sent with link to this page upon order creation |  |
| BAC-9 | Page is publicly accessible via direct link (no login required) |  |
| BAC-10 | Page is accessible via Order History for logged-in customers |  |

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
| **Order Confirmation Page** | The post-purchase page that displays complete order information and provides access to digital assets |
| **Order Status** | The current state of the order: Pending (payment processing), Success (payment confirmed), Failed (payment failed) |
| **Digital Goods** | Products that include downloadable files delivered electronically |
| **Recorded NFT Product** | A product that has been minted as an NFT on the blockchain and can be claimed by the customer |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Page Access Logic**

| Condition | System Behavior |
| --- | --- |
| Customer completes checkout (any payment result) | Redirect to Order Confirmation Page with Order ID |
| Customer clicks email link | Load Order Confirmation Page for specified Order ID |
| Customer clicks order in Order History | Load Order Confirmation Page for specified Order ID |
| Invalid or non-existent Order ID accessed | Display "Order not found" error page |

---

### **FL-2: Order Status Display Logic**

| Status | Visual Indicator | Description Text |
| --- | --- | --- |
| Pending | Yellow/Orange indicator | "Your order is being processed" |
| Success | Green indicator | "Order Confirmed!" |
| Failed | Red indicator | "Payment Failed" |

---

### **FL-3: Section Visibility Logic**

| Section | Visibility Condition |
| --- | --- |
| Order Information | Always visible |
| Payment Details | Always visible |
| Delivery Details | Visible if order contains physical/shippable products |
| NFT Section | Visible if order contains products with recorded NFTs |
| Digital Goods Section | Visible if order contains products with downloadable files |
| Product List | Always visible |

---

### **FL-4: Payment Details Calculation Display**

| Field | Source | Format |
| --- | --- | --- |
| Payment Method | Order.payment_method | "Credit Card" / "PayPal" / "Crypto" |
| Subtotal | Sum of (item.price × item.quantity) for all items | $XX.XX USD |
| Shipping | Order.shipping_cost | $XX.XX USD |
| Tax | Order.tax_amount | $XX.XX USD |
| Discount | Order.discount_amount | -$XX.XX USD (negative) |
| Paid (Total) | Subtotal + Shipping + Tax - Discount | $XX.XX USD |

---

### **FL-5: Delivery Details Display Logic**

| Field | Source | Display Rule |
| --- | --- | --- |
| Email | Order.customer_email | Always shown in Delivery section |
| Address | Order.shipping_address | Format: "Line 1, City, State, Zip, Country" |
| Full Name | Order.shipping_name | First Name + Last Name |
| Phone Number | Order.shipping_phone | As entered |
| Additional Note | Order.additional_note | Only shown if not empty |

---

### **FL-6: Product List Display Logic**

For each item in Order.items:

| Element | Source | Display |
| --- | --- | --- |
| Thumbnail | item.product.thumbnail_url | Small image |
| Title | item.product.title | Product name |
| Variants | item.variant_options | e.g., "Green, Medium" |
| Quantity | item.quantity | Numeric |
| Price | item.unit_price × item.quantity | $XX.XX USD |

---

### B3) Behavioral Flow

```
[Payment Complete]
    ↓
[Redirect to Order Confirmation Page]
    ↓
[Load Order Data by Order ID]
    ↓
[Determine Order Status] → [Display Status Indicator]
    ↓
[Render Order Information Section]
    ↓
[Render Payment Details Sidebar]
    ↓
[Check: Has Shippable Items?]
    ├─ YES → [Render Delivery Details Section]
    └─ NO → [Skip Delivery Section]
    ↓
[Check: Has NFT Products?]
    ├─ YES → [Render NFT Section with Claim Buttons]
    └─ NO → [Skip NFT Section]
    ↓
[Check: Has Digital Products with Files?]
    ├─ YES → [Render Digital Goods Section with Download Links]
    └─ NO → [Skip Digital Goods Section]
    ↓
[Render Product List]
    ↓
[Trigger Confirmation Email (async)]

```

---

### B4) State Transitions

| Current State | Trigger | New State | Side Effects |
| --- | --- | --- | --- |
| Payment Processing | Payment confirmed | Order Success | Email sent, status updated |
| Payment Processing | Payment timeout/failure | Order Failed | Status updated |
| Order Pending | Background payment confirmation | Order Success | Email sent, status updated |
| Order Failed | Customer retries payment | Payment Processing | New payment attempt initiated |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Order ID not found | Display "Order not found" error with link to homepage |
| **EC-2:** Order has no items (data corruption) | Display error, log incident, show support contact |
| **EC-3:** Product image unavailable | Display placeholder image |
| **EC-4:** Page loaded during status transition | Show current status, page refreshable for updates |
| **EC-5:** Email delivery fails | Order page still accessible, email retry in background |
| **EC-6:** Mixed order (physical + digital + NFT) | All relevant sections displayed |
| **EC-7:** Order with $0 total (100% discount) | Payment Method shows "N/A" or "Free Order", Paid shows $0.00 |
| **EC-8:** Guest user loses email | Can contact support with Order ID if known |

---

### B6) Data Requirements

**Order Object Structure:**

| Field | Type | Description |
| --- | --- | --- |
| order_id | String | Unique order identifier |
| status | Enum | PENDING, SUCCESS, FAILED |
| created_at | DateTime | Order creation timestamp |
| customer_email | String | Customer's email address |
| payment_method | String | Payment method used |
| subtotal | Decimal | Sum of item prices |
| shipping_cost | Decimal | Shipping charges |
| tax_amount | Decimal | Tax applied |
| discount_amount | Decimal | Discount applied |
| total_paid | Decimal | Final amount charged |
| shipping_address | Object | Full address details |
| additional_note | String (nullable) | Customer's note |
| items | Array[OrderItem] | List of purchased items |

**OrderItem Object:**

| Field | Type | Description |
| --- | --- | --- |
| product_id | String | Product identifier |
| product_title | String | Product name |
| thumbnail_url | String | Product image URL |
| variant_options | Array[String] | Selected variant values |
| quantity | Integer | Quantity purchased |
| unit_price | Decimal | Price per unit |
| has_nft | Boolean | Product has claimable NFT |
| has_digital_files | Boolean | Product has downloadable files |
| digital_files | Array[File] | List of downloadable files |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Load page with valid Order ID (Success status) | All sections render correctly with green status |
| TC-2 | Load page with valid Order ID (Pending status) | Pending indicator shown |
| TC-3 | Load page with valid Order ID (Failed status) | Failed indicator shown |
| TC-4 | Load page with invalid Order ID | Error page displayed |
| TC-5 | Order with physical products only | Delivery section visible, NFT/Digital hidden |
| TC-6 | Order with digital products only | Digital section visible, Delivery hidden |
| TC-7 | Order with NFT products | NFT section visible with claim buttons |
| TC-8 | Order with mixed product types | All relevant sections visible |
| TC-9 | Guest user accesses via email link | Page loads correctly without login |
| TC-10 | Logged-in user accesses via Order History | Page loads correctly |
| TC-11 | Order with $0 total | Payment section shows Free Order |
| TC-12 | Order with additional note | Note displayed in Delivery section |
| TC-13 | Order without additional note | Note field hidden |
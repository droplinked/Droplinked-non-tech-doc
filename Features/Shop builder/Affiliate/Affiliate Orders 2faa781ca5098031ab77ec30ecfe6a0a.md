# Affiliate Orders

# Affiliate Orders

### Feature ID:

**[IAA-AFF-504]**

### Title:

**Affiliate Orders – Order Processing & Tracking**

### Category:

**Affiliate** | **Actors**: Merchant (Affiliator), Merchant (Co-seller), Customer | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
When a customer purchases an imported affiliate product, both the Affiliator and Co-seller need visibility into the order. The Affiliator is responsible for fulfillment (shipping), while the Co-seller earns commission. Both parties need to see these orders in their dashboard with relevant information.

**Desired Outcome:**

- Affiliate orders appear in both parties' order management.
- New "Affiliate" tab in Orders section for dedicated tracking.
- Affiliator sees order with fulfillment responsibilities.
- Co-seller sees order with commission information.
- Payment is automatically split via credits.
- Order data is simple and clear for both parties.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Order Creation**
    - Order created when customer completes purchase
    - Order linked to both Affiliator and Co-seller
    - Commission calculated and recorded
- **Affiliator Order View**
    - New "Affiliate" tab in Orders
    - See orders from Co-sellers
    - Responsible for fulfillment/shipping
    - See customer info and shipping details
    - See commission deducted
- **Co-seller Order View**
    - New "Affiliate" tab in Orders
    - See orders for imported products
    - See commission earned
    - No fulfillment responsibility
- **Payment Split**
    - Automatic credit distribution
    - Co-seller receives commission %
    - Affiliator receives (price - commission)
    - Credits available immediately after payment

**Out of Scope:**

- Refund handling
- Order cancellation flow
- Dispute resolution
- Manual payment adjustments
- Order status updates by Co-seller

---

### 3) **Key User Journeys**

---

**Journey 1: Customer Places Order**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Completes checkout on Co-seller's shop | Payment processed |
| 2 | System | Identifies affiliate products | Marks order as affiliate |
| 3 | System | Calculates commission | Based on product's commission rate |
| 4 | System | Creates order record for Co-seller | With commission info |
| 5 | System | Creates order record for Affiliator | With fulfillment info |
| 6 | System | Splits payment | Credits both accounts |
| 7 | Both | Receive order notification | Order visible in dashboard |

---

**Journey 2: Affiliator Views Affiliate Orders**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Goes to Orders | Order management page |
| 2 | Affiliator | Clicks "Affiliate" tab | Affiliate orders listed |
| 3 | Affiliator | Views order list | See: Order ID, Date, Co-seller, Products, Total, Commission, Net |
| 4 | Affiliator | Clicks on order | Order detail page |
| 5 | Affiliator | Sees customer info | Name, address, contact for shipping |
| 6 | Affiliator | Sees products ordered | With quantities |
| 7 | Affiliator | Sees financial breakdown | Total, Commission paid, Net earned |
| 8 | Affiliator | Manages fulfillment | Responsible for shipping |

---

**Journey 3: Co-seller Views Affiliate Orders**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Goes to Orders | Order management page |
| 2 | Co-seller | Clicks "Affiliate" tab | Affiliate orders listed |
| 3 | Co-seller | Views order list | See: Order ID, Date, Affiliator Shop, Products, Total, Commission Earned |
| 4 | Co-seller | Clicks on order | Order detail page |
| 5 | Co-seller | Sees order info | Products, quantities, prices |
| 6 | Co-seller | Sees commission earned | Calculated amount |
| 7 | Co-seller | Does NOT see customer details | Shipping handled by Affiliator |

---

**Journey 4: Payment Distribution**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Pays $100 for product | Payment to Droplinked |
| 2 | System | Commission rate = 15% | Calculate split |
| 3 | System | Credits Co-seller | $15 to shop credit |
| 4 | System | Credits Affiliator | $85 to shop credit |
| 5 | Both | Check balance | Credits available immediately |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Order Creation** |  |
| BAC-1 | Affiliate order is created for both Affiliator and Co-seller |
| BAC-2 | Commission is calculated based on product's commission rate at time of order |
| BAC-3 | Order contains product details, quantities, and prices |
| **Affiliator View** |  |
| BAC-4 | "Affiliate" tab appears in Orders section |
| BAC-5 | List shows: Order ID, Date, Co-seller Name, Products, Total, Commission Paid, Net Amount |
| BAC-6 | Detail view shows customer shipping information |
| BAC-7 | Affiliator is responsible for order fulfillment |
| BAC-8 | Financial breakdown is clear |
| **Co-seller View** |  |
| BAC-9 | "Affiliate" tab appears in Orders section |
| BAC-10 | List shows: Order ID, Date, Affiliator Shop, Products, Total, Commission Earned |
| BAC-11 | Detail view shows order info but NOT customer shipping details |
| BAC-12 | Commission earned is clearly displayed |
| **Payment** |  |
| BAC-13 | Payment is split automatically |
| BAC-14 | Co-seller receives commission percentage as credit |
| BAC-15 | Affiliator receives (total - commission) as credit |
| BAC-16 | Credits are available immediately after payment confirmation |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Exhaustive Functional Logic

---

### **FL-1: Order Creation Flow**

```
ON CheckoutComplete(cart, payment):
    FOR EACH item IN cart.items:
        IF item.is_affiliate:
            // Create affiliate order
            affiliateOrder = createAffiliateOrder(item, payment)

            // Calculate commission
            commission = item.price * item.quantity * (item.commission_rate / 100)
            affiliatorAmount = (item.price * item.quantity) - commission

            // Credit both parties
            API.creditShop(item.coseller_shop_id, commission)
            API.creditShop(item.affiliator_shop_id, affiliatorAmount)

            // Create order records
            createAffiliatorOrderRecord(affiliateOrder, affiliatorAmount, commission)
            createCosellerOrderRecord(affiliateOrder, commission)
        ELSE:
            // Normal order flow
            createRegularOrder(item, payment)

```

---

### **FL-2: Affiliate Order Data Structure**

```
AffiliateOrder = {
    id: UUID,
    created_at: DateTime,

    // Product info
    product_id: UUID,
    product_title: String,
    product_image: String,
    quantity: Integer,
    unit_price: Decimal,
    total_price: Decimal,

    // Parties
    affiliator_shop_id: UUID,
    affiliator_shop_name: String,
    coseller_shop_id: UUID,
    coseller_shop_name: String,

    // Commission
    commission_rate: Integer,
    commission_amount: Decimal,
    affiliator_amount: Decimal,

    // Customer (for Affiliator only)
    customer_name: String,
    customer_email: String,
    shipping_address: Object,

    // Status
    fulfillment_status: Enum,  // pending, shipped, delivered
    payment_status: 'paid'
}

```

---

### **FL-3: Affiliator Order List Query**

```
ON LoadAffiliatorOrders(shop_id, filters):
    orders = DB.query(`
        SELECT
            ao.id,
            ao.created_at,
            ao.product_title,
            ao.quantity,
            ao.total_price,
            ao.commission_amount,
            ao.affiliator_amount,
            ao.coseller_shop_name,
            ao.fulfillment_status
        FROM affiliate_orders ao
        WHERE ao.affiliator_shop_id = {shop_id}
        AND ao.created_at BETWEEN {filters.date_from} AND {filters.date_to}
        ORDER BY ao.created_at DESC
    `)

    RETURN orders

```

---

### **FL-4: Co-seller Order List Query**

```
ON LoadCosellerOrders(shop_id, filters):
    orders = DB.query(`
        SELECT
            ao.id,
            ao.created_at,
            ao.product_title,
            ao.quantity,
            ao.total_price,
            ao.commission_amount,
            ao.affiliator_shop_name
        FROM affiliate_orders ao
        WHERE ao.coseller_shop_id = {shop_id}
        AND ao.created_at BETWEEN {filters.date_from} AND {filters.date_to}
        ORDER BY ao.created_at DESC
    `)

    // Note: Customer details not included for Co-seller
    RETURN orders

```

---

### **FL-5: Order Detail View - Affiliator**

```
ON LoadAffiliatorOrderDetail(order_id, shop_id):
    order = DB.query(`
        SELECT * FROM affiliate_orders
        WHERE id = {order_id}
        AND affiliator_shop_id = {shop_id}
    `)

    IF order == null:
        RETURN 404

    RETURN {
        orderInfo: {
            id: order.id,
            date: order.created_at,
            status: order.fulfillment_status
        },
        product: {
            title: order.product_title,
            image: order.product_image,
            quantity: order.quantity,
            unitPrice: order.unit_price,
            totalPrice: order.total_price
        },
        coseller: {
            shopName: order.coseller_shop_name
        },
        customer: {
            name: order.customer_name,
            email: order.customer_email,
            address: order.shipping_address
        },
        financial: {
            totalAmount: order.total_price,
            commissionPaid: order.commission_amount,
            netEarned: order.affiliator_amount
        }
    }

```

---

### **FL-6: Order Detail View - Co-seller**

```
ON LoadCosellerOrderDetail(order_id, shop_id):
    order = DB.query(`
        SELECT
            id, created_at, product_title, product_image,
            quantity, unit_price, total_price,
            commission_amount, affiliator_shop_name
        FROM affiliate_orders
        WHERE id = {order_id}
        AND coseller_shop_id = {shop_id}
    `)

    IF order == null:
        RETURN 404

    // Note: No customer info for Co-seller
    RETURN {
        orderInfo: {
            id: order.id,
            date: order.created_at
        },
        product: {
            title: order.product_title,
            image: order.product_image,
            quantity: order.quantity,
            unitPrice: order.unit_price,
            totalPrice: order.total_price
        },
        affiliator: {
            shopName: order.affiliator_shop_name
        },
        financial: {
            totalAmount: order.total_price,
            commissionEarned: order.commission_amount
        }
    }

```

---

### **FL-7: Credit Distribution**

```
ON PaymentConfirmed(order):
    IF order.is_affiliate:
        // Calculate amounts
        commission = order.total_price * (order.commission_rate / 100)
        affiliatorAmount = order.total_price - commission

        // Credit Co-seller
        transaction1 = {
            shop_id: order.coseller_shop_id,
            amount: commission,
            type: 'affiliate_commission',
            reference: order.id,
            description: "Commission from order #" + order.id
        }
        API.createCreditTransaction(transaction1)

        // Credit Affiliator
        transaction2 = {
            shop_id: order.affiliator_shop_id,
            amount: affiliatorAmount,
            type: 'affiliate_sale',
            reference: order.id,
            description: "Affiliate sale #" + order.id + " (after commission)"
        }
        API.createCreditTransaction(transaction2)

        // Update shop balances
        API.incrementShopBalance(order.coseller_shop_id, commission)
        API.incrementShopBalance(order.affiliator_shop_id, affiliatorAmount)

```

---

### B2) Order List UI

**Affiliator Tab Columns:**

| Column | Description |
| --- | --- |
| Order ID | Unique order identifier |
| Date | Order creation date |
| Co-seller | Shop name that sold the product |
| Product | Product title + thumbnail |
| Qty | Quantity ordered |
| Total | Total order amount |
| Commission Paid | Amount paid to Co-seller |
| Net Earned | Amount received by Affiliator |
| Status | Fulfillment status |

**Co-seller Tab Columns:**

| Column | Description |
| --- | --- |
| Order ID | Unique order identifier |
| Date | Order creation date |
| Affiliator | Shop that owns the product |
| Product | Product title + thumbnail |
| Qty | Quantity ordered |
| Total | Total order amount |
| Commission Earned | Amount received by Co-seller |

---

### B3) Behavioral Flow

```
[Customer Completes Checkout]
    ↓
[System Identifies Affiliate Products]
    ↓
[Calculate Commission]
    ↓
[Create Order Records]
    ├─ [Affiliator Record] → Full details + customer info
    └─ [Co-seller Record] → Order details only
        ↓
[Distribute Credits]
    ├─ [Co-seller] → Commission amount
    └─ [Affiliator] → Total - Commission
        ↓
[Orders Appear in Dashboards]
    ├─ [Affiliator] → Sees in "Affiliate" tab, handles shipping
    └─ [Co-seller] → Sees in "Affiliate" tab, views earnings

```

---

### B4) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Mixed order (affiliate + regular products) | Split into separate order records |
| **EC-2:** Payment fails | No order created, no credits |
| **EC-3:** Commission rate is 0% (edge) | All goes to Affiliator |
| **EC-4:** Commission rate is 99% (edge) | Almost all goes to Co-seller |
| **EC-5:** Product price changed between cart and checkout | Use checkout price |
| **EC-6:** Multiple affiliate products from different Affiliators | Create separate orders per Affiliator |

---

### B5) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Customer completes affiliate purchase | Order created for both parties |
| TC-2 | Affiliator views affiliate orders | List with all columns displayed |
| TC-3 | Co-seller views affiliate orders | List without customer info |
| TC-4 | Affiliator views order detail | Customer shipping info shown |
| TC-5 | Co-seller views order detail | No customer info shown |
| TC-6 | Commission calculation (15% of $100) | Co-seller gets $15, Affiliator gets $85 |
| TC-7 | Check shop credits after order | Both balances updated correctly |
| TC-8 | Order with multiple affiliate products | Separate records per product/Affiliator |
| TC-9 | Date filter on orders | Only matching orders shown |
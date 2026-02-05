# Affiliate Dashboard & Analytics

# Affiliate Dashboard & Analytics

### Feature ID:

**[IAA-AFF-505]**

### Title:

**Affiliate Dashboard & Analytics**

### Category:

**Affiliate** | **Actors**: Merchant (Affiliator), Merchant (Co-seller) | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Both Affiliators and Co-sellers need visibility into their affiliate performance. Affiliators want to track which Co-sellers are selling their products and how much revenue they're generating. Co-sellers want to track their earnings from different Affiliators.

**Desired Outcome:**

- Dedicated dashboard showing affiliate role (Affiliator or Co-seller).
- Affiliator Dashboard: Track Co-sellers, products sold, revenue, and commissions paid.
- Co-seller Dashboard: Track shops imported from, products, sales, and commissions earned.
- Date picker for filtering metrics.
- Summary cards with key metrics.
- Detailed breakdowns by partner.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Role Display**
    - Show current affiliate role prominently
    - Role cannot be changed
- **Affiliator Dashboard**
    - List of Co-sellers who imported products
    - Per Co-seller: products imported, sales count, net profit
    - Summary: Total sales, net profit, total revenue
    - Date range picker for filtering
- **Co-seller Dashboard**
    - List of shops imported from
    - Per shop: products imported, sales count, commission earned
    - Summary: Total orders, products imported, net commission
    - Date range picker for filtering
- **Drill-down Views**
    - Click Co-seller to see imported products detail
    - Click shop to see products from that shop

**Out of Scope:**

- Graphical charts/visualizations
- Export to CSV/PDF
- Real-time updates
- Comparison periods
- Goal setting

---

### 3) **Key User Journeys**

---

**Journey 1: Affiliator Views Dashboard Overview**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Opens Affiliate Dashboard | Dashboard loads |
| 2 | System | Displays role | "You are an Affiliator" |
| 3 | System | Shows summary cards | Total Sales, Net Profit, Revenue (in period) |
| 4 | System | Shows Co-seller list | All who imported their products |
| 5 | Affiliator | Adjusts date picker | Metrics recalculate for period |

---

**Journey 2: Affiliator Drills Down to Co-seller**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Clicks on a Co-seller row | Detail view opens |
| 2 | System | Shows products imported | List of products this Co-seller imported |
| 3 | System | Shows per-product stats | Sales count, revenue per product |
| 4 | System | Shows totals | Total sales, net profit from this Co-seller |

---

**Journey 3: Co-seller Views Dashboard Overview**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Opens Affiliate Dashboard | Dashboard loads |
| 2 | System | Displays role | "You are a Co-seller" |
| 3 | System | Shows summary cards | Total Orders, Products Imported, Net Commission (in period) |
| 4 | System | Shows shop list | All shops they imported from |
| 5 | Co-seller | Adjusts date picker | Metrics recalculate for period |

---

**Journey 4: Co-seller Drills Down to Shop**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Clicks on a shop row | Detail view opens |
| 2 | System | Shows products imported | List of products from this shop |
| 3 | System | Shows per-product stats | Sales count, commission per product |
| 4 | System | Shows totals | Total sales, net commission from this shop |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **General** |  |
| BAC-1 | Dashboard clearly displays current affiliate role |
| BAC-2 | Role indicator shows "Affiliator" or "Co-seller" |
| BAC-3 | Date picker allows filtering all metrics |
| BAC-4 | Default date range is last 30 days |
| **Affiliator Dashboard** |  |
| BAC-5 | Summary shows: Total Sales Count, Net Profit, Total Revenue |
| BAC-6 | Net Profit = Total Revenue - Commissions Paid |
| BAC-7 | List shows all Co-sellers who imported products |
| BAC-8 | Per Co-seller row shows: Name, Products Imported, Sales Count, Net Profit |
| BAC-9 | Click Co-seller opens detail with product breakdown |
| **Co-seller Dashboard** |  |
| BAC-10 | Summary shows: Total Orders, Products Imported, Net Commission |
| BAC-11 | List shows all shops imported from |
| BAC-12 | Per shop row shows: Name, Products Imported, Sales Count, Commission Earned |
| BAC-13 | Click shop opens detail with product breakdown |
| **Date Filtering** |  |
| BAC-14 | All metrics respect selected date range |
| BAC-15 | Historical data is preserved and accessible |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Affiliator Dashboard Structure

```
AffiliatorDashboard:
    ├── RoleIndicator: "You are an Affiliator"
    │
    ├── DatePicker: [From Date] - [To Date]
    │
    ├── SummaryCards:
    │   ├── Total Sales: {count}
    │   ├── Net Profit: ${amount}
    │   └── Total Revenue: ${amount}
    │
    └── CosellerList:
        └── Table:
            - Co-seller Name
            - Products Imported
            - Sales Count
            - Net Profit
            - [View Details →]

```

---

### B2) Co-seller Dashboard Structure

```
CosellerDashboard:
    ├── RoleIndicator: "You are a Co-seller"
    │
    ├── DatePicker: [From Date] - [To Date]
    │
    ├── SummaryCards:
    │   ├── Total Orders: {count}
    │   ├── Products Imported: {count}
    │   └── Net Commission: ${amount}
    │
    └── ShopList:
        └── Table:
            - Shop Name
            - Products Imported
            - Sales Count
            - Commission Earned
            - [View Details →]

```

---

### B3) Exhaustive Functional Logic

---

### **FL-1: Load Affiliator Dashboard**

```
ON LoadAffiliatorDashboard(shop_id, dateRange):
    // Get summary metrics
    summary = DB.query(`
        SELECT
            COUNT(*) as total_sales,
            SUM(total_price) as total_revenue,
            SUM(affiliator_amount) as net_profit,
            SUM(commission_amount) as commissions_paid
        FROM affiliate_orders
        WHERE affiliator_shop_id = {shop_id}
        AND created_at BETWEEN {dateRange.from} AND {dateRange.to}
    `)

    // Get Co-seller breakdown
    cosellers = DB.query(`
        SELECT
            coseller_shop_id,
            coseller_shop_name,
            COUNT(DISTINCT affiliate_product_id) as products_imported,
            COUNT(*) as sales_count,
            SUM(affiliator_amount) as net_profit
        FROM affiliate_orders
        WHERE affiliator_shop_id = {shop_id}
        AND created_at BETWEEN {dateRange.from} AND {dateRange.to}
        GROUP BY coseller_shop_id, coseller_shop_name
        ORDER BY net_profit DESC
    `)

    RETURN {
        role: 'affiliator',
        summary: {
            totalSales: summary.total_sales,
            netProfit: summary.net_profit,
            totalRevenue: summary.total_revenue
        },
        cosellers: cosellers
    }

```

---

### **FL-2: Load Co-seller Dashboard**

```
ON LoadCosellerDashboard(shop_id, dateRange):
    // Get summary metrics
    summary = DB.query(`
        SELECT
            COUNT(*) as total_orders,
            SUM(commission_amount) as net_commission
        FROM affiliate_orders
        WHERE coseller_shop_id = {shop_id}
        AND created_at BETWEEN {dateRange.from} AND {dateRange.to}
    `)

    // Get total imported products (not time-filtered)
    totalImported = DB.query(`
        SELECT COUNT(*) as count
        FROM imported_products
        WHERE coseller_shop_id = {shop_id}
    `)

    // Get shop breakdown
    shops = DB.query(`
        SELECT
            affiliator_shop_id,
            affiliator_shop_name,
            COUNT(DISTINCT affiliate_product_id) as products_imported,
            COUNT(*) as sales_count,
            SUM(commission_amount) as commission_earned
        FROM affiliate_orders
        WHERE coseller_shop_id = {shop_id}
        AND created_at BETWEEN {dateRange.from} AND {dateRange.to}
        GROUP BY affiliator_shop_id, affiliator_shop_name
        ORDER BY commission_earned DESC
    `)

    RETURN {
        role: 'co-seller',
        summary: {
            totalOrders: summary.total_orders,
            productsImported: totalImported.count,
            netCommission: summary.net_commission
        },
        shops: shops
    }

```

---

### **FL-3: Affiliator Detail View (Per Co-seller)**

```
ON LoadCosellerDetail(affiliator_shop_id, coseller_shop_id, dateRange):
    // Get Co-seller info
    coseller = DB.getShop(coseller_shop_id)

    // Get products and their performance
    products = DB.query(`
        SELECT
            product_id,
            product_title,
            product_image,
            COUNT(*) as sales_count,
            SUM(total_price) as total_revenue,
            SUM(affiliator_amount) as net_profit,
            SUM(commission_amount) as commission_paid
        FROM affiliate_orders
        WHERE affiliator_shop_id = {affiliator_shop_id}
        AND coseller_shop_id = {coseller_shop_id}
        AND created_at BETWEEN {dateRange.from} AND {dateRange.to}
        GROUP BY product_id, product_title, product_image
        ORDER BY net_profit DESC
    `)

    // Calculate totals
    totals = {
        salesCount: products.sum(p => p.sales_count),
        netProfit: products.sum(p => p.net_profit),
        totalRevenue: products.sum(p => p.total_revenue)
    }

    RETURN {
        coseller: coseller,
        products: products,
        totals: totals
    }

```

---

### **FL-4: Co-seller Detail View (Per Shop)**

```
ON LoadShopDetail(coseller_shop_id, affiliator_shop_id, dateRange):
    // Get Affiliator shop info
    shop = DB.getShop(affiliator_shop_id)

    // Get products and their performance
    products = DB.query(`
        SELECT
            product_id,
            product_title,
            product_image,
            COUNT(*) as sales_count,
            SUM(commission_amount) as commission_earned
        FROM affiliate_orders
        WHERE coseller_shop_id = {coseller_shop_id}
        AND affiliator_shop_id = {affiliator_shop_id}
        AND created_at BETWEEN {dateRange.from} AND {dateRange.to}
        GROUP BY product_id, product_title, product_image
        ORDER BY commission_earned DESC
    `)

    // Calculate totals
    totals = {
        salesCount: products.sum(p => p.sales_count),
        commissionEarned: products.sum(p => p.commission_earned)
    }

    RETURN {
        shop: shop,
        products: products,
        totals: totals
    }

```

---

### **FL-5: Date Range Handling**

```
defaultDateRange = {
    from: NOW - 30 days,
    to: NOW
}

ON DateRangeChange(newRange):
    IF newRange.from > newRange.to:
        SHOW error "Start date must be before end date"
        RETURN

    IF newRange.to > NOW:
        newRange.to = NOW

    SET currentDateRange = newRange
    REFRESH dashboardData(currentDateRange)

```

---

### B4) Dashboard Metrics Definitions

**Affiliator Metrics:**

| Metric | Calculation | Description |
| --- | --- | --- |
| Total Sales | COUNT(orders) | Number of orders in period |
| Total Revenue | SUM(order.total_price) | Gross revenue from all sales |
| Net Profit | SUM(order.affiliator_amount) | Revenue after commission |
| Commission Paid | SUM(order.commission_amount) | Total paid to Co-sellers |

**Co-seller Metrics:**

| Metric | Calculation | Description |
| --- | --- | --- |
| Total Orders | COUNT(orders) | Number of orders in period |
| Products Imported | COUNT(imported_products) | All-time count |
| Net Commission | SUM(order.commission_amount) | Total commission earned |

---

### B5) Behavioral Flow

```
[Open Affiliate Dashboard]
    ↓
[Check Role]
    ├─ Affiliator → [Load Affiliator Dashboard]
    │                   ↓
    │               [Show Summary Cards]
    │               [Show Co-seller List]
    │                   ↓
    │               [Click Co-seller]
    │                   ↓
    │               [Show Product Breakdown]
    │
    └─ Co-seller → [Load Co-seller Dashboard]
                       ↓
                   [Show Summary Cards]
                   [Show Shop List]
                       ↓
                   [Click Shop]
                       ↓
                   [Show Product Breakdown]

[Change Date Range] → [Refresh All Metrics]

```

---

### B6) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** No data in selected period | Show zeros, display "No data for this period" |
| **EC-2:** New Affiliator with no Co-sellers yet | Show empty state message |
| **EC-3:** New Co-seller with no sales yet | Show zeros, list imported products |
| **EC-4:** Very large date range | May be slower, consider pagination |
| **EC-5:** Future date selected | Limit to current date |
| **EC-6:** Co-seller/Shop no longer exists | Show historical data with "(Removed)" label |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Affiliator loads dashboard | Summary + Co-seller list shown |
| TC-2 | Co-seller loads dashboard | Summary + Shop list shown |
| TC-3 | Change date range | All metrics update |
| TC-4 | Click Co-seller row | Product breakdown shown |
| TC-5 | Click shop row | Product breakdown shown |
| TC-6 | No sales in period | Shows zeros, empty state message |
| TC-7 | Verify Net Profit calculation | Total Revenue - Commission Paid |
| TC-8 | Verify products imported count | Matches actual imports |
| TC-9 | Date range validation | Cannot select future dates |
| TC-10 | Role display | Correct role shown |
# Analytics Page

# **Feature ID:** IAA-RPT-102

# **Category:** Report / Dashboard | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a visual and numerical overview of their shop’s performance over selectable time periods. Analytics help merchants understand sales trends, customer activity, and product performance to make data-driven decisions.

### **Desired Outcome**

- Display a sales chart with configurable date ranges.
- Show key performance metrics below the chart.
- Highlight best-selling products.
- Display placeholders or empty states for sections without data.

---

# **2) Scope – In / Out**

### **In Scope**

- **Sales Chart:**
    - X-axis: time (day/week/month based on selected range)
    - Y-axis: total confirmed order sales
    - Date picker to select custom time range
- **Metrics Below Chart:**
    - Net Profit: sum of confirmed order profits in selected range
    - Customers: total number of unique customers in selected range
    - Orders: total confirmed orders in selected range
    - Visitors: total shop visitors in selected range
    - Total Inventory Value: inactive, display as “—”
    - Number of Products: total products in the system
- **Product Sections:**
    - Best Selling Products: top 5 products by quantity sold in selected range
    - Most Imported Products: display empty state if no data

### **Out of Scope**

- Forecasting or predictive analytics
- Advanced filtering beyond date range
- Export of analytics data (handled separately)

---

# **3) Key User Journeys**

---

### **Journey 1 – View Analytics Overview**

As a **Merchant**, I can view sales and performance data over a selected time range.

**Step 1:** Merchant opens Analytics page.

**Step 2:** Merchant selects a date range using the date picker.

**Step 3:** Sales chart updates based on selected range.

**Step 4:** Metrics below chart update for the same range: Net Profit, Customers, Orders, Visitors, Total Inventory Value (inactive), Number of Products.

**Step 5:** Best Selling Products section shows top 5 products by sales quantity.

**Step 6:** Most Imported Products section shows empty state if no data exists.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Sales chart must display total confirmed order sales accurately along X (time) and Y (amount) axes.

**BAC 2:** Metrics must update correctly according to selected date range.

**BAC 3:** Total Inventory Value must remain inactive and display as “—”.

**BAC 4:** Best Selling Products must list top 5 products by quantity sold in the selected range.

**BAC 5:** Most Imported Products must display empty state if no imported product data exists.

**BAC 6:** Date picker must allow merchant to choose valid time ranges, and chart & metrics must reflect the selection dynamically.
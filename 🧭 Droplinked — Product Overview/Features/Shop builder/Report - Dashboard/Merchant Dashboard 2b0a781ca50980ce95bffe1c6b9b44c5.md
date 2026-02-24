# Merchant Dashboard

# **Feature ID:** IAA-RPT-101

# **Category:** Report / Dashboard | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a centralized view of their shop’s performance immediately after onboarding or login. The dashboard must handle both empty states (no products or orders) and populated states (existing products and orders), providing key metrics, quick actions, and links to community resources, help, and blog content.

### **Desired Outcome**

- Display a clear empty state with actionable buttons and links when no products or orders exist.
- Display a populated dashboard with key metrics (Revenue, Net Profit, Orders, Customers).
- Provide quick access buttons for creating products, collections, invoices, and discounts.
- Show a summary of recent orders.
- Display affiliate section (empty state).
- Present community social media links, recent blog posts, and Help Center links.

---

# **2) Scope – In / Out**

### **In Scope**

- **Empty State:**
    - Display 3 sections with 2 buttons and 2 links (per design).
    - Actionable buttons: “Create Product”, “Create Collection”.
    - Links functional to correct pages.
- **Populated Dashboard:**
    - Key metrics:
        - Total Revenue (sum of confirmed orders)
        - Net Profit (sum of confirmed order profit)
        - Orders (total count)
        - Customers (total count)
    - Buttons:
        - Create Product → Product creation page
        - Create Collection → Collection page
        - Create Invoice → Orders page
        - Create Discount → Settings page
    - **Order Summary:**
        - Show last 5 confirmed orders (design-specific layout)
        - Empty state if no orders exist
    - **Affiliate Section:** Empty state
    - **Community Links:** Telegram, Discord, Twitter, TikTok, YouTube, LinkedIn, Instagram
        - Open in a new tab
    - **Blog Section:** Display 4 most recent blog posts with clickable links
    - **Help Center Section:** Display 4 key help topics with links

### **Out of Scope**

- Analytics charts or graphs
- Filtering or sorting of orders
- Bulk actions on dashboard metrics
- Dynamic affiliate tracking

---

# **3) Key User Journeys**

---

### **Journey 1 – Empty State Dashboard**

As a **Merchant** with no products or orders, I can see an empty state and quick actions.

**Step 1:** Merchant logs in or completes onboarding.

**Step 2:** Dashboard detects zero products and orders.

**Step 3:** Dashboard displays empty state with:

- Buttons: “Create Product”, “Create Collection”
- Links: “Create Invoice”, “Create Discount”
    
    **Step 4:** Merchant clicks buttons or links → navigates to corresponding pages.
    

---

### **Journey 2 – Populated Dashboard**

As a **Merchant** with products or orders, I can see shop metrics and take quick actions.

**Step 1:** Merchant logs in.

**Step 2:** Dashboard displays key metrics: Total Revenue, Net Profit, Orders, Customers.

**Step 3:** Buttons: Create Product, Create Collection, Create Invoice, Create Discount → navigate to correct pages.

**Step 4:** Recent 5 orders displayed in Order Summary section.

**Step 5:** Affiliate section displays empty state.

**Step 6:** Community section shows social media links → open in new tab when clicked.

**Step 7:** Blog section displays 4 recent posts → clickable.

**Step 8:** Help Center displays 4 key topics → clickable.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Dashboard must correctly detect empty vs populated state.

**BAC 2:** Key metrics must calculate based on confirmed orders only.

**BAC 3:** Action buttons must navigate to the correct pages.

**BAC 4:** Recent orders section must display up to 5 latest orders or empty state if none exist.

**BAC 5:** Affiliate section must show empty state correctly.

**BAC 6:** Community social media links must open in new tabs.

**BAC 7:** Blog section must display 4 most recent posts with valid links.

**BAC 8:** Help Center section must display 4 key topics with valid links.
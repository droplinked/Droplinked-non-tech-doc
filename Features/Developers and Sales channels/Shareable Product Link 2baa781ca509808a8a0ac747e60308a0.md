# Shareable Product Link

# **Feature ID:** INV-SHARE-LINK-105

# **Category:** Inventory Management | Growth Tools | **Actors:** Merchant, Customer | **Channel:** Web

# **Status:** Defined

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants often need to promote a specific product directly on social media, chat apps, or email marketing without directing customers to the full storefront catalog. Currently, deep-linking to a specific product purchase flow requires a streamlined interface.
This feature enables merchants to quickly generate and copy a direct link for any product. For customers, it provides a focused landing page to view details, select variants, and proceed directly to checkout.

### **Desired Outcome**

- Allow merchants to access a unique shareable link via the Inventory Management dashboard.
- Provide a dedicated product landing page for customers accessed via this link.
- Display essential product info (Images, Title, Description, Variants, Quantity).
- Enable a direct "Buy Now" flow that redirects immediately to the checkout page.
- Handle "Out of Stock" scenarios gracefully.

---

# **2) Scope – In / Out**

### **In Scope**

- **Merchant Interface:**
    - "3-dot" menu action in the Inventory Management list.
    - Modal displaying the generated Product Link (e.g., `https://dev.droplinked.io/shopname/product-link/slug`).
    - "Copy Link" functionality.
- **Customer Interface (Product Landing Page):**
    - Display Product Title, Description, and Images.
    - Variant Selection (if applicable).
    - Quantity Selector.
    - **"Buy Now"** button (Active state).
    - **"Out of Stock"** button (Disabled state).
- **System Logic:**
    - Generating the unique URL based on shop and product slug.
    - Checking real-time inventory for the "Out of Stock" status.
    - Redirecting to the Checkout Page with the selected SKU and quantity payload.

### **Out of Scope**

- The Checkout page UI/UX (this feature ends upon redirection).
- "Add to Cart" functionality (this is a direct purchase flow).
- Editing product details from the landing page.
- Social media preview customization (OG Tags) – handled separately.

---

# **3) Key User Journeys**

---

### **Journey 1 – Merchant Shares a Product**

**Step 1:** Merchant logs into the **Droplinked Dashboard** and navigates to **Inventory Management**.
**Step 2:** Merchant finds the desired product and clicks the **3-dot menu** icon.
**Step 3:** A modal opens displaying the **Shareable Product Link**.
**Step 4:** Merchant copies the link and closes the modal.

---

### **Journey 2 – Customer Purchases a Product (In Stock)**

**Step 1:** Customer clicks the link shared by the merchant.
**Step 2:** Customer lands on the **Product Landing Page**.
**Step 3:** Customer views images, title, and description.
**Step 4:** Customer selects desired **Variants** (e.g., Size, Color).
**Step 5:** Customer sets the **Quantity**.
**Step 6:** Customer clicks **"Buy Now"**.
**Step 7:** System redirects Customer to the **Checkout Page** with the order details pre-loaded.

---

### **Journey 3 – Customer Views Out of Stock Product**

**Step 1:** Customer opens the link.
**Step 2:** System detects that the specific variant or product has **0 Quantity**.
**Step 3:** The primary button displays **"Out of Stock"** and is **disabled** (unclickable).
**Step 4:** Customer cannot proceed to checkout.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** The "3-dot" menu in Inventory must include a "Get Share Link" (or similar) option.
**BAC 2:** The modal must display the correct URL format: `domain/shopname/product-link/product-slug`.
**BAC 3:** The Product Landing Page must render the correct Product Title, Description, and Media Gallery.
**BAC 4:** Variant selectors must be available if the product has defined variants.
**BAC 5:** The "Buy Now" button must be active only if:
- Inventory > 0.
- All required variant options are selected.

**BAC 6:** Clicking "Buy Now" must redirect the user to the Checkout Domain (`checkout.droplinked.com` or equivalent) passing the selected SKU and Quantity.
**BAC 7:** If the selected variant's inventory is 0, the button must change to "Out of Stock" and be disabled.
**BAC 8:** The Quantity selector must not allow a number higher than the available stock.
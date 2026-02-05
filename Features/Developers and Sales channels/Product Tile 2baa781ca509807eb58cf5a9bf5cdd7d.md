# Product Tile

# **Feature ID:** IAA-PROD-203

# **Category:** Growth Tools | Sales Channels | **Actors:** Merchant, Customer, Developer | **Channel:** External Websites / Web Widgets

# **Status:** Defined

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants want to sell their products not just on their Droplinked storefront, but also on their personal blogs, partner websites, or landing pages. They need a simple, "headless" way to embed a specific product card (Tile) that includes real-time product data and a direct purchase link.
The **Product Tile** feature provides a plug-and-play web component (script + HTML tag) that renders a fully functional product card on any external website. It supports both redirection logic and an embedded modal checkout experience.

### **Desired Outcome**

- Allow merchants to generate embed code for any specific product from the Dashboard.
- Provide a standardized Web Component (`<droplinked-product>`) that renders: Image, Title, Description, Variants, Price, and Action Button.
- Support two purchase modes controlled by attributes: **Redirect** (Default) and **Modal**.
- Handle product states automatically: **In Stock**, **Out of Stock**, and **Not Found/Draft**.
- Ensure the checkout flow is triggered correctly based on the selected mode.

---

# **2) Scope – In / Out**

### **In Scope**

- **Merchant Dashboard:**
    - UI in "Products" list: 3-dot menu > Share > "Product Tile" tab.
    - Displaying the required Script tag: `<script defer src="https://apiv3.droplinked.com/widget/bundle"></script>`.
    - Displaying the Component tag with the specific `product-id`.
- **Widget Rendering (The Tile):**
    - Default Product Image.
    - Product Title.
    - Short Description.
    - Available Variants (Dropdown/Selector).
    - Price (Dynamic based on variant).
    - "Buy Now" Button.
- **Logic & States:**
    - **Active:** Product is published and has stock.
    - **Out of Stock:** If Quantity = 0, button text changes to "Out of Stock" and is disabled.
    - **Not Found:** If Product ID is invalid or Product Status is "Draft", render a "Product Not Found" state.
- **Purchase Modes:**
    - **Redirect (Default):** `<droplinked-product product-id="...">` → Redirects user to Droplinked Checkout page.
    - **Modal:** `<droplinked-product ... purchase-mode="modal">` → Opens Checkout process in an overlay on the same page.

### **Out of Scope**

- The internal logic of the Checkout process (Refer to **Checkout Feature Documentation**).
- Advanced styling customization (CSS injection) for the merchant (Standard UI only for V1).
- Embedding entire collections (Single product only).

---

# **3) Key User Journeys**

---

### **Journey 1 – Merchant Generates Embed Code**

**Step 1:** Merchant navigates to **Products** list.
**Step 2:** Clicks the **3-dot menu** on a specific product and selects **Share**.
**Step 3:** Selects the **Product Tile** tab.
**Step 4:** System displays the Script tag and the Component tag.
**Step 5:** Merchant copies the code to their clipboard.
**Step 6:** Merchant pastes the script and tag into their external website's HTML.

---

### **Journey 2 – Customer Views Product Tile (External Site)**

**Step 1:** Customer visits the 3rd party website where the tile is embedded.
**Step 2:** The script loads and fetches product data via API.
**Step 3:** The component renders the Product Image, Title, Price, and Variant options.
**Step 4:** Customer selects a Variant (e.g., Size: Large).
**Step 5:** Price updates if necessary.

---

### **Journey 3 – Purchase via Redirect (Default)**

**Pre-condition:** Tag is set to `<droplinked-product product-id="...">` (No mode specified).
**Step 1:** Customer clicks **"Buy Now"**.
**Step 2:** Browser opens the Droplinked Checkout page in a new tab/window.
**Step 3:** The cart is pre-loaded with the specific product and variant.

---

### **Journey 4 – Purchase via Modal**

**Pre-condition:** Tag is set to `purchase-mode="modal"`.
**Step 1:** Customer clicks **"Buy Now"**.
**Step 2:** An overlay (Modal) opens directly on the current page.
**Step 3:** The Checkout flow loads inside the modal.

---

### **Journey 5 – Error State (Draft/Deleted)**

**Step 1:** Merchant unpublishes (Drafts) a product that was previously embedded.
**Step 2:** Customer visits the external site.
**Step 3:** API returns a 404 or status check failure.
**Step 4:** The Tile displays a "Product Not Available" or "Not Found" UI instead of the product details.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** The widget must require the script `https://apiv3.droplinked.com/widget/bundle` to function.
**BAC 2:** The component `<droplinked-product>` must accurately render the specific product details (Image, Title, Desc, Price) based on the `product-id`.
**BAC 3:** **Variant Handling:** The widget must allow users to select variants. If a variant is selected, the Price must reflect that specific SKU's price.
**BAC 4:** **Stock Check:** If the selected variant has 0 Quantity, the "Buy Now" button must be disabled and show "**Out of Stock**".
**BAC 5:** **Mode Logic:**
- If `purchase-mode` attribute is missing, clicking the button must **Redirect** to the checkout URL.
- If `purchase-mode="modal"`, clicking the button must open the checkout **Modal**.
**BAC 6:** **Validation:** If the Product ID is invalid or the Product Status is **Draft**, the widget must render a "Not Found" state, not a broken UI.
**BAC 7:** The Checkout logic invoked by the button must align with the standard Checkout documentation (Product details passed correctly).
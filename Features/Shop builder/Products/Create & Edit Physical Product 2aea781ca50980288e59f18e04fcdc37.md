# Create & Edit Physical Product

# Create & Edit Physical Product

## 

### Feature ID:

**[IAA-PROD-201]**

### Title:

**Create & Edit Physical Product**

### Category:

**Product Management** | **Actors**: Merchant | **Channel**: Web & API

### Status:

**Released**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**
Merchants need a flexible and powerful interface to create and manage physical products via both the Request Dashboard and API. The system must support diverse selling strategies, including pre-orders (Scheduled Release), private catalog items (Unlisted), and simplified product listings without mandatory images or descriptions. Ensuring accurate inventory management, variant handling, and shipping configurations is critical for order fulfillment.

**Desired Outcome:**

- Enable product creation via Dashboard and API.
- Allow "Draft" creation with only a Title.
- Support "Scheduled Release" with date selection and "Unlisted" status.
- Allow optional Images (with default placeholder) and Designation of a Primary Image.
- Make Description and Price optional (validating sellability based on price).
- Support "Continue selling when out of stock" vs. finite inventory logic.
- Provide AI tools: Generate Title/Description from Images, Generate Image from Title.
- Support Simple products (no variants) and Complex products (up to 2 variant tiers) with automatic SKU matrix generation.
- Allow optional Shipping configuration (treating non-shipping physical products like digital in checkout).
- Manage Advanced Settings like Keywords.

---

### 2) **Scope – In / Out**

**In:**

- **Channels:** Dashboard & API support.
- **Product Fields:**
    - Title (Mandatory).
    - Collection (Default selected in Dashboard, Optional/Default in API).
    - Images (0 to unlimited; Primary Image selection).
    - Description (Optional).
    - Price (Optional; >0 to be sellable).
    - External ID (Optional).
    - Stock Configuration (Finite vs. Unlimited/Continue Selling).
- **Variants:**
    - 0 Variants (Simple) or Variants enabled.
    - Max 2 Variant Groups (e.g., Color, Size).
    - Automatic Color Picker for "Color" type variants.
    - Automatic SKU Matrix Generation (e.g., 5 colors * 5 sizes = 25 SKUs).
    - Group-level or Individual SKU editing (Price, Stock, External ID, Image).
- **Shipping:**
    - Optional toggle.
    - If enabled: Weight, Dimensions (L/W/H), Units.
    - If disabled: Digital-like checkout behavior.
- **AI Features:**
    - Generate Title & Description from Images (Tone selection).
    - Generate Image from Title.
- **Advanced:** Keywords (Optional).
- **Status:** Public, Draft, Unlisted, Scheduled Release.

**Out:**

- Digital Product specific features (File uploads).
- POD specific features (Printful integration, Designer).
- Multi-language descriptions (unless specified globally).

---

### 3) **Key User Journeys**

**Journey 1: Quick Draft Creation**

- **Step 1:** Merchant initiates "Create Product".
- **Step 2:** Selects "Physical Product".
- **Step 3:** Enters **Title** (The only mandatory field).
- **Step 4:** System defaults Collection (Dashboard) or allows API default.
- **Step 5:** Merchant saves as **Draft**.
- **Step 6:** Product created successfully.

**Journey 2: Full Product with AI & Variants**

- **Step 1:** Merchant creates Physical Product.
- **Step 2:** Uploads images.
- **Step 3:** Uses **"Generate Title & Description from Images"**:
    - Selects Tone.
    - System generates content.
- **Step 4:** Defines Variants (e.g., Color, Size).
- **Step 5:** System generates SKU Matrix.
- **Step 6:** Merchant customizes specific SKUs (e.g., different image for 'Red', different price for 'XL').
- **Step 7:** Configures Stock: "Continue selling when out of stock" (Unlimited).
- **Step 8:** Sets Status to **Scheduled Release** and picks a future date.
- **Step 9:** Saves/Publishes.

**Journey 3: Product without Shipping (Local/Service-like)**

- **Step 1:** Merchant creates/edits Physical Product.
- **Step 2:** Skips Shipping Tab (or toggles off).
- **Step 3:** Sets Price and Stock.
- **Step 4:** Publishes.
- **Step 5:** Checkout flow will not request shipping address or method for this item.

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** Product creation must be possible with **only** a Title.
- **BAC 2:** **Price Logic:** Field is optional. If Price is null or <= 0, product cannot be added to cart/sold.
- **BAC 3:** **Image Logic:** 0 images allowed (shows default placeholder). 1 Image designated as Primary (Thumbnail).
- **BAC 4:** **Stock Logic:**
    - If "Continue selling..." = ON: Stock is infinite.
    - If "Continue selling..." = OFF: Non-negative integer required. Sales block at 0.
- **BAC 5:** **Variant Logic:** Max 2 groups. SKU Matrix auto-generated. SKU-specific images key off the variant definition.
- **BAC 5.1:** **Simple vs Variable Field Visibility:**
    - **Simple Product (No Variants):** Price, Quantity, External ID fields shown at product level (General/Advanced tabs).
    - **Variable Product (Has Variants):** Price, Quantity, External ID fields HIDDEN at product level. These fields are managed per SKU in the Variants tab SKU Matrix.
- **BAC 6:** **Shipping Logic:** Optional. If configured, requires W/L/H/Weight + Units. If not, bypasses shipping steps in checkout.
- **BAC 7:** **Status Logic:**
    - Public: Sellable immediately.
    - Unlisted: Not in shop lists, accessible via direct link/API? (Clarify behavior: "Not shown in shop").
    - Draft: Not sellable.
    - Scheduled Release: Not sellable until Date >= Current Date. Automatically flips to Public.
- **BAC 8:** **AI Features:**
    - Tone selection required for Text gen.
    - Title required for Image gen.

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Added reference to Shared Base [IAA-PROD-100], detailed SKU Group Editing | Consolidation |
| 2026-01-31 | Copywriter | Initial Update based on [product-changes.md](http://product-changes.md/) | Feature Update |

---

---

# [PART 2: AI REFERENCE SPEC]

### B1) Definitions & Glossary

- **Simple Product:** A physical product with no defined variant groups. Has a single Price, Stock, and SKU.
- **Variable Product:** A physical product with 1 or 2 variant groups (e.g., Color, Size). Inventory is managed at the generated SKU level.
- **Primary Image:** The specific image asset used as the thumbnail in catalogs/cart.
- **Scheduled Release:** A status where the product becomes 'Public' automatically at a specific timestamp.
- **Unlisted:** Status where product doesn't appear in PLP (Product Listing Page) but might be accessible (Hidden).
- **Continue Selling:** A boolean flag. If True, Quantity check is bypassed (Infinite Stock).

### B2) Detailed Functional Rules (Numbered)

- **R1.** **Creation Minimum:** A Product record can be created receiving only a `Title`. Default status is `Draft`.
- **R2.** **Collection Assignment:**
    - Dashboard: Pre-selects default collection. User can change.
    - API: Optional. If missing, assigns to Default Collection.
- **R3.** **Image Handling:**
    - Min: 0. Max: Unlimited.
    - Apps/Frontend must display a placeholder if Image Count == 0.
    - User must specify one `is_primary` image if count > 0.
- **R4.** **Price & Sellability:**
    - `Price` is optional in DB.
    - **Condition:** If `Price` is Null OR `Price` <= 0 -> Product `is_purchasable` = False. (Cannot add to cart).
- **R5.** **Stock Management:**
    - Field `continue_selling` (Boolean).
    - If `continue_selling` == True: Display "Unlimited", validation bypasses Quantity check.
    - If `continue_selling` == False: `Quantity` field is Mandatory (Integer >= 0).
    - **Logic:** If `Quantity` == 0 AND `continue_selling` == False -> `is_purchasable` = False.
- **R6.** **AI Text Generation:**
    - Input: Selected Image(s) + Tone Selection (Enum: Professional, Fun, etc.).
    - Output: Populates `Title` and `Description`.
- **R7.** **AI Image Generation:**
    - Input: `Title`.
    - Output: Appends generated image to Image Gallery.
- **R8.** **Variant Definition:**
    - Max Groups: 2.
    - Group Name (String) + Values (Array).
    - If Group Name ~ "Color" (fuzzy match), UI provides Color Picker.
- **R8.1.** **Simple vs Variable Field Visibility:**
    - **Rule:** When `variantGroups.count > 0`, the following fields are HIDDEN at product level:
        - `price` (General tab)
        - `quantity` (General tab)
        - `continue_selling` (General tab)
        - `external_id` (Advanced tab)
    - **Logic:** These fields move to SKU Matrix in Variants tab.
    - **On Variant Removal:** If all variants removed, fields reappear at product level.
- **R9.** **SKU Matrix:**
    - Cartesian product of Variant Values.
    - Example: 2 Colors * 3 Sizes = 6 SKUs.
    - Each SKU has its own: `price`, `quantity`, `external_id`, `image`.
    - **Note:** Product-level price/quantity/external_id are NULL when variants exist.
- **R10.** **Shipping Configuration:**
    - Toggle `has_shipping`.
    - If True: `Weight`, `Length`, `Width`, `Height`, `Unit` are Mandatory.
    - If False: Product is treated as virtual during Checkout (No address requirement).
- **R11.** **Status State Machine:**
    - `Public`: Visible & Purchasable (if R4 & R5 pass).
    - `Draft`: Not Visible, Not Purchasable.
    - `Unlisted`: Not Visible in Catalog, Purchasable (if R4 & R5 pass) via direct link.
    - `Scheduled`: Not Visible/Purchasable until `release_date`.
        - Cron/Trigger: When `now() >= release_date`, Status -> `Public`.
- **R12.** **Advanced:** `Keywords` list (Array of Strings) is optional.

### B3) UI/UX States & Triggers

- **Loading:**
    - When "Generate Title & Description" is clicked -> Spinner.
    - When "Generate Image" is clicked -> Skeleton/Spinner in gallery.
- **Empty:**
    - Image Gallery: Shows "Add Image or Generate" placeholder.
- **Error:**
    - Stock Input: If `continue_selling` is off and user inputs negative number -> Block save, show "Value must be non-negative".
    - Scheduled Date: If user picks past date -> "Date must be in future".

### B4) Edge Cases & Error Handling

- **EC1:** User adds variants then removes them.
    - System must warn: "This will delete all SKU inventory data."
    - After removal: Price/Quantity/External ID fields reappear at product level.
- **EC1.1:** User had Price=$50 at product level, then adds variants.
    - $50 is NOT copied to SKUs. Merchant must set SKU prices manually.
- **EC2:** API sends product with no collection.
    - System assigns default collection silently.
- **EC3:** Scheduled Release date arrives.
    - System updates status. If Product fails validation (e.g. Price 0), does it publish?
    - **Rule:** Validation for Sellability checks at creation/edit time? OR at purchase time?
    - **Clarification:** Product can be Public but not Purchasable. It publishes, but Buy button is disabled.

### B5) Data Requirements & API Contracts

- **Fields:**
    - `title`: String (Req)
    - `description`: String (Opt)
    - `price`: Float (Opt)
    - `images`: Array[Obj] {url, is_primary}
    - `inventory`: { quantity: Int, continue_selling: Bool }
    - `shipping`: { enabled: Bool, weight: Float, dimensions: {x,y,z}, unit: Enum }
    - `variants`: Array[Group]
    - `skus`: Array[SKU] { id, variant_ids, price, quantity, image_id }
    - `status`: Enum [PUBLIC, DRAFT, UNLISTED, SCHEDULED]
    - `scheduled_at`: DateTime (Nullable)
    - `keywords`: Array[String] (Opt)

### B6) Logical Flow & Pseudo-code

```
IF Create_Request:
    Validate Title is Present
    IF NO Title -> Throw Error
    Default Collection IF missing
    Set Status = DRAFT (default) OR provided status
    Save to DB
    RETURN Success

IF Update_Product (Publishing):
    IF Status -> PUBLIC:
        (Note: No strict mandatory field check prevents status change,
         but lack of Price/Stock prevents purchase)
        Update DB

```

### B7) Testing Matrix for AI

- **T1:** Create Product (Title Only) -> Expect Success (Draft).
- **T2:** Create Product (No Collection sent) -> Expect Success (Default Collection assigned).
- **T3:** Set Price = 0 -> Expect Product displayed but "Add to Cart" disabled/hidden.
- **T4:** Set Stock = 0, Continue Selling = False -> Expect "Out of Stock".
- **T5:** Set Stock = 0, Continue Selling = True -> Expect "In Stock".
- **T6:** Upload Images, Select Primary -> Expect Primary marked in DB.
- **T7:** Enable Shipping, Empty Dimensions -> Expect Validation Error.
- **T8:** Generate Description (AI) -> Expect Text field populated.
- **T9:** Create Simple Product (no variants) -> Expect Price/Quantity/External ID visible in General/Advanced tabs.
- **T10:** Add Variant Group to product -> Expect Price/Quantity/External ID fields HIDDEN, SKU Matrix appears.
- **T11:** Remove all Variant Groups -> Expect SKU Matrix hidden, Price/Quantity/External ID fields reappear.
- **T12:** Variable Product: Set different prices per SKU -> Expect each SKU has independent price.
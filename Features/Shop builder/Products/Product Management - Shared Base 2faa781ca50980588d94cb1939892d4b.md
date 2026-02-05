# Product Management - Shared Base

# Product Management - Shared Base

### Feature ID:

**[IAA-PROD-100]**

### Title:

**Product Management - Shared Base Features**

### Category:

**Product Management** | **Actors**: Merchant | **Channel**: Web & API

### Status:

**Released**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
All product types (Physical, Digital, POD) share a common set of features and behaviors. This document defines the shared foundation that all product types inherit, ensuring consistency across the platform and reducing duplication in specifications.

**Desired Outcome:**

- Define common fields, behaviors, and rules that apply to ALL product types.
- Serve as the "base class" that other product documents inherit from.
- Reduce redundancy and ensure consistency in product management.

---

### 2) **Scope – In / Out**

**In Scope (Shared Features):**

- **Creation Channels:** Dashboard & API (except POD which is Web-only)
- **Core Fields:**
    - Title (Mandatory)
    - Description (Optional)
    - Images (Optional, 0 to unlimited)
    - Price (Optional)
    - Collection (Default assigned)
    - External ID (Optional)
    - Keywords (Optional, in Advanced)
- **Status Management:** Public, Draft, Unlisted, Scheduled Release
- **Stock Management:** Finite vs Unlimited ("Continue selling when out of stock")
- **Variant System:** Up to 2 variant groups, SKU Matrix generation
- **AI Features:** Generate Title/Desc from Images, Generate Image from Title

**Out of Scope:**

- Product-type specific features (Shipping, Files, POD Designer)
- Order fulfillment logic

---

### 3) **Inheritance Map**

| Feature | Physical | Digital | POD |
| --- | --- | --- | --- |
| **Title** (Mandatory) | ✅ | ✅ | ✅ |
| **Description** (Optional) | ✅ | ✅ | ✅ (Auto-generated) |
| **Images** (Optional) | ✅ | ✅ | ✅ (Auto-generated) |
| **Price** (Optional) | ✅ | ✅ | ✅ (Must >= Cost) |
| **Collection** | ✅ | ✅ | ✅ |
| **External ID** | ✅ | ✅ | ❌ (Locked) |
| **Keywords** | ✅ | ✅ | ✅ |
| **Status** | ✅ | ✅ | ✅ |
| **Stock** | ✅ | ✅ | ❌ (Managed by Printful) |
| **Variants/SKU** | ✅ | ✅ | ✅ (Auto from Designer) |
| **AI Text Gen** | ✅ | ✅ | ❌ |
| **AI Image Gen** | ✅ | ✅ | ❌ |
| **Shipping Tab** | ✅ (Optional) | ❌ | ❌ |
| **Files Tab** | ❌ | ✅ | ❌ |
| **API Creation** | ✅ | ✅ | ❌ |

---

### 3.1) **Product Creation Tab Structure**

| Product Type | Tab 1 | Tab 2 | Tab 3 | Tab 4 |
| --- | --- | --- | --- | --- |
| **Physical** | General | Variants | Shipping | Advanced |
| **Digital** | General | Variants | Files | Advanced |
| **POD** | General | - | - | Advanced |

**Tab Contents:**

| Tab | Contains |
| --- | --- |
| **General** | Title, Description, Images, Price*, Collection, Stock*, Status |
| **Variants** | Variant Groups, SKU Matrix (Physical & Digital only) |
| **Shipping** | Weight, Dimensions, Units (Physical only) |
| **Files** | File uploads (max 5MB), External links (Digital only) - No reorder |
| **Advanced** | Keywords, External ID* |

> \ Conditional Fields:* Price, Stock (Quantity), and External ID fields in General/Advanced tabs are hidden when Variants are defined. These fields move to the SKU Matrix in the Variants tab.
> 

---

### 3.2) **Simple vs Variable Product Field Visibility**

| Field | Simple Product (No Variants) | Variable Product (Has Variants) |
| --- | --- | --- |
| **Price** | ✅ Shown in General tab | ❌ Hidden - Set per SKU in Variants tab |
| **Quantity** | ✅ Shown in General tab | ❌ Hidden - Set per SKU in Variants tab |
| **External ID** | ✅ Shown in Advanced tab | ❌ Hidden - Set per SKU in Variants tab |
| **Continue Selling** | ✅ Shown in General tab | ❌ Hidden - Set per SKU in Variants tab |

**Visual Logic:**

```
┌─────────────────────────────────────────────────────────────┐
│                    PRODUCT CREATION                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Has Variants Defined?                                     │
│           │                                                 │
│     ┌─────┴─────┐                                           │
│     │           │                                           │
│    NO          YES                                          │
│     │           │                                           │
│     ▼           ▼                                           │
│ ┌─────────┐  ┌──────────────────────────────────────────┐   │
│ │ SIMPLE  │  │ VARIABLE PRODUCT                         │   │
│ │ PRODUCT │  │                                          │   │
│ ├─────────┤  ├──────────────────────────────────────────┤   │
│ │ General │  │ General Tab:                             │   │
│ │  - Price│  │  - Price field HIDDEN                    │   │
│ │  - Qty  │  │  - Quantity field HIDDEN                 │   │
│ │         │  │  - Continue Selling HIDDEN               │   │
│ ├─────────┤  ├──────────────────────────────────────────┤   │
│ │ Advanced│  │ Advanced Tab:                            │   │
│ │  - ExtID│  │  - External ID field HIDDEN              │   │
│ │         │  │                                          │   │
│ │         │  ├──────────────────────────────────────────┤   │
│ │         │  │ Variants Tab:                            │   │
│ │         │  │  - SKU Matrix with:                      │   │
│ │         │  │    • Price per SKU                       │   │
│ │         │  │    • Quantity per SKU                    │   │
│ │         │  │    • External ID per SKU                 │   │
│ │         │  │    • Image per SKU                       │   │
│ └─────────┘  └──────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘

```

---

### 4) **Business Acceptance Criteria (BAC) - Shared**

| ID | Criteria | Applies To |
| --- | --- | --- |
| BAC-B1 | Product can be created with only Title | All |
| BAC-B1.1 | Title maximum length is 150 characters | All |
| BAC-B1.2 | Description maximum length is 5000 characters | All |
| BAC-B2 | Product without Price (or Price ≤ 0) cannot be sold | All |
| BAC-B3 | Product without images shows default placeholder | All |
| BAC-B4 | One image can be designated as Primary (thumbnail) | All |
| BAC-B5 | Description is optional | All |
| BAC-B6 | Collection defaults to shop's default collection | All |
| BAC-B7 | Status can be: Public, Draft, Unlisted, Scheduled | All |
| BAC-B8 | Scheduled Release auto-changes to Public at set date | All |
| BAC-B9 | Keywords can be added in Advanced Settings | All |
| BAC-B10 | When Variants are defined, Price/Quantity/External ID fields are hidden at product level | Physical, Digital |
| BAC-B11 | When Variants are defined, Price/Quantity/External ID are managed per SKU in Variants tab | Physical, Digital |
| BAC-B12 | Products recorded on blockchain (Onchain Inventory) CANNOT be edited - read-only mode enforced | All |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Added BAC-B12: Recorded products cannot be edited | Onchain Inventory integration |
| 2026-02-01 | Behdad | Initial document creation | Consolidate shared features |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Simple Product** | A product with no variant groups. Single Price, Stock, SKU. |
| **Variable Product** | A product with 1-2 variant groups. Inventory managed at SKU level. |
| **Primary Image** | The image used as thumbnail in catalogs, cart, and listings. |
| **Scheduled Release** | Status where product becomes Public automatically at a specific timestamp. |
| **Unlisted** | Product not shown in catalog but accessible via direct link. |
| **Recorded Product** | A product that has been minted as NFT on blockchain via Onchain Inventory. Cannot be edited. |
| **Continue Selling** | Boolean flag. If true, stock is infinite. |

---

### B2) Detailed Functional Rules (Shared)

---

### **R-B1: Title (Mandatory)**

```
IF title IS NULL OR title.trim() == "":
    REJECT creation with error "Title is required"
ELSE IF title.length > 150:
    REJECT with error "Title cannot exceed 150 characters"
ELSE:
    ALLOW creation
    SET status = DRAFT (default)

```

---

### **R-B1.1: Description (Optional)**

```
IF description IS NOT NULL AND description.length > 5000:
    REJECT with error "Description cannot exceed 5000 characters"
ELSE:
    ALLOW save

```

| Constraint | Value |
| --- | --- |
| Max Length | 5000 characters |
| Format | Rich text allowed |
| Required | No |

---

### **R-B2: Collection Assignment**

| Channel | Behavior |
| --- | --- |
| Dashboard | Default collection pre-selected, user can change |
| API | If not provided, assign to shop's default collection |

---

### **R-B3: Image Handling**

| Scenario | Behavior |
| --- | --- |
| 0 images | Show default placeholder in storefront |
| 1+ images | Display in gallery |
| Primary not set | First image is primary by default |
| Primary set | Designated image used as thumbnail everywhere |
| Reorder images | ❌ Not supported - images display in upload order |

**Primary Image Logic:**

```
IF images.count > 0:
    IF no image has is_primary == true:
        SET images[0].is_primary = true
    DISPLAY primary image as thumbnail
ELSE:
    DISPLAY placeholder image

```

---

### **R-B4: Price & Sellability**

```
is_purchasable = (price IS NOT NULL) AND (price > 0)

IF NOT is_purchasable:
    HIDE "Add to Cart" button
    SHOW price as "Price not set" or $0.00

```

| Price Value | Result |
| --- | --- |
| null | Not purchasable |
| 0 | Not purchasable |
| < 0 | Not purchasable (validation error on save) |
| > 0 | Purchasable (if stock available) |

---

### **R-B5: Stock Management**

| Setting | Behavior |
| --- | --- |
| continue_selling = true | Infinite stock, always purchasable |
| continue_selling = false | Finite stock, quantity required |

**Continue Selling Toggle Behavior:**

| State | Quantity Input | Display | Purchasable |
| --- | --- | --- | --- |
| ON (Unlimited) | Disabled, shows ∞ symbol | "Unlimited" in checkout | ✅ Always |
| OFF (Finite) | Enabled, integer input | Shows actual number | ✅ If qty > 0 |

**UI State when Continue Selling = ON:**

```
┌─────────────────────────────────────────────┐
│ Stock Management                            │
├─────────────────────────────────────────────┤
│ [✓] Continue selling when out of stock      │
│                                             │
│ Quantity: ┌─────────────┐                   │
│           │     ∞       │ ← Input DISABLED  │
│           └─────────────┘                   │
│           "Unlimited stock"                 │
└─────────────────────────────────────────────┘

```

**UI State when Continue Selling = OFF:**

```
┌─────────────────────────────────────────────┐
│ Stock Management                            │
├─────────────────────────────────────────────┤
│ [ ] Continue selling when out of stock      │
│                                             │
│ Quantity: ┌─────────────┐                   │
│           │     100     │ ← Input ENABLED   │
│           └─────────────┘                   │
│           "100 items in stock"              │
└─────────────────────────────────────────────┘

```

**Applies To:**

| Product Type | Continue Selling Available | Default |
| --- | --- | --- |
| Physical | ✅ Yes | OFF (Finite) |
| Digital | ✅ Yes | OFF (Finite) |
| POD | ❌ No (Always Unlimited) | ON (Infinite) - Locked |

**For Variable Products (with Variants):**

- Continue Selling can be set at SKU Matrix level
- "All Variants" row: Toggle ON → All SKUs become unlimited
- Group row: Toggle ON → All SKUs in group become unlimited
- Individual SKU: Toggle ON → That SKU becomes unlimited

```
IF continue_selling == false:
    REQUIRE quantity >= 0
    IF quantity == 0:
        SET is_purchasable = false
        SHOW "Out of Stock"

```

---

### **R-B6: Status State Machine**

| Status | Visible in Shop | Purchasable | Notes |
| --- | --- | --- | --- |
| Public | ✅ | ✅ (if price/stock valid) | Normal selling state |
| Draft | ❌ | ❌ | Work in progress |
| Unlisted | ❌ (in listings) | ✅ (via direct link) | Hidden but accessible |
| Scheduled | ❌ | ❌ | Until release_date |

**Scheduled Release Logic:**

```
CRON JOB (every minute):
    FOR each product WHERE status == SCHEDULED:
        IF now() >= release_date:
            SET status = PUBLIC
            LOG "Product {id} released"

```

---

### **R-B7: AI Text Generation**

**Flow:**

1. User clicks "Generate Title & Description from Images"
2. User selects Tone from list (Professional, Fun, Casual, etc.)
3. System sends images to AI
4. AI returns generated Title and Description
5. Fields are populated (user can edit)

```
ON GenerateTextClick:
    IF images.count == 0:
        SHOW error "Please add at least one image"
        RETURN

    SHOW ToneSelector

ON ToneSelect(tone):
    SET loading = true
    response = AI.generateText(images, tone)
    SET title = response.title
    SET description = response.description
    SET loading = false

```

---

### **R-B8: AI Image Generation**

**Flow:**

1. User clicks "Generate Image from Title"
2. System sends Title to AI
3. AI returns generated image
4. Image is added to gallery

```
ON GenerateImageClick:
    IF title.trim() == "":
        SHOW error "Please enter a title first"
        RETURN

    SET loading = true
    image = AI.generateImage(title)
    APPEND image to images[]
    SET loading = false

```

---

### **R-B9: Variant System**

**Constraints:**

- Maximum 2 variant groups
- Each group has a name and array of values
- Color-type groups get automatic color picker
- **Applies to:** Physical and Digital products only (POD variants are auto-generated)

**Field Visibility Rule (Critical):**

```
ON VariantGroupAdded OR VariantGroupRemoved:
    IF variantGroups.count > 0:
        // VARIABLE PRODUCT MODE
        HIDE product.price field in General tab
        HIDE product.quantity field in General tab
        HIDE product.continue_selling field in General tab
        HIDE product.external_id field in Advanced tab
        SHOW SKU Matrix in Variants tab

        // Price/Quantity/External ID are now per-SKU
        SET product.price = NULL (ignored)
        SET product.quantity = NULL (ignored)
        SET product.external_id = NULL (ignored)
    ELSE:
        // SIMPLE PRODUCT MODE
        SHOW product.price field in General tab
        SHOW product.quantity field in General tab
        SHOW product.continue_selling field in General tab
        SHOW product.external_id field in Advanced tab
        HIDE SKU Matrix

```

**SKU Matrix Generation:**

```
IF variantGroups.count == 0:
    // Simple product - NO SKU Matrix
    // Price, Quantity, External ID at PRODUCT level
    product.price = user_input
    product.quantity = user_input
    product.external_id = user_input

ELSE IF variantGroups.count >= 1:
    // Variable product - SKU Matrix created
    // Price, Quantity, External ID at SKU level
    SKUs = generateCartesianProduct(variantGroups)
    FOR each SKU:
        SKU.price = default_or_user_input
        SKU.quantity = default_or_user_input
        SKU.external_id = default_or_user_input

```

**Example:**

- Group 1: Color = [Red, Blue, Green]
- Group 2: Size = [S, M, L, XL, XXL]
- Result: 3 × 5 = 15 SKUs (each with own Price, Quantity, External ID)

---

### **R-B10: SKU Matrix Editing**

> UI Reference: See attached SKU Matrix Table screenshot showing Group By dropdown and hierarchical editing.
> 

Merchants can edit SKU data in three hierarchical levels:

| Edit Level | Row | Description | Use Case |
| --- | --- | --- | --- |
| **Level 1: All Variants** | "All Variants" row | Set same value for ALL SKUs at once | "Set all prices to $29.99" |
| **Level 2: Group By** | Color/Size group row (e.g., "Green - 3 Variants") | Set same value for all SKUs in one variant group | "All Blue items are $34.99" |
| **Level 3: Individual** | Individual SKU row (e.g., "Small", "Medium") | Edit single SKU independently | "Blue XL is $39.99" |

**Editing Hierarchy:**

```
┌─ All Variants (Level 1) ─────────────────────────┐
│   Sets Price/Quantity for ALL SKUs               │
│                                                  │
│   ┌─ Group: Green (Level 2) ─────────────────┐  │
│   │   Sets Price/Quantity for all Green SKUs │  │
│   │   ├─ Small (Level 3) - Individual edit   │  │
│   │   ├─ Medium (Level 3) - Individual edit  │  │
│   │   └─ Large (Level 3) - Individual edit   │  │
│   └──────────────────────────────────────────┘  │
│                                                  │
│   ┌─ Group: Blue (Level 2) ──────────────────┐  │
│   │   Sets Price/Quantity for all Blue SKUs  │  │
│   │   ├─ Small (Level 3) - Individual edit   │  │
│   │   ├─ Medium (Level 3) - Individual edit  │  │
│   │   └─ Large (Level 3) - Individual edit   │  │
│   └──────────────────────────────────────────┘  │
└──────────────────────────────────────────────────┘

```

**Editable Fields per SKU:**

| Field | Editable | Notes |
| --- | --- | --- |
| Price | ✅ | Inherits from parent if not set |
| Stock/Quantity | ✅ | Per-SKU inventory |
| External ID | ✅ | For integration |
| Image | ✅ | Select from product images |

**Group Editing Flow:**

```
ON GroupBySelect(variantGroup):
    DISPLAY SKUs grouped by selected variant
    e.g., Group by Color:
        Red: [Red-S, Red-M, Red-L, Red-XL, Red-XXL]
        Blue: [Blue-S, Blue-M, ...]

ON GroupPriceChange(group, price):
    FOR each SKU in group:
        SET SKU.price = price

ON GroupStockChange(group, stock):
    FOR each SKU in group:
        SET SKU.quantity = stock

```

**UI Behavior (Based on Actual UI):**

```
SKU MATRIX TABLE:
┌──────────────────────────────────────────────────────────────┐
│ Group by   [Color ▼]                                         │
├──────────────────┬─────────────┬────────────────┬────────────┤
│ Variants         │ Price       │ Unit Quantity  │ Actions    │
├──────────────────┼─────────────┼────────────────┼────────────┤
│ 🔲 All Variants  │ $ 0.00      │ 0              │            │
├──────────────────┼─────────────┼────────────────┼────────────┤
│ 🖼 Green         │ $ 0.00      │ 0              │     ˅      │
│    3 Variants    │             │                │            │
├──────────────────┼─────────────┼────────────────┼────────────┤
│ 🖼 Blue          │ $ 0.00      │ 0              │     ˄      │
│    3 Variants    │             │                │ (expanded) │
│  ├─ 🖼 Small     │ $ 0.00      │ 0              │ [Edit ✎]   │
│  ├─ ⬜ Medium    │ $ 0.00      │ 0              │ [Edit ✎]   │
│  └─ 🖼 Large     │ $ 0.00      │ 0              │ [Edit ✎]   │
└──────────────────┴─────────────┴────────────────┴────────────┘

```

**UI Elements:**

| Element | Description |
| --- | --- |
| Group by dropdown | Select which variant group to use for grouping (e.g., Color, Size) |
| All Variants row | Top-level row to set values for ALL SKUs at once |
| Group row (e.g., Green) | Collapsed view showing variant group with count, expandable |
| Expand/Collapse (˅/˄) | Toggle to show/hide individual SKUs within a group |
| Individual SKU row | Single SKU with editable Price and Quantity |
| Edit button (✎) | Opens detailed edit modal for individual SKU |

---

### **R-B11: Color Picker for Color Variants**

```
ON VariantGroupNameChange(name):
    IF name.toLowerCase() MATCHES ["color", "colour", "رنگ"]:
        ENABLE colorPicker for values
        EACH value gets: { name: "Red", hex: "#FF0000" }
    ELSE:
        DISABLE colorPicker
        EACH value is string only

```

---

### B3) Edge Cases & Error Handling

| Edge Case | Behavior |
| --- | --- |
| **EC-B1:** User removes all variants after creating matrix | Warn: "This will delete all SKU data" |
| **EC-B2:** API creates product without collection | Assign default collection |
| **EC-B3:** Scheduled date is in past | Validation error: "Date must be in future" |
| **EC-B4:** Product published with Price = 0 | Allowed, but "Add to Cart" disabled |
| **EC-B5:** Stock = 0 with continue_selling = false | Show "Out of Stock" |
| **EC-B6:** Negative stock entered | Validation error: "Stock must be non-negative" |
| **EC-B7:** Primary image deleted | Next image becomes primary |
| **EC-B8:** All images deleted | Show placeholder |
| **EC-B9:** User attempts to reorder images | Feature not available - images remain in upload order |
| **EC-B10:** Title exceeds 150 characters | Validation error, save blocked |
| **EC-B11:** Description exceeds 5000 characters | Validation error, save blocked |
| **EC-B12:** User adds first variant to simple product | Product-level Price/Quantity/External ID fields HIDE, SKU Matrix appears |
| **EC-B13:** User removes all variants from variable product | SKU Matrix HIDES, Product-level Price/Quantity/External ID fields SHOW |
| **EC-B14:** User had Price=$50 at product level, then adds variants | $50 is NOT copied to SKUs - merchant must set SKU prices manually |
| **EC-B15:** Variable product has some SKUs without price set | Those SKUs are not purchasable, others may be |
| **EC-B16:** User toggles Continue Selling ON | Quantity input disabled, shows ∞ symbol |
| **EC-B17:** User toggles Continue Selling OFF | Quantity input enabled, user must enter value |
| **EC-B18:** Variable product: Set Continue Selling ON for all SKUs | All SKUs become unlimited via "All Variants" row |
| **EC-B19:** User attempts to edit a product recorded on blockchain | All edit controls disabled, read-only banner shown |
| **EC-B20:** User attempts to delete a recorded product | Soft delete allowed (product hidden but blockchain record remains) |

---

### **R-B12: Recorded Product Edit Lock**

Products that have been recorded on blockchain via **Onchain Inventory** cannot be edited. This ensures blockchain data integrity.

```
ON LoadProductEditPage(productId):
    product = API.getProduct(productId)

    IF product.is_recorded == true:
        // Product is locked for editing
        SHOW readOnlyBanner({
            type: "warning",
            icon: "🔒",
            message: "This product is recorded on blockchain and cannot be edited.",
            link: { text: "View in Onchain Inventory", url: "/onchain-inventory" }
        })

        // Disable all edit controls
        DISABLE all input fields
        HIDE save/update buttons
        SHOW view-only mode

    ELSE:
        RENDER normalEditMode(product)

```

**UI State for Recorded Product:**

```
┌─────────────────────────────────────────────────────────────────┐
│ ⚠️ 🔒 This product is recorded on blockchain and cannot be     │
│      edited. [View in Onchain Inventory →]                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Title:      [Gaming Headset Pro        ] ← DISABLED            │
│  Price:      [$99.99                    ] ← DISABLED            │
│  Stock:      [250                       ] ← DISABLED            │
│                                                                 │
│  [Save Changes] ← HIDDEN                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

```

**Fields Locked on Recorded Products:**

| Field | Editable | Notes |
| --- | --- | --- |
| Title | ❌ | Immutable after recording |
| Description | ❌ | Immutable after recording |
| Price | ❌ | Immutable after recording |
| Images | ❌ | Cannot add/remove/reorder |
| Variants | ❌ | Cannot modify variant structure |
| SKU Data | ❌ | Price/Quantity per SKU locked |
| Status | ❌ | Cannot change visibility |
| Collection | ❌ | Cannot reassign |
| Shipping | ❌ | If applicable, locked |
| Files | ❌ | If digital, locked |

**Allowed Actions on Recorded Products:**

| Action | Allowed | Notes |
| --- | --- | --- |
| View product details | ✅ | Read-only access |
| Soft delete | ✅ | Hides from shop, blockchain record persists |
| Duplicate | ✅ | Creates new unrecorded copy |
| View in Onchain Inventory | ✅ | Link to blockchain record |
| View on storefront | ✅ | Normal customer view |

---

### B4) Data Model (Shared Fields)

| Field | Type | Required | Default | Notes |
| --- | --- | --- | --- | --- |
| id | UUID | Auto | - | System generated |
| title | String | ✅ | - | Max 150 chars |
| description | String | ❌ | null | Rich text allowed, Max 5000 chars |
| images | Array[Image] | ❌ | [] | Each has url, is_primary |
| price | Decimal | ❌ | null | Must be > 0 to sell |
| collection_id | UUID | ❌ | default | Auto-assigned |
| external_id | String | ❌ | null | For integrations |
| status | Enum | ❌ | DRAFT | PUBLIC, DRAFT, UNLISTED, SCHEDULED |
| scheduled_at | DateTime | ❌ | null | Required if SCHEDULED |
| keywords | Array[String] | ❌ | [] | For search/SEO |
| inventory | Object | ❌ | - | See below |
| variants | Array[Group] | ❌ | [] | Max 2 groups |
| skus | Array[SKU] | Auto | - | Generated from variants |
| is_recorded | Boolean | ❌ | false | True if recorded on blockchain |
| recorded_at | DateTime | ❌ | null | When product was recorded |

**Inventory Object:**

```json
{
    "quantity": 100,
    "continue_selling": false
}

```

**Variant Group:**

```json
{
    "name": "Color",
    "values": [
        { "name": "Red", "hex": "#FF0000" },
        { "name": "Blue", "hex": "#0000FF" }
    ]
}

```

**SKU Object:**

```json
{
    "id": "sku-123",
    "variant_values": ["Red", "Large"],
    "price": 29.99,
    "quantity": 50,
    "external_id": "RED-L-001",
    "image_id": "img-456"
}

```

---

### B5) Test Case Hooks (Shared)

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-B1 | Create with Title only | Success, status = DRAFT |
| TC-B1.1 | Create with Title > 150 chars | Validation error: "Title cannot exceed 150 characters" |
| TC-B1.2 | Create with Description > 5000 chars | Validation error: "Description cannot exceed 5000 characters" |
| TC-B2 | Create without Title | Validation error |
| TC-B3 | Create via API without collection | Default collection assigned |
| TC-B4 | Set Price = 0 | Product visible, not purchasable |
| TC-B5 | Set Price = 25 | Product purchasable |
| TC-B6 | Upload 0 images | Placeholder shown in shop |
| TC-B7 | Upload 3 images, set #2 as primary | #2 used as thumbnail |
| TC-B8 | Stock = 0, continue_selling = false | "Out of Stock" |
| TC-B9 | Stock = 0, continue_selling = true | "In Stock" (unlimited) |
| TC-B10 | Create 2 variant groups (3×5) | 15 SKUs generated |
| TC-B11 | Edit SKU group price | All SKUs in group updated |
| TC-B12 | Set status = Scheduled, past date | Validation error |
| TC-B13 | Scheduled date arrives | Status → PUBLIC |
| TC-B14 | Generate Title from images | Title/Desc populated |
| TC-B15 | Generate Image from title | Image added to gallery |
| TC-B16 | Create simple product (no variants) | Price/Quantity/External ID shown in General/Advanced tabs |
| TC-B17 | Add variant group to product | Price/Quantity/External ID fields HIDE, SKU Matrix appears |
| TC-B18 | Remove all variant groups | SKU Matrix HIDES, Price/Quantity/External ID fields reappear |
| TC-B19 | Variable product: set price per SKU | Each SKU has independent price |
| TC-B20 | Variable product: verify product-level price field | Field is hidden/disabled |
| TC-B21 | Toggle Continue Selling ON | Quantity input disabled, shows ∞ |
| TC-B22 | Toggle Continue Selling OFF | Quantity input enabled |
| TC-B23 | Variable product: Set Continue Selling ON for all SKUs | All SKUs show ∞ in quantity |
| TC-B24 | Open edit page for recorded product | Read-only banner shown, all fields disabled |
| TC-B25 | Attempt to save changes on recorded product | Save button hidden/disabled |
| TC-B26 | Duplicate a recorded product | New unrecorded copy created successfully |
| TC-B27 | Soft delete a recorded product | Product hidden, blockchain record persists |
# Create & Edit Digital Product

# Create & Edit Digital Product

### Feature ID:

**[IAA-PROD-204]**

### Title:

**Create & Edit Digital Product**

### Category:

**Product Management** | **Actors**: Merchant | **Channel**: Web & API

### Status:

**Released**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**
Digital products require a unique fulfillment flow where the customer receives access to files or links immediately after purchase. While sharing the core creation architecture with physical products (inclusive of AI tools, variants, and flexible fields), they must bypass shipping logistics and instead enforce a strict digital delivery mechanism via file attachments or external links.

**Desired Outcome:**

- Enable Digital Product creation mirroring Physical Product flexibility (Title-only Drafts, AI generation).
- **Support Variants** for Digital Products (e.g., License Types: Personal/Commercial, Resolution: 4K/1080p).
- Provide a dedicated **"Files" tab** to manage downloadable assets or access links.
- Allow multiple files/links per product.
- Securely deliver these assets to the customer post-purchase.
- Remove all Shipping configuration tabs/requirements.
- Full parity with Physical features: Collection defaults, Keywords, Scheduled Release, Unlisted status.

---

### 2) **Scope – In / Out**

**In:**

- **Core Features (Shared with Physical):**
    - Dashboard & API creation.
    - Mandatory Title, Optional Description/Price/Images.
    - **Images:** Maximum 50 images per product.
    - AI Tools: Text & Image Generation.
    - Status: Public, Draft, Unlisted, Scheduled. Default: **Publish**.
    - Stock: Finite or Unlimited ("Continue selling...").
    - Advanced: Keywords.
    - Default Collection assignment.
    - **SKU Images:** User can set image per SKU in matrix.
- **Variants (New):**
    - Digital products CAN have variants (Max 2 groups).
    - Example: Resolution (High/Low), Format (PDF/EPUB).
    - Matrix generation.
- **Digital Specifics:**
    - **Files Tab:**
        - Upload File(s).
        - Add Link(s).
        - Multiple entries allowed.
        - Optional (Can accept 0 files/links, though functionality limited).
    - **Delivery:** Files/Links accessible to buyer after payment.
- **Exclusions:**
    - **Shipping Tab:** Completely removed. No Weight/Dimensions.

**Out:**

- Physical shipping/tracking logic.
- Print-on-Demand design tools.

---

### 3) **Key User Journeys**

**Journey 1: Selling a Digital Artwork (Simple)**

- **Step 1:** Create Digital Product.
- **Step 2:** Upload Image (Art preview).
- **Step 3:** **Files Tab**: Upload the high-res ZIP file.
- **Step 4:** Set Price.
- **Step 5:** Publish.

**Journey 2: Software License (Variants)**

- **Step 1:** Create Digital Product "Pro Tool".
- **Step 2:** Define Variants: "License Type" -> [Standard, Enterprise].
- **Step 3:** System generates SKUs.
- **Step 4:** Merchant sets Price: Standard ($10), Enterprise ($99).
- **Step 5:** **Files Tab**: Uploads installer. (Note: Currently files attach to Product level, or SKU level? Input implies "Files page... merchant can add file/link". Usually Digital files are product wide, or sometimes SKU specific. Input says "Exactly similar to physical... differences... Files tab". It doesn't explicitly limit files to SKU. Assuming Product Level for simplicity unless "Variant specific files" logic exists. *Self-correction*: If I sell "4k" vs "1080p", the file *must* be different. Physical product has SKU images. Digital might need SKU files. However, input says: "In this page merchant can add file... This file/link will be shown after purchase". It treats it as a general tab. I will assume Product Level attachments for now, but note ambiguity).
- **Step 6:** Publish.

**Journey 3: AI Generated E-Book**

- **Step 1:** Create Product.
- **Step 2:** Use **AI** to generate Title/Desc from a cover image.
- **Step 3:** Files Tab: Add link to Google Drive folder.
- **Step 4:** Set Status: **Scheduled Release** for launch day.

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** **Creation:** Requires only Title to save as Draft.
- **BAC 1.1:** **Default Status:** New products default to **Publish**.
- **BAC 2:** **Shipping:** Must NOT present Shipping tab/fields.
- **BAC 3:** **Files/Links:**
    - UI must allow uploading files OR adding URL links.
    - Support >1 file/link.
    - **Max file size:** 5 MB per file.
    - **Reorder files:** ❌ Not supported.
    - Delivery: visible to customer only after successful order.
- **BAC 3.1:** **Images:** Maximum 50 images per product.
- **BAC 4:** **Variants:** Must support Variant Groups and SKU Matrix (same logic as Physical).
- **BAC 4.1:** **Simple vs Variable Field Visibility:**
    - **Simple Product (No Variants):** Price, Quantity, External ID fields shown at product level (General/Advanced tabs).
    - **Variable Product (Has Variants):** Price, Quantity, External ID fields HIDDEN at product level. These fields are managed per SKU in the Variants tab SKU Matrix.
- **BAC 4.2:** **SKU Images:** User can set image per SKU in matrix.
- **BAC 5:** **Stock/Price:** Optional fields. Price > 0 to be sellable. Stock logic (Unlimited vs Finite) applies.
- **BAC 6:** **AI Integration:** Available for Title/Desc/Image generation.

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Added reference to Shared Base [IAA-PROD-100] | Consolidation |
| 2026-01-31 | Copywriter | Updated to match Physical parity + Variants + File Tab | Feature Update |

---

---

# [PART 2: AI REFERENCE SPEC]

### B1) Definitions & Glossary

- **Digital File:** A binary asset uploaded to storage, accessible via secure link.
- **External Link:** A URL provided by the merchant (e.g., Dropbox, Mega) for the customer.
- **Delivery:** The mechanism of revealing these Files/Links in the "Order Success" release or "My Orders" view.

### B2) Detailed Functional Rules (Numbered)

- **R1.** **Structure Parity:** Digital Product inherits ALL Rules R1-R9, R11-R12 from Physical Product (See [IAA-PROD-201]), EXCEPT `Shipping` and `Image Mandatory` logic (if any differed).
- **R2.** **Shipping Exclusion:** `has_shipping` is strictly FALSE. UI hides Shipping Tab.
- **R3.** **File Attachments:**
    - Type: `File` (Upload) OR `Link` (URL).
    - Multiplicity: 0 to Many.
    - **Max File Size:** 5 MB per file.
    - **Reorder:** ❌ Not supported - files display in upload order.
    - Visibility: Private (Merchant only) -> Purchased (Customer).
- **R4.** **Variant Logic:**
    - Same as Physical.
    - *Clarification on Files:* Currently, Files are associated at the `Product` level (Parent), accessible to any purchaser of any SKU, unless specific "SKU-Specific File" logic is introduced. (Input text: "Merchant can add file... these files... shown after purchase". No distinction per variant made).
- **R4.1.** **Simple vs Variable Field Visibility:**
    - **Rule:** When `variantGroups.count > 0`, the following fields are HIDDEN at product level:
        - `price` (General tab)
        - `quantity` (General tab)
        - `continue_selling` (General tab)
        - `external_id` (Advanced tab)
    - **Logic:** These fields move to SKU Matrix in Variants tab.
    - **On Variant Removal:** If all variants removed, fields reappear at product level.
    - Each SKU has its own: `price`, `quantity`, `external_id`, `image`.
- **R5.** **Inventory:**
    - Digital goods often use `continue_selling = True` (Unlimited).
    - However, Finite stock is allowed (e.g., Limited edition NFTs or Keys).

### B3) UI/UX States & Triggers

- **Files Tab:**
    - **Empty:** "Computed hash/upload area".
    - **Uploading:** Progress bar.
    - **List:** Shows Name, Type (File/Link), Size (if file), Action (Delete).
    - **Reorder Files:** ❌ Not supported - files display in upload order.
    - **File Size Limit:** Maximum 5 MB per file.

**Files Tab Constraints:**

| Constraint | Value | Behavior |
| --- | --- | --- |
| Max file size | 5 MB | Files > 5MB rejected with error |
| Reorder files | ❌ Not supported | Files display in upload order |
| File types | Any | All file types allowed |
| Link validation | URL format | Must be valid URL |

**File Upload Error States:**

```
IF file.size > 5MB:
    REJECT with error "File size cannot exceed 5 MB"
    SHOW: "Maximum file size is 5 MB. Please compress or split your file."
```

### B4) Edge Cases & Error Handling

- **EC1:** User allows "Unlimited Stock" but provides NO file/link.
    - Warning: "You are selling a digital product with no content." (Soft warning, not blocking save).
- **EC1.1:** User adds variants then removes them.
    - System must warn: "This will delete all SKU inventory data."
    - After removal: Price/Quantity/External ID fields reappear at product level.
- **EC1.2:** User had Price=$50 at product level, then adds variants.
    - $50 is NOT copied to SKUs. Merchant must set SKU prices manually.
- **EC2:** Variants defined (e.g., "Mobi" vs "PDF") but only one file uploaded.
    - Logic: Both variants get the same file list.
- **EC3:** File upload failure.
    - Retry logic/Error toast.
- **EC4:** File exceeds 5 MB limit.
    - Error: "File size cannot exceed 5 MB". Upload blocked.
- **EC5:** User attempts to reorder files.
    - Feature not available. Files remain in upload order.

### B5) Data Requirements & API Contracts

- **Inherits:** All from Physical.
- **Removes:** `shipping` object.
- **Adds:**
    - `digital_assets`: Array[Obj]
        - `type`: Enum [FILE, LINK]
        - `name`: String
        - `url`: String (Source URL)
        - `size`: String (Opt)

### B6) Logical Flow & Pseudo-code

```
IF Product_Type == DIGITAL:
    Hide Shipping_Tab
    Show Files_Tab

    ON Save:
        Validate Title
        Process File Uploads (Async if needed)
        Save Standard Fields (Price, Stock, Variants)
```

### B7) Testing Matrix for AI

- **T1:** Create Digital Product (Draft) -> Success.
- **T2:** Add Variants -> Verify Matrix Generation.
- **T3:** Access Shipping Tab -> Fail (Should not exist).
- **T4:** Upload 3 Files + 2 Links -> Verify persistence.
- **T5:** Checkout with Digital Item -> Verify No Shipping Address asked.
- **T6:** Post-Purchase -> Verify Access to Files in Order History.
- **T7:** Create Simple Product (no variants) -> Expect Price/Quantity/External ID visible in General/Advanced tabs.
- **T8:** Add Variant Group to product -> Expect Price/Quantity/External ID fields HIDDEN, SKU Matrix appears.
- **T9:** Remove all Variant Groups -> Expect SKU Matrix hidden, Price/Quantity/External ID fields reappear.
- **T10:** Variable Product: Set different prices per SKU -> Expect each SKU has independent price.
- **T11:** Upload file > 5 MB -> Expect error "File size cannot exceed 5 MB".
- **T12:** Upload file exactly 5 MB -> Expect success.
- **T13:** Attempt to reorder files -> Expect feature not available.
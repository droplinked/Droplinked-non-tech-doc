# Create & Edit POD Product

# Create & Edit POD Product

## 

### Feature ID:

**[IAA-PROD-301]**

### Title:

**Create & Edit POD (Print-On-Demand) Product**

### Category:

**Product Management** | **Actors**: Merchant | **Channel**: Web Only (Creation)

### Status:

**Released**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**
POD products rely on external fulfillment (Printful) and require a specialized creation flow that integrates a design tool. Unlike standard products, their core attributes (Images, Base Title, Base Description, Variant Matrix) are derived from the external provider. Merchants need a workflow that enforces the design step, locks cost-related fields to ensure profitability, and automatically manages the complexity of print combinations.

**Desired Outcome:**

- Enforce "Blank Product Selection" -> "Printful Designer" flow (Web Only).
- **Auto-Generate**: Images, Title, Description, and Variant Matrix from Printful data.
- **Image Loading State**: Handle async image generation from Printful.
- **Cost Protection**: Ensure Retail Price >= Printful Cost.
- **Inventory & Fulfillment**: Lock Inventory (Managed by Printful) and Dimensions. Disable Shipping configuration (Handled by Printful).
- **Non-Standard SKUs**: Handle Printful SKUs that are "Out of Stock" or inactive by creating them as inactive in the matrix.
- Align with standard status (Public/Draft/Scheduled) and Collection logic.

---

### 2) **Scope – In / Out**

**In:**

- **Creation Flow:**
    - Step 1: Blank Product Selector.
    - Step 2: Printful Designer (iframe/module).
    - Step 3: Metadata Sync (Images, Title, Desc, Variants).
- **Product Fields:**
    - **Title/Design:** Mandatory for creation.
    - **Images:** Auto-generated (Loading indicator required).
    - **Description:** Auto-generated (Editable).
    - **Collection:** Shared logic (Default/Selectable).
    - **Status:** Public, Draft, Unlisted, Scheduled.
- **Variant Matrix & Pricing:**
    - Matrix: Auto-built based on Printful options.
    - **Fields:**
        - `Cost` (Read-Only).
        - `Quantity` (Read-Only/Ignored - Unlimited).
        - `External ID` (Read-Only/Locked).
        - `Price` (Editable, Must be >= Cost).
- **Logic:**
    - If Printful SKU missing/inactive -> Matrix entry created but `state` = Inactive/OOS.
- **Exclusions:**
    - File Attachments (Digital).
    - Shipping Configuration (Merchant side).

**Out:**

- API Creation (POD products must be created via Web UI due to Designer requirement).
- Merchant editing of External IDs or variant structure (Locked to Printful).

---

### 3) **Key User Journeys**

**Journey 1: Design & Publish**

- **Step 1:** Merchant selects "Create POD Product".
- **Step 2:** Selects Base Item (e.g., T-Shirt).
- **Step 3:** Enters Designer, adds Logo, finishes design.
- **Step 4:** System redirects to Product Detail.
    - *UI:* Shows "Generating Images..." loaders.
    - *Data:* Matrix populated.
- **Step 5:** Merchant reviews **Price**.
    - System defaults Price to (Cost + Margin) or similar, or Merchant manually updates.
    - Validates Price >= Cost.
- **Step 6:** Assigns Collection.
- **Step 7:** Publishes.

**Journey 2: Handling Out of Stock Variants**

- **Step 1:** Merchant designs a Hoodie.
- **Step 2:** Printful reports "Red XL" is Temporarily Out of Stock.
- **Step 3:** System generates Matrix.
- **Step 4:** "Red XL" row is visible but toggled **Inactive** or shows "Supplier Out of Stock".
- **Step 5:** Merchant cannot enable sales for this specific SKU until Supplier restocks.

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** Creation requires interaction with Designer; cannot be done via simple API payload.
- **BAC 2:** **Auto-Sync:** Images, Title, Desc must populate from Printful.
- **BAC 3:** **Pricing Guardrails:** `Price` field validation: `Price >= Cost`.
- **BAC 4:** **Read-Only Fields:** `External ID`, `Cost`, `Quantity` are not editable by merchant.
- **BAC 5:** **Variant Integrity:** Matrix structure (Colors/Sizes) is locked to the Design. Merchant cannot manually add "Green" if not designed.
- **BAC 6:** **Image State:** UI must handle "Pending" state for images while Printful generates them.
- **BAC 7:** **Shipping/Files:** No Shipping Tab (Printful handles it). No Files Tab.

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Added reference to Shared Base [IAA-PROD-100] | Consolidation |
| 2026-01-31 | Copywriter | Updated for Printful Design Flow & Cost Logic | Feature Update |

---

---

# [PART 2: AI REFERENCE SPEC]

### B1) Definitions & Glossary

- **Blank Product:** The raw material (e.g., Gildan 5000) selected from Printful catalog.
- **Printful Cost:** The base charge from the provider.
- **Retail Price:** The amount the merchant charges the customer.

### B2) Detailed Functional Rules (Numbered)

- **R1.** **Creation Restriction:** Creating a POD product initiates the `Designer_Session`. The Product record is solidified only after `Designer_Callback` / Completion.
- **R2.** **Mandatory Fields (Creation):** Title, Design Configuration (from Provider).
- **R3.** **Image Generation:**
    - Images are async.
    - Status `image_generation_status`: `PENDING` -> `COMPLETED`.
    - FE must poll or listen for socket event to replace Loaders with Images.
- **R4.** **Variant Matrix Construction:**
    - Source: Printful Variants.
    - If Printful Variant `is_available` == False -> System creates SKU with `stock_status` = OUT_OF_STOCK / HIDDEN.
- **R5.** **Pricing Logic:**
    - `price`: Editable.
    - `cost`: Stored (Hidden or Read-Only).
    - Validation: `price` must be >= `cost`. Exception: Specific promo override? (Input says "Cannot be less"). Strict Rule: `price >= cost`.
- **R6.** **Locked Fields:**
    - `quantity` (Managed by Printful).
    - `external_id` (Mapped to Printful Variant ID).
    - `dimensions/weight` (Managed by Printful).
- **R7.** **Shipping & Files:**
    - `has_shipping` = False (technically True internally for Printful, but Hidden from Merchant UI).
    - `digital_files` = Empty/Disabled.
- **R8.** **Status Logic:**
    - Same as Standard (Public, Draft, Unlisted, Scheduled).
    - **Constraint:** Cannot Publish if `image_generation_status` != `COMPLETED`? (Preferably wait, but not strictly blocking if placeholders exist). *Strictness:* Usually allow save, but maybe warn on Publish.

### B3) UI/UX States & Triggers

- **Loading:**
    - "Syncing with Printful..." overlay after design.
    - Image placeholders with spinning loaders.
- **Error:**
    - Price Input: If Merchant enters $10 and Cost is $12 -> Show inline error "Price must be at least $12".
    - Save: Disable Save if validation fails.

### B4) Edge Cases & Error Handling

- **EC1:** Printful API Down during creation.
    - Error: "Cannot load designer".
- **EC2:** Printful changes Cost after product creation.
    - System should periodically sync? Or sync on Load? If Cost > Price -> Merchant needs alert. (Out of scope for this specific doc, but worth noting in logic).
- **EC3:** Merchant tries to delete a Variant row.
    - Action blocked. POD variants are "All or Nothing" based on the design. User must Edit Design to remove a color.

### B5) Data Requirements & API Contracts

- **Extends Product, but:**
    - `is_pod`: True.
    - `pod_provider`: 'PRINTFUL'.
    - `variants`: Locked structure.
    - `skus`:
        - `cost`: Decimal.
        - `details`: { provider_variant_id, sync_status }.

### B6) Logical Flow & Pseudo-code

```
IF Create_POD:
    Select Blank -> Open Designer
    On Design_Complete -> Receive Payload
    Generate SKUs from Payload
    Fetch Images (Async)
    Set Default Prices (Cost * Multiplier?)
    Set Title/Desc
    Save Draft

```

### B7) Testing Matrix for AI

- **T1:** Enter Price < Cost -> Expect ValidationError.
- **T2:** Check SKU Count -> Must match Designer selection (e.g. 5 sizes * 2 colors).
- **T3:** Modify Quantity -> Expect Field Disabled or Reverted.
- **T4:** Publish -> Verify visible in Shop.
- **T5:** Check Shipping Tab -> Expect Hidden.
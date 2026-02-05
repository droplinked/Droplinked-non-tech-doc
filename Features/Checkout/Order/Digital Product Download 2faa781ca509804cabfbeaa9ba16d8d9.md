# Digital Product Download

# Digital Product Download

### Feature ID:

**[CHK-DIG-001]**

### Title:

**Digital Product Download**

### Category:

**Checkout / Order** | **Actors**: Customer | **Channel**: Storefront (Order Confirmation Page)

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Customers who purchase digital products (e-books, software, design files, etc.) need immediate access to their purchased files after completing payment. Without a clear, secure, and reliable download mechanism, customers cannot access the value they paid for.

**Desired Outcome:**

- Customers see all downloadable files associated with their purchased digital products on the Order Confirmation Page.
- Each file is displayed individually with direct download capability.
- Downloads are available immediately after successful payment.
- Downloads have no time limit or download count restriction.
- Guest and logged-in customers can access downloads equally.
- Files remain accessible through Order History for future downloads.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Digital Goods Section:** Dedicated area on Order Confirmation Page for downloadable files.
- **File List Display:** All files from purchased digital products listed individually.
- **Direct Download Links:** Each file has its own download button/link.
- **Unlimited Downloads:** No restriction on download count or time.
- **Guest Access:** Files accessible to guest customers via order link.
- **Order History Access:** Files accessible via Order History for logged-in customers.
- **Multiple Files per Product:** Products with multiple files show each file separately.
- **File Metadata Display:** File name visible for each download.

**Out of Scope:**

- File upload/management (Handled in Product Creation).
- Streaming or preview functionality.
- DRM or copy protection.
- Download manager or batch download (ZIP).
- External link products (handled differently, user redirected).
- File versioning or updates.

---

### 3) **Key User Journeys**

---

**Journey 1: Download Digital Product After Purchase**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Completes purchase of digital product | Order confirmed, redirected to confirmation page |
| 2 | Customer | Views Order Confirmation Page | Digital Goods section displays with file list |
| 3 | Customer | Clicks download button for a file | Browser initiates file download |
| 4 | Customer | File downloads to device | Download completes successfully |

---

**Journey 2: Download Multiple Files from One Product**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Purchases digital product with 3 attached files | Order created with 3 downloadable items |
| 2 | Customer | Views Digital Goods section | 3 separate files listed individually |
| 3 | Customer | Clicks download on first file | First file downloads |
| 4 | Customer | Clicks download on second file | Second file downloads |
| 5 | Customer | Clicks download on third file | Third file downloads |

---

**Journey 3: Re-download from Order History**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Needs to re-download purchased file | Navigates to Order History |
| 2 | Customer | Finds original order | Clicks to view order details |
| 3 | Customer | Views Order Confirmation Page | Digital Goods section available |
| 4 | Customer | Clicks download | File downloads again |

---

**Journey 4: Guest Customer Download**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Guest Customer | Completes purchase without account | Order created, confirmation page shown |
| 2 | Guest Customer | Views Digital Goods section | Files available for download |
| 3 | Guest Customer | Downloads files | Files download successfully |
| 4 | Guest Customer | Later clicks link from email | Order page loads, files still available |

---

**Journey 5: Mixed Order (Physical + Digital)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Purchases physical product + digital product | Order contains both types |
| 2 | Customer | Views Order Confirmation Page | Both Delivery Details AND Digital Goods sections shown |
| 3 | Customer | Downloads digital files | Files download immediately |
| 4 | Customer | Waits for physical shipment | Physical product shipped separately |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | Digital Goods section appears on Order Confirmation Page when order contains digital products with files |  |
| BAC-2 | Each file attached to a digital product is displayed as a separate downloadable item |  |
| BAC-3 | File name is visible for each downloadable file |  |
| BAC-4 | Each file has its own download button/link |  |
| BAC-5 | Clicking download initiates direct file download |  |
| BAC-6 | No download count limit exists |  |
| BAC-7 | No time limit exists for downloads |  |
| BAC-8 | Guest customers can download files from order confirmation page |  |
| BAC-9 | Logged-in customers can re-download from Order History |  |
| BAC-10 | Products with multiple files show all files separately |  |
| BAC-11 | Download links are direct (not expiring or signed) |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature Definition |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Digital Product** | A product type that delivers value through downloadable files rather than physical shipment |
| **Digital File** | A binary file (PDF, ZIP, MP3, etc.) uploaded by merchant and attached to a digital product |
| **External Link** | A URL to external storage (Dropbox, Google Drive) - NOT handled by this feature |
| **Direct Download** | A permanent, non-expiring URL that initiates immediate file download |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Digital Goods Section Visibility Logic**

| Condition | Result |
| --- | --- |
| Order contains at least one digital product with `digital_files.length > 0` | Digital Goods section is displayed |
| Order contains digital products but all have 0 files | Digital Goods section is hidden |
| Order contains only physical products | Digital Goods section is hidden |
| Order contains only external link products | Different handling (redirect) |

---

### **FL-2: File List Aggregation Logic**

```
digital_goods_list = []

FOR each item in order.items:
    IF item.product.type == DIGITAL:
        FOR each file in item.product.digital_files:
            digital_goods_list.append({
                product_title: item.product.title,
                file_name: file.name,
                file_url: file.download_url,
                file_size: file.size
            })

RETURN digital_goods_list

```

---

### **FL-3: File Display Logic**

For each file in `digital_goods_list`:

| Element | Source | Display |
| --- | --- | --- |
| Product Title | item.product.title | Section header or grouping |
| File Name | [file.name](http://file.name/) | Primary text |
| Download Button | file.download_url | Clickable action |
| File Size (optional) | file.size | Secondary info (e.g., "15 MB") |

**Display Format:**

```
Digital Goods
─────────────────
📄 Product Title
   └─ [Download] filename.pdf
   └─ [Download] bonus-content.zip

📄 Another Product
   └─ [Download] ebook.epub

```

---

### **FL-4: Download URL Logic**

| URL Type | Behavior |
| --- | --- |
| Direct storage URL | Browser initiates download immediately |
| CDN URL | Same behavior, potentially faster |

**URL Characteristics:**

- Permanent (no expiration)
- Public (no authentication required)
- Direct (no redirect chain)

---

### **FL-5: Multiple Quantity Handling**

| Scenario | Behavior |
| --- | --- |
| Customer buys 1x Digital Product (3 files) | 3 files shown |
| Customer buys 3x Digital Product (3 files) | Still 3 files shown (same content, quantity irrelevant for downloads) |

**Logic:** File access is granted at product level, not per-unit. Purchasing multiple quantities does not multiply file access.

---

### **FL-6: Access Control Logic**

| User Type | Access Method | File Access |
| --- | --- | --- |
| Guest Customer | Direct URL from confirmation/email | Full access |
| Logged-in Customer | Order Confirmation Page | Full access |
| Logged-in Customer | Order History → Order Details | Full access |
| Random User (no order) | Attempts to access file URL | Depends on URL security model* |
- Note: Current implementation uses direct public URLs. Files are accessible to anyone with the URL. This is acceptable for current scope but may need security enhancement in future.

---

### B3) Behavioral Flow

```
[Order Confirmation Page Loads]
    ↓
[Check: Order has digital products?]
    ├─ NO → [Digital Goods Section Hidden]
    └─ YES ↓
[Check: Digital products have files attached?]
    ├─ NO → [Digital Goods Section Hidden]
    └─ YES ↓
[Aggregate all files from all digital products]
    ↓
[Render Digital Goods Section]
    ↓
[For Each File:]
    ↓
[Display: Product Title + File Name + Download Button]
    ↓
[User Clicks Download]
    ↓
[Browser Initiates Download]
    ↓
[File Saves to User's Device]

```

---

### B4) State Transitions

This feature is relatively stateless. Files are either:

| State | Description |
| --- | --- |
| Available | File exists and can be downloaded |
| Unavailable | File was deleted or storage error (Edge case) |

**No claim/pending states exist** - files are immediately and permanently available.

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Digital product purchased but merchant attached 0 files | No files shown for that product; if all products have 0 files, section hidden |
| **EC-2:** File deleted by merchant after purchase | Download fails; show "File no longer available. Contact support." |
| **EC-3:** Storage service temporarily unavailable | Download fails; show "Download temporarily unavailable. Please try again." |
| **EC-4:** Very large file (>1GB) | Normal download behavior; browser handles large file |
| **EC-5:** Customer's browser blocks download | Standard browser behavior; customer must allow downloads |
| **EC-6:** Customer on mobile device | Mobile browser handles download; may prompt to open/save |
| **EC-7:** Multiple products with same file name | Each file shown separately; no deduplication |
| **EC-8:** Product with both files AND external links | Files shown in Digital Goods; links handled separately |
| **EC-9:** Order with Pending payment status | Files should NOT be accessible until payment Success |
| **EC-10:** Order with Failed payment status | Files should NOT be accessible |

---

### **FL-7: Payment Status Gate**

| Order Status | File Access |
| --- | --- |
| Success | Files visible and downloadable |
| Pending | Files section hidden OR downloads disabled with message |
| Failed | Files section hidden OR downloads disabled with message |

---

### B6) Data Requirements

**Digital Product File Object:**

| Field | Type | Description |
| --- | --- | --- |
| file_id | String | Unique file identifier |
| product_id | String | Parent product identifier |
| name | String | Display name of file |
| original_filename | String | Original uploaded filename |
| download_url | String | Direct URL for download |
| size | Integer | File size in bytes |
| mime_type | String | File MIME type |
| uploaded_at | DateTime | Upload timestamp |

**Order Item Digital Access:**

| Field | Type | Description |
| --- | --- | --- |
| order_id | String | Parent order |
| order_item_id | String | Line item reference |
| product_id | String | Product purchased |
| files | Array[File] | List of accessible files |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Order with 1 digital product (1 file) | Digital Goods section shows, 1 file listed |
| TC-2 | Order with 1 digital product (5 files) | Digital Goods section shows, 5 files listed separately |
| TC-3 | Order with 3 digital products | All files from all products listed |
| TC-4 | Order with only physical products | Digital Goods section hidden |
| TC-5 | Order with digital product (0 files) | Digital Goods section hidden |
| TC-6 | Mixed order (physical + digital) | Both Delivery and Digital sections shown |
| TC-7 | Click download button | File download initiates |
| TC-8 | Download same file twice | Both downloads succeed |
| TC-9 | Guest customer downloads | Download succeeds |
| TC-10 | Re-download from Order History | Files still accessible |
| TC-11 | Order status = Pending | Downloads not available |
| TC-12 | Order status = Failed | Downloads not available |
| TC-13 | Buy 3x same digital product | Files shown once (not triplicated) |
| TC-14 | File deleted by merchant post-purchase | Error message shown |
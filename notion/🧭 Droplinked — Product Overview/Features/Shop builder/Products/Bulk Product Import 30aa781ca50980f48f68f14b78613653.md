# Bulk Product Import

### Feature ID:

**[IAA-BLK-009]**

### Title:

**Bulk Product Import (CSV Upload)**

### Category:

**Shop Builder | Product Management** | **Actors**: Merchant | **Channel**: Web Dashboard

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants need the ability to upload products in bulk via CSV or other file formats to save time when migrating from other platforms or adding large catalogs. Without bulk import, merchants must create each product individually, which is inefficient and error-prone for shops with hundreds or thousands of products.

**Desired Outcome:**

- Merchant can download CSV template with all required columns
- CSV upload accepts files up to 10MB
- System validates file format and encoding (UTF-8)
- Preview shows first 10 rows before import
- Field mapping interface for non-standard CSVs
- Import processes 100 products per minute (minimum)
- Import history saved for 90 days

---

### 2) **Scope – In / Out**

**In Scope:**

- **File Upload**
    - CSV file upload (primary format)
    - Excel file support (.xlsx) as secondary format
    - Downloadable CSV templates
    - File size limit: 10MB
    - Encoding validation (UTF-8)
- **Data Validation & Preview**
    - Validation of required fields (name, price, currency)
    - Preview screen showing first 10 rows
    - Field mapping interface for custom CSVs
    - Error reporting with row numbers and specific issues
    - Duplicate detection (by SKU)
- **Import Processing**
    - Support for 1000 products per import (limit)
    - Image URL import (with automatic download)
    - Variant import (size, color, etc.)
    - Category and tag assignment
    - Draft/published status setting
    - Import progress tracking for large files
- **History & Management**
    - Import history and logs
    - Import summary shows: total rows, success count, error count
    - Import history saved for 90 days
    - Failed imports can be retried with corrected file

**Out of Scope:**

- API-based bulk import (separate feature)
- Real-time sync with external platforms
- Scheduled/recurring imports
- Import from marketplaces (Amazon, eBay)
- Automatic category creation
- AI-powered data cleaning
- Import undo functionality
- Partial import success (all-or-nothing)
- Image editing during import
- Printful/POD product import via CSV

---

### 3) **Key User Journeys**

---

**Journey 1: CSV Upload and Import**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to Products → Import | Import page loads |
| 2 | Merchant | Downloads CSV Template | Template downloaded |
| 3 | Merchant | Fills CSV with Product Data | CSV prepared |
| 4 | Merchant | Uploads CSV File | File uploaded |
| 5 | System | Validates and Shows Preview | Preview displayed |
| 6 | Merchant | Reviews and Maps Fields (if needed) | Fields mapped |
| 7 | Merchant | Confirms Import | Import initiated |
| 8 | System | Products Created in Bulk | Products imported |
| 9 | System | Import Report Generated | Report available |

---

**Journey 2: Error Handling and Retry**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Uploads CSV | File uploaded |
| 2 | System | Validation Detects Errors | Errors identified |
| 3 | System | System Shows Error Report (Row-by-Row) | Error details displayed |
| 4 | Merchant | Downloads Error Report | Report downloaded |
| 5 | Merchant | Fixes Errors in CSV | CSV corrected |
| 6 | Merchant | Re-uploads Corrected File | File re-uploaded |
| 7 | System | Import Successful | Products imported |

---

**Journey 3: Template-Based Import**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Selects Platform (Shopify, WooCommerce, etc.) | Platform selected |
| 2 | System | System Provides Platform-Specific Template | Template provided |
| 3 | Merchant | Exports from Source Platform | Export completed |
| 4 | Merchant | Uploads Export File | File uploaded |
| 5 | System | Auto-Mapping Suggests Field Mappings | Mappings suggested |
| 6 | Merchant | Merchant Confirms/Adjusts Mappings | Mappings confirmed |
| 7 | System | Import Executed | Products imported |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **File Upload** |  |
| BAC-1 | Merchant can download CSV template with all required columns |
| BAC-2 | CSV upload accepts files up to 10MB |
| BAC-3 | System validates file format and encoding (UTF-8) |
| **Validation & Preview** |  |
| BAC-4 | Required fields validation: product_name, price, currency |
| BAC-5 | Preview shows first 10 rows before import |
| BAC-6 | Field mapping interface for non-standard CSVs |
| BAC-7 | Error report shows: row number, column, error message, suggested fix |
| **Import Processing** |  |
| BAC-8 | Import processes 100 products per minute (minimum) |
| BAC-9 | Import progress bar for files > 100 rows |
| BAC-10 | Duplicate SKU detection with options: skip, update, error |
| BAC-11 | Image URLs downloaded and processed automatically |
| BAC-12 | Variants import with parent/child relationship |
| **Reporting** |  |
| BAC-13 | Import summary shows: total rows, success count, error count |
| BAC-14 | Import history saved for 90 days |
| BAC-15 | Excel (.xlsx) files automatically converted |
| BAC-16 | Failed imports can be retried with corrected file |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-009 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **CSV** | Comma-Separated Values file format |
| **Field Mapping** | Matching CSV columns to product database fields |
| **SKU** | Stock Keeping Unit - unique product identifier |
| **Variant** | Product variation (size, color, etc.) |
| **Import Queue** | Background processing system for large imports |
| **Rollback** | Reverting all changes on import failure |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: CSV Template & Validation**

```
REQUIRED_COLUMNS = ['product_name', 'price', 'currency']
OPTIONAL_COLUMNS = [
    'sku', 'description', 'category', 'tags',
    'inventory_quantity', 'weight', 'weight_unit',
    'image_url_1', 'image_url_2', 'image_url_3',
    'variant_option_1_name', 'variant_option_1_value',
    'variant_option_2_name', 'variant_option_2_value',
    'status' // 'draft' or 'published'
]

FUNCTION generateTemplate():
    RETURN CSV.stringify({
        headers: REQUIRED_COLUMNS + OPTIONAL_COLUMNS,
        example_row: [
            'Sample Product', '29.99', 'USD',
            'SKU-001', 'This is a sample product description',
            'Electronics', 'gadget, tech',
            '100', '1.5', 'kg',
            '<https://example.com/image1.jpg>',
            '', '', // empty image slots
            'Color', 'Red',
            'Size', 'Large',
            'published'
        ]
    })

FUNCTION validateFile(file):
    // Check file size
    IF file.size > 10 * 1024 * 1024: // 10MB
        RETURN { valid: false, error: 'File exceeds 10MB limit' }

    // Check file type
    IF file.type NOT IN ['text/csv', 'application/vnd.ms-excel']:
        // Try to detect by extension
        IF NOT file.name ENDS_WITH '.csv' AND NOT file.name ENDS_WITH '.xlsx':
            RETURN { valid: false, error: 'File must be CSV or Excel (.xlsx)' }

    // Check encoding (UTF-8)
    encoding = detectEncoding(file.content)
    IF encoding != 'UTF-8':
        RETURN {
            valid: false,
            error: 'File encoding must be UTF-8. Detected: ' + encoding
        }

    RETURN { valid: true }

FUNCTION parseCSV(file_content):
    // Use streaming parser for large files
    parser = Papa.parse(file_content, {
        header: true,
        dynamicTyping: true,
        skipEmptyLines: true,
        complete: function(results):
            RETURN results.data
    })
```

---

### **FL-2: Data Validation & Field Mapping**

```
FUNCTION validateRow(row, row_number, field_mapping):
    errors = []

    // Map fields if custom mapping provided
    IF field_mapping:
        mapped_row = {}
        FOR each source_field, target_field IN field_mapping:
            mapped_row[target_field] = row[source_field]
        row = mapped_row

    // Validate required fields
    FOR each field IN REQUIRED_COLUMNS:
        IF NOT row[field] OR row[field].trim() == '':
            errors.push({
                row: row_number,
                column: field,
                error: 'Required field is empty',
                value: row[field]
            })

    // Validate price
    IF row.price:
        price = parseFloat(row.price)
        IF isNaN(price) OR price < 0:
            errors.push({
                row: row_number,
                column: 'price',
                error: 'Price must be a positive number',
                value: row.price
            })

    // Validate currency
    IF row.currency:
        IF NOT isValidCurrency(row.currency):
            errors.push({
                row: row_number,
                column: 'currency',
                error: 'Invalid currency code. Use ISO 4217 format (e.g., USD)',
                value: row.currency
            })

    // Validate SKU format
    IF row.sku:
        IF row.sku.length > 50:
            errors.push({
                row: row_number,
                column: 'sku',
                error: 'SKU must be 50 characters or less',
                value: row.sku
            })

    // Check duplicate SKU
    IF row.sku:
        existing = FIND product WHERE sku == row.sku AND shop_id == shop_id
        IF existing:
            errors.push({
                row: row_number,
                column: 'sku',
                error: 'SKU already exists in your shop',
                value: row.sku,
                existing_product: existing.name
            })

    RETURN {
        valid: errors.length == 0,
        errors: errors,
        mapped_row: row
    }

FUNCTION autoMapFields(csv_headers):
    // Suggest field mappings based on common names
    mappings = {}

    common_mappings = {
        'title': 'product_name',
        'name': 'product_name',
        'product title': 'product_name',
        'cost': 'price',
        'amount': 'price',
        'product_price': 'price',
        'product description': 'description',
        'body_html': 'description',
        'stock': 'inventory_quantity',
        'qty': 'inventory_quantity'
    }

    FOR each header IN csv_headers:
        normalized = header.toLowerCase().trim()
        IF normalized IN common_mappings:
            mappings[header] = common_mappings[normalized]

    RETURN mappings
```

---

### **FL-3: Import Processing**

```
FUNCTION processImport(import_job_id):
    job = GET import_job WHERE id == import_job_id

    // Update status
    job.status = 'processing'
    job.started_at = NOW
    SAVE job

    // Get rows to process
    rows = job.data.rows
    total_rows = rows.length

    IF total_rows > 1000:
        job.status = 'failed'
        job.error = 'Maximum 1000 rows allowed per import'
        SAVE job
        RETURN

    // Use transaction for atomicity
    BEGIN TRANSACTION

    TRY:
        success_count = 0
        error_count = 0

        FOR each row, index IN rows:
            // Update progress every 10 rows
            IF index % 10 == 0:
                job.progress = (index / total_rows) * 100
                SAVE job

            // Validate
            validation = validateRow(row, index + 1, job.field_mapping)

            IF NOT validation.valid:
                error_count += 1
                job.errors.push(validation.errors)
                CONTINUE

            // Create product
            product = createProductFromRow(validation.mapped_row, job.shop_id)

            IF product:
                success_count += 1

                // Process images if URLs provided
                IF validation.mapped_row.image_url_1:
                    downloadImagesAsync(product.id, [
                        validation.mapped_row.image_url_1,
                        validation.mapped_row.image_url_2,
                        validation.mapped_row.image_url_3
                    ])

                // Handle variants
                IF validation.mapped_row.variant_option_1_name:
                    createVariant(product, validation.mapped_row)
            ELSE:
                error_count += 1

        // Commit transaction
        COMMIT TRANSACTION

        // Update job status
        job.status = 'completed'
        job.completed_at = NOW
        job.success_count = success_count
        job.error_count = error_count
        job.progress = 100

        SAVE job

        // Send notification
        NOTIFY merchant "Import complete: ${success_count} products imported successfully"

    CATCH error:
        // Rollback on error
        ROLLBACK TRANSACTION

        job.status = 'failed'
        job.error = error.message
        job.completed_at = NOW

        SAVE job

        NOTIFY merchant "Import failed: ${error.message}"

FUNCTION createProductFromRow(row, shop_id):
    product = CREATE Product({
        shop_id: shop_id,
        name: row.product_name,
        price: parseFloat(row.price),
        currency: row.currency.toUpperCase(),
        sku: row.sku OR generateSKU(),
        description: row.description OR '',
        status: row.status OR 'draft',
        inventory_quantity: parseInt(row.inventory_quantity) OR 0,
        weight: parseFloat(row.weight) OR null,
        weight_unit: row.weight_unit OR 'kg',
        created_via: 'bulk_import',
        created_at: NOW
    })

    // Assign categories
    IF row.category:
        category = FIND_OR_CREATE category WHERE name == row.category
        product.categories.push(category)

    // Assign tags
    IF row.tags:
        tags = row.tags.split(',').map(t => t.trim())
        FOR each tag_name IN tags:
            tag = FIND_OR_CREATE tag WHERE name == tag_name
            product.tags.push(tag)

    SAVE product

    RETURN product
```

---

### **FL-4: Image Processing**

```
FUNCTION downloadImagesAsync(product_id, image_urls):
    FOR each url IN image_urls:
        IF NOT url:
            CONTINUE

        // Add to queue
        ADD_TO_QUEUE 'image_download', {
            product_id: product_id,
            image_url: url
        }

ON_QUEUE 'image_download' (job):
    TRY:
        // Download image
        response = HTTP.GET(job.image_url)

        IF response.status == 200:
            // Validate image
            IF NOT isValidImage(response.content):
                LOG error "Invalid image format"
                RETURN

            // Process image
            processed = processImage(response.content, {
                max_width: 2048,
                max_height: 2048,
                quality: 85
            })

            // Upload to CDN
            cdn_url = uploadToCDN(processed, job.product_id)

            // Update product
            product = GET product WHERE id == job.product_id
            product.images.push({
                url: cdn_url,
                original_url: job.image_url,
                uploaded_at: NOW
            })
            SAVE product

    CATCH error:
        LOG error "Failed to download image: ${error.message}"
        // Continue without image
```

---

### B3) Behavioral Flow

```
[Merchant Opens Import Page]
    ↓
[Download Template]
    ↓
[Fill CSV Data]
    ↓
[Upload File]
    ↓
[Validate File]
    ├─ Invalid → [Show Errors]
    │             ↓
    │         [Require Fix]
    │
    └─ Valid → [Parse CSV]
                ↓
            [Show Preview (10 rows)]
                ↓
            [Field Mapping (if needed)]
                ↓
            [Validate All Rows]
                ├─ Errors Found → [Show Error Report]
                │                   ↓
                │               [Download Error CSV]
                │                   ↓
                │               [Fix & Re-upload]
                │
                └─ All Valid → [Confirm Import]
                                ↓
                            [Start Processing]
                                ↓
                            [Show Progress Bar]
                                ↓
                            [Complete]
                                ├─ Success → [Show Summary]
                                └─ Failure → [Show Error, Allow Retry]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Draft | File uploaded | Validating | File validation runs |
| Validating | File valid | Preview | Preview generated |
| Validating | File invalid | Error | Errors displayed |
| Preview | Mapping confirmed | Ready | Awaiting confirmation |
| Ready | Import confirmed | Processing | Background job starts |
| Processing | Complete | Completed | Products created |
| Processing | Error | Failed | Transaction rolled back |
| Failed | Retry clicked | Validating | Re-validation starts |
| Completed | View history | Archived | History saved |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Malformed CSV | Show parse error with line number |
| **EC-2:** Image download fails | Continue without image, log warning |
| **EC-3:** Duplicate SKU detected | Offer skip/update/error options |
| **EC-4:** Import exceeds 1000 rows | Reject file, suggest splitting |
| **EC-5:** Network error during import | Retry once, then fail with option to resume |
| **EC-6:** Currency mismatch with shop | Flag warning, allow override |
| **EC-7:** Invalid image URL | Skip image, continue with product |
| **EC-8:** Category doesn't exist | Auto-create category (if configured) |
| **EC-9:** Transaction rollback fails | Manual intervention required alert |
| **EC-10:** Concurrent imports | Queue imports, process sequentially |

---

### B6) Data Requirements

**Import Job:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Job identifier |
| shop_id | UUID | Reference to shop |
| status | Enum | pending, processing, completed, failed |
| file_name | String | Original filename |
| file_size | Integer | Size in bytes |
| row_count | Integer | Total rows |
| field_mapping | JSON | Custom field mappings |
| data | JSON | Parsed CSV data |
| errors | JSON | Validation errors |
| success_count | Integer | Successfully imported |
| error_count | Integer | Failed rows |
| progress | Integer | Percentage complete |
| created_at | DateTime | When job created |
| started_at | DateTime | When processing started |
| completed_at | DateTime | When completed |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Download template | CSV with correct headers downloaded |
| TC-2 | Upload valid CSV | Preview displayed |
| TC-3 | Upload invalid format | Error message shown |
| TC-4 | Upload file >10MB | Rejected with size error |
| TC-5 | Map custom fields | Fields correctly mapped |
| TC-6 | Import with errors | Error report generated |
| TC-7 | Download error report | CSV with error details |
| TC-8 | Retry fixed CSV | Import succeeds |
| TC-9 | Import 1000 products | All products imported |
| TC-10 | Import with images | Images downloaded and attached |
| TC-11 | Duplicate SKU detection | Warning shown with options |
| TC-12 | View import history | Last 90 days imports listed |
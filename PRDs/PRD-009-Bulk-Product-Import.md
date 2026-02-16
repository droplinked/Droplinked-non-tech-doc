# PRD-009: Bulk Product Import

---

## Feature Specification

- **Feature Title:** Bulk Product Import (CSV Upload)
- **Feature ID:** IAA-BLK-009
- **Category:** Shop Builder | Product Management
- **Actors:** Merchant
- **Channel:** Web Dashboard
- **Status:** Defined
- **Owner:** Product Management
- **Linked Ticket:** Ticket 3 - Bulk Product Import (Delivery Phase)

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants need the ability to upload products in bulk via CSV or other file formats to save time when migrating from other platforms or adding large catalogs. Without bulk import, merchants must create each product individually, which is inefficient and error-prone for shops with hundreds or thousands of products. Bulk import streamlines the onboarding process and reduces time-to-market for new merchants.

### User Stories

- As a merchant, I want to upload a CSV file with product data so that I can add multiple products at once.
- As a merchant, I want to see a preview of my import before confirming so that I can catch errors early.
- As a merchant, I want clear error messages for invalid data so that I can fix issues and retry.
- As a merchant migrating from another platform, I want to map my CSV columns to Droplinked fields so that my data imports correctly.

### Key User Journeys

**Journey 1: CSV Upload and Import**
```
Merchant Navigates to Products → Import
    ↓
Downloads CSV Template
    ↓
Fills CSV with Product Data
    ↓
Uploads CSV File
    ↓
System Validates and Shows Preview
    ↓
Merchant Reviews and Maps Fields (if needed)
    ↓
Confirms Import
    ↓
Products Created in Bulk
    ↓
Import Report Generated
```

**Journey 2: Error Handling and Retry**
```
Merchant Uploads CSV
    ↓
Validation Detects Errors
    ↓
System Shows Error Report (Row-by-Row)
    ↓
Merchant Downloads Error Report
    ↓
Fixes Errors in CSV
    ↓
Re-uploads Corrected File
    ↓
Import Successful
```

**Journey 3: Template-Based Import**
```
Merchant Selects Platform (Shopify, WooCommerce, etc.)
    ↓
System Provides Platform-Specific Template
    ↓
Merchant Exports from Source Platform
    ↓
Uploads Export File
    ↓
Auto-Mapping Suggests Field Mappings
    ↓
Merchant Confirms/Adjusts Mappings
    ↓
Import Executed
```

### Scope

#### ✅ In Scope:
- CSV file upload (primary format)
- Excel file support (.xlsx) as secondary format
- Downloadable CSV templates
- Field mapping interface for custom CSVs
- Validation of required fields (name, price, currency)
- Preview screen showing first 10 rows
- Error reporting with row numbers and specific issues
- Import progress tracking for large files
- Support for 1000 products per import (limit)
- Image URL import (with automatic download)
- Variant import (size, color, etc.)
- Category and tag assignment
- Draft/published status setting
- Import history and logs
- Duplicate detection (by SKU)

#### ❌ Out of Scope:
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

### Acceptance Criteria

- ☑ Merchant can download CSV template with all required columns
- ☑ CSV upload accepts files up to 10MB
- ☑ System validates file format and encoding (UTF-8)
- ☑ Required fields validation: product_name, price, currency
- ☑ Preview shows first 10 rows before import
- ☑ Field mapping interface for non-standard CSVs
- ☑ Error report shows: row number, column, error message, suggested fix
- ☑ Import processes 100 products per minute (minimum)
- ☑ Import progress bar for files > 100 rows
- ☑ Duplicate SKU detection with options: skip, update, error
- ☑ Image URLs downloaded and processed automatically
- ☑ Variants import with parent/child relationship
- ☑ Import summary shows: total rows, success count, error count
- ☑ Import history saved for 90 days
- ☑ Excel (.xlsx) files automatically converted
- ☑ Failed imports can be retried with corrected file

### Technical Notes

- Client-side validation before upload (file size, format)
- Server-side streaming parser for large files (papaparse, csv-parser)
- Queue-based processing for imports (Bull/Redis)
- Image download and optimization async
- Transaction wrapping: all-or-nothing import
- Progress tracking via WebSocket or polling
- Validation schema based on product data model
- Sandbox environment for testing imports

### Dependencies
- Product management system
- Image processing service
- Category/tag system
- Variant management
- SKU uniqueness validation
- File storage system
- Queue processing infrastructure
- Import logging system

---

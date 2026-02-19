# Media Library

**Feature ID:** IAA-SB-001  
**Category:** Shop Builder / Shop Settings  
**Actors:** Merchant  
**Channel:** Web  
**Status:** Writing Spec (Draft)  
**Owner:** [Product Manager Name]  

---

## Part 1: Human-Readable Spec

### Problem Statement

Currently, when merchants upload images to the system, the images are directly uploaded to a CDN and only the URL is saved with the product. This approach has several limitations:

1. **No centralized image management** - Merchants cannot view, manage, or reuse images they've previously uploaded
2. **No ownership tracking** - There's no clear record of which merchant owns which images
3. **No storage monitoring** - Platform admins cannot track how much storage space each merchant is using (storage is tracked in backend only, not visible to merchants)
4. **No image metadata** - Image details (size, format, dimensions) are not stored or tracked
5. **Wasted storage** - Same images may be uploaded multiple times by the same merchant

This feature creates a **Media Library** system that allows merchants to centrally manage all their uploaded images, track storage usage, and reuse images across different products.

### User Stories

- As a **Merchant**, I want to upload images to a centralized library so that I can reuse them across multiple products without re-uploading.
- As a **Merchant**, I want to browse all my uploaded images in one place so that I can find and select the right image quickly.
- As a **Merchant**, I want to search and sort my images so that I can find specific images efficiently.
- As a **Platform Admin**, I want to track storage usage per merchant in the backend so that I can monitor resource consumption.
- As a **Platform Admin**, I want to track image ownership so that I can manage resources per merchant.

### Key User Journeys

#### Journey 1: Upload Image from Component
1. Merchant is on a page with the image upload component (e.g., product creation)
2. Merchant sees two buttons: "Upload" and "Browse Library"
3. Merchant clicks "Upload" button
4. System opens file picker dialog
5. Merchant selects image file from local system
6. System validates file (format: JPEG/JPG/PNG, size: max 5MB)
7. If valid, system uploads image to CDN
8. System saves image metadata to database (title, format, size, compressed version, original version)
9. System associates image with the merchant (ownership)
10. Image appears in the merchant's Media Library
11. Image is automatically selected/used in the current context

#### Journey 2: Browse and Select from Library
1. Merchant is on a page with the image upload component
2. Merchant sees two buttons: "Upload" and "Browse Library"
3. Merchant clicks "Browse Library" button
4. System navigates to Media Library view (modal or dedicated page)
5. Merchant sees their image collection:
   - If no images: Empty state with upload option
   - If images exist: Grid view of image thumbnails
6. Merchant can search images by title
7. Merchant can sort images by date or file size
8. Merchant selects an image from the library
9. System returns to previous page with selected image
10. Selected image is used in the current context

#### Journey 3: Backend Storage Tracking (Admin Only)
1. System tracks storage usage per merchant in backend
2. Admin can view storage metrics showing:
   - Total storage used by merchant
   - Number of images stored
3. Storage tracking is used for internal monitoring and resource management
4. **Note:** Storage metrics are NOT displayed to the merchant in the UI

### Scope

**✅ In Scope:**
- Reusable image upload component with dual buttons (Upload + Browse Library)
- Media Library view (accessible only via "Browse Library" button, not in sidebar)
- Empty state when no images exist in the library
- Grid view for image list display
- Image thumbnail preview in the list
- File validation: JPEG, JPG, PNG formats only, max 5MB
- CDN storage with tracking capabilities
- Storage usage tracking per merchant (backend only, not visible to users)
- Search functionality by image title
- Sort functionality by date and file size
- Image metadata storage: title, format, size, compressed version, original version
- Image ownership association with merchant
- Upload button within the Media Library to add new images

**❌ Out of Scope:**
- List view (only Grid view in this sprint)
- Image editing/cropping capabilities
- Image deletion from library
- Folder/collection organization
- Bulk upload operations
- Image analytics (views, downloads)
- Sharing images between merchants
- Video or other media types
- Drag-and-drop upload functionality

### Acceptance Criteria

- [ ] Merchant sees "Upload" and "Browse Library" buttons in the reusable upload component
- [ ] Clicking "Upload" opens system file picker and allows image selection
- [ ] System validates file format (JPEG, JPG, PNG only) and shows error for invalid formats
- [ ] System validates file size (max 5MB) and shows error for oversized files
- [ ] Valid images are uploaded to CDN successfully
- [ ] Image metadata (title, format, size, compressed version, original version) is stored in database
- [ ] Image is associated with the uploading merchant (ownership tracking)
- [ ] Clicking "Browse Library" navigates to Media Library view
- [ ] Media Library view displays empty state when no images exist with upload option
- [ ] Media Library view displays grid of image thumbnails when images exist
- [ ] Merchant can search images by title with real-time filtering
- [ ] Merchant can sort images by upload date (newest/oldest) and file size (largest/smallest)
- [ ] Upload button is visible at the top of Media Library page when images exist
- [ ] Storage usage is tracked in backend per merchant (not visible in UI)
- [ ] Selecting an image from library returns to the previous context with the image selected

### Technical Notes

- Images stored in CDN (CloudFront/Cloudflare) for fast global delivery
- Database schema needs fields for: merchant_id, title, format, size_bytes, original_url, compressed_url, created_at, updated_at
- Storage calculation should aggregate total size per merchant from database records (backend tracking only, no UI display)
- Consider implementing image compression for optimized storage (compressed version)
- Original version should be preserved for high-quality needs
- API endpoints needed: upload, list (with search/sort), get storage metrics (admin only)
- Media Library view should be a modal or dedicated route, not a sidebar item
- Component should be reusable across different contexts (product creation, profile, etc.)

### Dependencies

- Existing CDN infrastructure for image storage
- Authentication system for merchant identification
- Product management module (primary consumer of this feature)
- Database migration for new media library tables

---

## UI Flow (Source of Truth)

### Flow 1: Upload New Image from Component
```
[Page with Upload Component] 
    → Click "Upload" Button 
    → [System File Picker Opens]
    → Select Image File
    → [Validation Check]
    → If Valid: Upload to CDN → Save Metadata → Add to Library → Return with Image
    → If Invalid: Show Error Message → Retry
```

### Flow 2: Browse and Select Existing Image
```
[Page with Upload Component]
    → Click "Browse Library" Button
    → [Media Library View Opens]
    → [Check: Has Images?]
        → No: Show Empty State → Click "Upload First Image" → [Upload Flow]
        → Yes: Show Image Grid
    → [Optional] Search by Title
    → [Optional] Sort by Date/Size
    → Click on Image Thumbnail
    → Select Image
    → Return to Previous Page with Selected Image
```

### Flow 3: Upload from Media Library
```
[Media Library View - With Images]
    → Click "Upload" Button (Top of Page)
    → [System File Picker Opens]
    → Select Image File
    → [Validation Check]
    → If Valid: Upload to CDN → Save Metadata → Refresh Grid with New Image
    → If Invalid: Show Error Message
```

---

## Attachments

- 📎 **Linked Request:** [REQ-XXX]
- 📄 **Spec Document:** Features/Shop builder/Shop Settings/media-library-spec.md
- 🎨 **Design:** [Link to Figma - To be added]
- 🧪 **Test Cases:** [Link to Test Sheet - To be added]

---

## Change Log

- 2026-02-19 — [Name] — Initial draft created based on Persian requirements

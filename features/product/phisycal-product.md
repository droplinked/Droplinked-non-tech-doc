# Feature Name: Physical Product Management

**Version number:** 1.0.0  
**Date created/updated:** 2025/05/27

## 1. Overview:

The Physical Product feature enables merchants on the Droplinked platform to create and manage tangible goods with detailed configurations. Unlike other product types, Physical Products support multiple images and allow merchants to define custom variants such as size or color. Each variant requires its own SKU with associated price, external ID, and quantity. Additionally, Physical Products require packaging dimensions (width, height, length, weight) and a mandatory shipping type to be specified during creation. This feature addresses the need for precise handling of physical inventory and shipping logistics, providing merchants the tools to accurately represent and sell their physical products.

---

## 2. User Story:

- **As a** merchant,
- **I want to** create and manage Physical Products including multiple images, custom variants, packaging sizes, and shipping types,
- **So that I can** effectively manage my physical inventory and fulfill shipping requirements accurately.

## 3. Pre-requisites and Flow:

- **Pre-requisites**:

  - Merchant must be logged in to the Droplinked platform.

- **Flow**:
  1. **Step 1**: Merchant logs in and navigates to the Products page.
  2. **Step 2**: Merchant clicks the "Add Product" button.
  3. **Step 3**: Merchant selects "Physical Product" as the product type.
  4. **Step 4**: Merchant enters mandatory product information including title, description, collection, multiple images, and variants.
  5. **Step 5**: Merchant defines variants via a pop-up, specifying variant types (e.g., size) and their possible values (e.g., Large, Medium, XLarge).
  6. **Step 6**: For each SKU generated from the variants, merchant sets price, external ID, and quantity.
  7. **Step 7**: Merchant provides packaging size details (width, height, length, weight).
  8. **Step 8**: Merchant selects the mandatory shipping type.
  9. **Step 9**: Merchant saves the product to complete creation.

## 4. Inputs:

- **User Inputs**:
  - Title: Name of the physical product.
  - Description: Detailed information about the product.
  - Collection: Product collection/category.
  - Images: Multiple images showcasing the product.
  - Variants: Custom variants defined by the merchant (e.g., size, color).
  - SKU Details (per variant):
    - Price: Price for each SKU variant.
    - External ID: Unique identifier for each SKU in external systems.
    - Quantity: Available stock for each SKU.
  - Packaging Size:
    - Width: Width of the product package.
    - Height: Height of the product package.
    - Length: Length of the product package.
    - Weight: Weight of the product package.
  - Shipping Type: Mandatory selection indicating the shipping method for the product.

## 5. Logic and Process:

The feature follows the logic outlined below:

1. **Product Creation Validation**:  
   Validates mandatory fields including title, description, collection, images, and variants.  
   For each variant SKU, price, external ID, and quantity are validated.  
   Packaging size (width, height, length, weight) and shipping type must be provided and validated.

2. **Variant Definition**:  
   Merchants define variants through a pop-up interface, selecting variant types (e.g., size) and their possible values (e.g., Large, Medium, XLarge).  
   Each variant combination generates SKUs requiring individual pricing, IDs, and quantities.

3. **Product Editing**:  
   Merchants can edit all product fields, with validations applied similarly as in creation.

4. **Product Deletion**:  
   When deleted, the product and its variants are permanently removed from the database.

5. **Product Reordering**:  
   Merchants can reorder products via a modal, and the new order is reflected across the Shop Builder and Shop Frontend.

---

## 6. Outputs:

The feature produces the following outputs:

- **Database Update**:  
  Upon creation or update, the product with all its variants and packaging details is saved to the database and shown in product listings.

- **UI Notifications**:  
  Toast notifications confirm successful creation, update, deletion, or reorder actions.

---

## 7. Error Handling and Troubleshooting:

- **Error 1**: Missing mandatory fields such as title, description, variants, packaging size, or shipping type.

  - **Solution**: Prompt merchant to fill all required fields before submission.

- **Error 2**: Invalid variant SKU details (e.g., missing price or quantity).

  - **Solution**: Validate inputs and show clear error messages.

- **Error 3**: Failure during product deletion or reorder due to server errors.
  - **Solution**: Notify merchant and provide retry options; log errors for backend review.

---

## 8. Testing and Validation:

- **Test Case 1**: Create a Physical Product with multiple variants, images, packaging sizes, and shipping type; verify it appears correctly in the product list and frontend.
- **Test Case 2**: Attempt creation with missing mandatory fields and confirm error messages are shown.
- **Test Case 3**: Edit variant details and confirm changes save properly.
- **Test Case 4**: Delete a Physical Product and verify permanent removal from database and UI.
- **Test Case 5**: Reorder products and verify updated order reflects in both Shop Builder and frontend.

---

## 9. Change Log:

- **[Date]**: Initial release of Physical Product feature including multiple images, variant management, packaging size, and shipping type requirements.

---

## 10. Limitations and Notes:

- **Limitations**:

  - Variants must be fully defined with price, external ID, and quantity for each SKU.
  - Shipping type is mandatory and currently limited to predefined options.
  - Deletions are permanent with no recovery.

- **Notes**:
  - Physical Product documentation is maintained separately from other product types due to unique complexities.
  - Proper definition of packaging size and shipping type is critical for logistics integration.

# Feature Name: Product Management

**Version number:** 1.0.0  
**Date created/updated:** 2025/05/27

## 1. Overview:

Product feature enables merchants on the Droplinked platform to manage their products with ease. Merchants can create, edit, delete, and reorder products through a user-friendly dashboard. Products are classified into three types: Physical, Digital, and Print-on-Demand (POD), each with distinct attributes and requirements.

## 2. User Story:

- **As a** merchant,
- **I want to** create, edit, and manage different types of products (Physical, Digital, POD),
- **So that I can** effectively organize my store's offerings and provide accurate product details to my customers.

## 3. Pre-requisites and Flow:

**Pre-requisites**:

- The merchant must be logged in to the Droplinked platform.

**Flow**:

- **Step 1**: Merchant logs in and navigates to the Products page.
- **Step 2**: Merchant clicks the "Add Product" button.
- **Step 3**: Merchant selects the product type (Physical, Digital, or POD).
- **Step 4**: Merchant enters the required product information (e.g., title, description, images, variants, price).
- **Step 5**: Merchant clicks the "Save" button to create the product.

## 4. Inputs:

- **User Inputs**:
  - **Product Title**: The name of the product.
  - **Description**: Detailed information about the product.
  - **Images**:
    - Physical and POD products can have multiple images without limitation.
    - Digital products can have only one image.
  - **Variants**: Different versions of the product (e.g., size, color) each with its own price.
  - **Pricing**: Price specified per variant.
  - **Release Date**: (If applicable) The date when the product becomes available.
  - **Shipping Type**: (For Physical products) Specifies the shipping method.
  - **Design Uploads**: (For POD products) Design files uploaded via the integrated Design Maker.

## 5. Logic and Process:

The feature follows the logic outlined below:

1. **Product Creation Validation**:  
   When a product is created, the system validates required fields including title, description, media, collection, and price.  
   For Physical and POD products, variants and quantities are also validated.  
   For Digital products, quantity validation applies as well.  
   Each product type has specific validation rules as detailed in the respective sections.

2. **Product Editing**:  
   Merchants can update any of the product information entered during creation.  
   All validations from the creation process are re-applied to ensure data integrity.

3. **Product Deletion**:  
   When a product is deleted, it is permanently removed from the database without recovery options.

4. **Product Reordering**:  
   Merchants can reorder products via a modal interface.  
   The updated order is reflected both in the Shop Builder and the Shop Frontend to maintain consistency.

## 6. Outputs:

The feature produces the following outputs:

- **Database Update**:  
  Upon successful product creation, the product is added to the database and appears in the product list on both the Shop Builder and Shop Frontend.

- **UI Notification**:  
  A toast notification is displayed in the frontend confirming the successful creation of the product.


## 7. Error Handling and Troubleshooting:
- **Error 1**: Missing required fields during product creation (e.g., title, description, images).  
  - **Solution**: Display an error message prompting the merchant to fill in all mandatory fields before saving.

- **Error 2**: Invalid variant pricing or quantity values.  
  - **Solution**: Validate variant inputs on the client and server side; show error messages for invalid entries.

- **Error 3**: Failure to save product due to server/database issues.  
  - **Solution**: Display a generic error notification and suggest retrying; log errors for backend investigation.

- **Error 4**: Failure during product deletion.  
  - **Solution**: Notify the user of the failure and retry option; ensure consistency between frontend and backend.

- **Error 5**: Issues with product reordering not reflecting in UI or database.  
  - **Solution**: Validate reorder requests; refresh UI after reorder; log errors if synchronization fails.


  ## 8. Testing and Validation:
- **Test Case 1**: Create a product with all valid inputs and verify it appears in the product list on both Shop Builder and Shop Frontend.
- **Test Case 2**: Attempt to create a product with missing mandatory fields and verify that appropriate error messages are shown.
- **Test Case 3**: Edit an existing product, update its information, and confirm that changes are saved and reflected correctly.
- **Test Case 4**: Delete a product and verify it is permanently removed from the database and no longer displayed in the UI.
- **Test Case 5**: Reorder products via the UI and confirm the updated order is correctly saved and displayed in both Shop Builder and Shop Frontend.


## 9. Change Log:
- **[ 2025/5/27 ]**: Initial release of the Product feature with support for creating, editing, deleting, and reordering Physical, Digital, and POD products.

- **Note**: Documentation for each product type (Physical, Digital, POD) will be written separately to provide detailed and focused information.
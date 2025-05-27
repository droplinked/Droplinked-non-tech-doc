# Feature Name: Digital Product Management

**Version number:** 1.0.0  
**Date created/updated:** 2025/05/27

## 1. Overview:

The Digital Product feature is a specialized type of product within the Droplinked platform, designed to manage and sell digital goods. While it follows the general product creation flow, merchants must specifically select "Digital Product" as the product type during creation. Unlike other product types, Digital Products have unique attributes and restrictions: they allow uploading only one image, do not support variants, and have a single price and quantity. Additionally, each Digital Product requires an external ID. Two exclusive features distinguish Digital Products: an optional delivery message consisting of customizable text and a link sent via email to customers upon purchase, and a custom field displayed to customers during purchase to collect additional information. This feature aims to streamline the management and sale of digital goods by providing tailored fields and workflows suited to their nature. اینو اضافه بکن

## 2. User Story:

- **As a** merchant,
- **I want to** create, edit, and manage Digital Products with their unique attributes,
- **So that I can** effectively sell digital goods tailored to their specific requirements.

## 3. Pre-requisites and Flow:

- **Pre-requisites**:

  - Merchant must be logged in to the Droplinked platform.

- **Flow**:
  1. **Step 1**: Merchant logs in and navigates to the Products page.
  2. **Step 2**: Merchant clicks the "Add Product" button.
  3. **Step 3**: Merchant selects "Digital Product" as the product type.
  4. **Step 4**: Merchant fills in required product information, including title, description, image, price, quantity, and external ID.
  5. **Step 5**: Merchant optionally configures delivery message and custom field.
  6. **Step 6**: Merchant clicks "Save" to create the Digital Product.

## 5. Logic and Process:

The feature follows the logic outlined below:

1. **Product Creation Validation**:  
   When creating a Digital Product, the system validates required fields including title, description, image (only one allowed), price, quantity, and external ID.  
   The system ensures no variants are created as Digital Products do not support variants.

2. **Product Editing**:  
   Merchants can update all the Digital Product details with the same validations as creation.

3. **Delivery Message Handling**:  
   If the delivery message feature is enabled, upon purchase, the system sends an email to the customer containing the delivery message and delivery link.

4. **Product Deletion**:  
   When a Digital Product is deleted, it is permanently removed from the database without recovery.

5. **Product Listing**:  
   Created Digital Products are listed in the merchant's product list and shown on the storefront once published.

## 4. Inputs:

- **User Inputs**:
  - Title: Name of the digital product.
  - Description: Detailed information about the product.
  - Image: A single image representing the product.
  - Price: The price of the digital product.
  - Quantity: Number of available licenses or downloads.
  - External ID: Unique identifier for the product in external systems.
  - Delivery Message (optional): Custom text message sent to customers after purchase.
  - Delivery Link (optional): URL included in the delivery message.
  - Custom Field (optional): Additional field displayed to customers during purchase to collect specific data.

## 6. Outputs:

The feature produces the following outputs:

- **Database Update**:  
  Upon successful creation, the Digital Product is added to the database and appears in the product list and storefront.

- **UI Notification**:  
  A confirmation toast notification is shown to the merchant upon successful creation or update.

- **Delivery Email**:  
  If enabled, a delivery email containing the custom message and link is sent to customers who purchase the Digital Product.

## 7. Error Handling and Troubleshooting:

- **Error 1**: Missing required fields such as title, description, image, price, or external ID during product creation or editing.

  - **Solution**: Display an error message prompting the merchant to fill in all mandatory fields before proceeding.

- **Error 2**: Attempt to upload more than one image for a Digital Product.

  - **Solution**: Restrict image upload to a single file and notify the user of the limitation.

- **Error 3**: Failure to send delivery email after purchase.

  - **Solution**: Log the error, notify administrators, and provide merchants with options to resend the delivery email manually.

- **Error 4**: Product deletion fails due to server or database issues.
  - **Solution**: Notify the user of the failure and suggest retrying later; log the error for investigation.

## 8. Testing and Validation:

- **Test Case 1**: Create a Digital Product with all valid inputs and verify it appears correctly in the product list and storefront.
- **Test Case 2**: Attempt to create a Digital Product missing mandatory fields and verify that appropriate error messages are displayed.
- **Test Case 3**: Edit an existing Digital Product and confirm that updates are saved and reflected properly.
- **Test Case 4**: Delete a Digital Product and ensure it is permanently removed from the database and UI.
- **Test Case 5**: Purchase a Digital Product with delivery message enabled and verify the customer receives the delivery email with the correct message and link.
- **Test Case 6**: Attempt to upload multiple images for a Digital Product and verify the system restricts to only one image.


## 9. Change Log:
- **[ 2025/5/27 ]**: Initial release of the Digital Product feature including creation, editing, deletion, single image upload restriction, delivery message and email functionality.

## 10. Limitations and Notes:
- **Limitations**:  
  - Digital Products support only one image upload.  
  - Variants (such as size or color) are not supported for Digital Products.  
  - Delivery message and custom fields are only applicable if explicitly enabled.  
  - Deleted Digital Products are permanently removed and cannot be recovered.

- **Notes**:  
  - Detailed documentation for Digital Product differs from other product types and is maintained separately.  

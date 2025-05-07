# Feature Name: Import Products via URL

**Version number:** 2.0.0  
**Date created/updated:** 2025/5/6

## 1. Overview:

**Import Products via URL** is designed to allow merchants to easily import products from external e-commerce platforms, such as Shopify or WooCommerce, into their Droplinked store. This feature addresses the problem of migrating products from other systems, providing an easy and efficient way to transfer multiple products at once. It benefits users by simplifying the process of store migration and bulk importing, ensuring a smooth transition to Droplinked without losing product data. The goal is to provide a seamless migration experience and enable merchants to scale their product offerings quickly and efficiently.

## 2. User Story:

- **As a** merchant,
- **I want to** import products from external platforms like Shopify or WooCommerce using a URL,
- **So that I can** easily migrate my products to Droplinked without having to manually add each product.

## 3. Pre-requisites and Flow:

- **Pre-requisites**:

  - The merchant must have a store set up on Droplinked.
  - The merchant must be logged in to their Droplinked account.
  - The merchant must have an active store on Shopify or WooCommerce from which products will be imported.
  - The merchant must have set a producer address for their store in Droplinked.

- **Flow**:
  1. The merchant logs in to their Droplinked account and navigates to the "Products" section.
  2. The merchant clicks the "Import Product" button.
  3. A modal window appears with a field for URL import (Product Listing URL).
  4. The merchant enters the URL of their Shopify or WooCommerce store.
  5. The system makes an API call to the external platform and retrieves all product data.
  6. The products are displayed in a table format, allowing the merchant to view and select the desired products to import.
  7. The merchant can select all products or specific ones they wish to import.
  8. After selecting the products, the merchant clicks the "Import" button.
  9. An API call is made to start the import process.
  10. The merchant is notified that the products will be imported within 15 minutes.
  11. After 15 minutes, the selected products are imported as draft physical products in Droplinked.

## 4. Inputs:

- **User Inputs**:
  - **Product URL**: The merchant needs to provide the URL of their Shopify or WooCommerce store's product listing page in the "URL Import" field. This URL is used to fetch product details.
  - **Product Selection**: The merchant can choose to select all products or manually select individual products they wish to import.

## 5. Logic and Process:

The feature follows the logic outlined below:

1. **Step 1**: The merchant enters the product listing URL (from Shopify or WooCommerce) in the "URL Import" field.
2. **Step 2**: The system scrapes the provided URL to retrieve key product information such as name, image, and price, and displays this data in a table format for the merchant to review.
3. **Step 3**: The merchant selects the products they wish to import and confirms their selection.
4. **Step 4**: The system scrapes detailed information for the selected products from the external platform (Shopify/WooCommerce).
5. **Step 5**: For each selected product, a new physical product draft is created in Droplinked by default.
6. **Step 6**: The system notifies the merchant that the import process will be completed within 15 minutes.
7. **Step 7**: After 15 minutes, the selected products are added to the merchant's store as draft physical products in Droplinked.

## 6. Outputs:

The feature produces the following outputs:

- **Success Notification**: After the final API call is made, a success message is shown to the merchant, indicating that the import process has been successfully initiated.
- **Modal Closure**: The modal window is closed once the merchant confirms the product selection, and the success notification appears.
- **Product Import Completion**: After 15 minutes, the selected products are automatically added to the merchant’s Droplinked store as draft physical products.

## 7. Error Handling and Troubleshooting:

- **Error 1**: **404 Error on Product Import**

  - **Description**: The system encounters a 404 error when trying to fetch product data from the provided URL.
  - **Solution**: The merchant should double-check the product URL to ensure it is valid. If the issue persists, they may need to try again later or contact support.

- **Error 2**: **No Store Address Set**

  - **Description**: The merchant hasn't set a producer address for their store in Droplinked.
  - **Solution**: The merchant needs to set the store address in their account settings before attempting to import products.

- **Error 3**: **Incomplete Product Import**

  - **Description**: Some products may not be fully imported during the process.
  - **Solution**: If some products are missing, the merchant should try re-importing or check the system for any issues. If the problem persists, contacting support may be necessary.

- **Error 4**: **Data Not Properly Passed During Product Creation**
  - **Description**: There may be issues with data being passed correctly during the product creation step (e.g., missing or incorrect product attributes).
  - **Solution**: Currently, there is no specific solution available, but merchants should ensure that all necessary data is properly configured in their source store. Future updates may address this issue.

## 8. Testing and Validation:

- **Test Case 1**: **Valid Product URL**

  - **Scenario**: Test the feature with a valid Shopify or WooCommerce product URL.
  - **Expected Outcome**: The system successfully fetches the product details, displays them in a table, and allows the merchant to select and import products.

- **Test Case 2**: **Invalid Product URL (404 Error)**

  - **Scenario**: Test the feature with an invalid product URL that results in a 404 error.
  - **Expected Outcome**: The system should display an error message notifying the merchant that the URL is invalid and provide guidance to correct the issue.

- **Test Case 3**: **No Store Address Set**

  - **Scenario**: Attempt to import products without setting a producer address in Droplinked.
  - **Expected Outcome**: The system should display an error message prompting the merchant to set a store address before proceeding with the import.

- **Test Case 4**: **Incomplete Product Import**

  - **Scenario**: Test with a scenario where some products fail to import (e.g., missing product details or connection issues).
  - **Expected Outcome**: The system should inform the merchant about the incomplete import and allow them to retry the import for the missing products.

- **Test Case 5**: **Successful Product Import**

  - **Scenario**: Test the end-to-end process with a valid URL, correct store address, and product selection.
  - **Expected Outcome**: After the merchant confirms the selection and the 15-minute waiting period, the products should appear as draft physical products in their Droplinked store.

- **Test Case 6**: **Edge Case - Large Number of Products**

  - **Scenario**: Test the feature with a store that has a large number of products (e.g., over 100 products).
  - **Expected Outcome**: The system should be able to handle large imports efficiently and notify the merchant upon completion.

- **Test Case 7**: **Edge Case - Special Characters in Product Data**

  - **Scenario**: Test the import process with products containing special characters in their names or descriptions (e.g., accented characters, symbols).
  - **Expected Outcome**: The system should correctly handle and display special characters without errors or data corruption.

- **Test Case 8**: **Performance Test - Quick Import**
  - **Scenario**: Measure the time it takes to import products from a valid URL with 10-20 products.
  - **Expected Outcome**: The system should complete the import process within a reasonable time frame (e.g., within 15 minutes as mentioned in the feature flow).

## 9. Change Log:

- **[ 2.0.0 / 2025/5/6 ]**: First version of the "Import Products via URL" feature has been implemented and deployed. This version supports basic product import from Shopify and WooCommerce, with a success notification and product import process.

## 10. Limitations and Notes:

- **Limitations**:
  - This feature is still in the testing phase and may have various unresolved issues.
  - Not all product import scenarios have been fully addressed (e.g., handling of certain product data or import errors).
  - The feature may not work as expected for stores with a large number of products or special characters in product data.
- **Notes**:
  - Currently, the task is not fully completed and there are many edge cases and scenarios that still need to be addressed.
  - Merchants should be aware that some issues may arise during the testing phase, and troubleshooting may be required.
  - This feature is expected to evolve with future updates, and improvements will be made to handle more complex scenarios.

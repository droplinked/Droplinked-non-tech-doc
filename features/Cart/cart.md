# Feature Name: Cart

**Version number:** 1.0.0  
**Date created/updated:** 2025/05/27

## 1. Overview:

**Cart** is designed to enable customers to add products from a store and proceed with purchasing them. This feature addresses the fundamental need for users to select and manage items they intend to buy on the storefront. The goal is to provide a seamless and intuitive shopping experience that facilitates easy product selection, modification, and checkout.


## 2. User Story:
- **As a** customer (both logged-in and anonymous),  
- **I want to** add products to the cart, update quantities, and review my selected items,  
- **So that I can** select the products I want and pay for them all at once.

## 3. Pre-requisites and Flow:

- **Pre-requisites**:
  - The selected product variant must have available quantity.
  - The product’s visibility status must be public (not private).
  - The product’s release date must have passed.
  - The product must pass any active ruleset validations (handled by a separate feature).

- **Flow**:
  1. **User visits a store on the Shop Frontend.**
  2. **User browses products and selects a product.**
  3. **User chooses product variants (e.g., size, color), with a default variant selected by default.**
  4. **User selects quantity (default is 1).**
  5. **User adds the product to the cart.**
  6. **If all pre-requisites are met, the product is added to the cart and a success notification is shown.**
  7. **The cart icon updates to show the current number of products in the cart.**
  8. **User clicks the cart icon to open the cart popup.**
  9. **User reviews and modifies cart items if needed, then clicks checkout.**
  10. **User is redirected to the cart page, where further modifications are possible.**
  11. **User proceeds to the payment page.**
  12. **User enters personal information.**
  13. **If physical or print-on-demand products are in the cart, user enters shipping details (address).**
  14. **User may optionally enter additional notes.**
  15. **User selects a shipping method from the list (if shipping details exist).**
  16. **User selects a payment method and completes the payment.**
  17. **Order is placed successfully.**


## 4. Inputs:
- **User Inputs**:
  - **Product Variant**: The customer selects a variant of the product (e.g., size, color). One variant is selected by default.
  - **Quantity**: The customer selects the number of units to add to the cart. Default is 1.
  - **Personal Information**: Customer enters personal details during checkout (e.g., name, email, phone).
  - **Shipping Details**: If the cart contains physical or print-on-demand products, the customer provides shipping address details.
  - **Additional Note** (optional): Customer can add extra notes related to the order.
  - **Shipping Method**: Customer selects a shipping option from the available list after entering shipping details.
  - **Payment Method**: Customer selects a preferred payment option to complete the purchase.


## 5. Logic and Process:
The feature follows the logic outlined below:
1. **Product Selection**: User selects a product variant and quantity (default variant and quantity 1 are pre-selected).
2. **Validation Checks**: Before adding the product to the cart, the system checks:
   - Availability of the selected variant quantity.
   - Product visibility status (must not be private).
   - Product release date (must have passed).
   - Ruleset validation (if active, product must pass).
3. **Add to Cart**: If all validations pass, the product is added to the cart, and a success message is shown.
4. **Failure Handling**: If any validation fails, the product is not added, and an error message is displayed to the user.
5. **Cart Icon Update**: The cart icon on the page updates to reflect the current total number of items.
6. **Cart Popup**: User can click the cart icon to open the cart popup to review and modify items.
7. **Checkout Process**:
   - User proceeds to the cart page for final review and adjustments.
   - User advances to payment page.
   - Personal information is entered.
   - If physical or print-on-demand products are present, shipping details are required.
   - User may add an optional additional note.
   - User selects a shipping method from available options.
   - User selects a payment method.
   - Order is submitted and confirmed.

## 6. Outputs:
The feature produces the following outputs:
- Success messages when a product is added to the cart successfully.
- Error messages when adding to the cart fails due to validation errors.
- Real-time update of the cart icon showing the total number of items.
- Order confirmation message after successful checkout and payment.

## 7. Error Handling and Troubleshooting:
- **Out of Stock Error**: When the selected quantity exceeds available stock.
  - **Solution**: Display an error message informing the user of insufficient stock.
- **Private Product Error**: When the product visibility is private.
  - **Solution**: Prevent adding to cart and notify the user accordingly.
- **Ruleset Validation Failure**: If the product does not pass active ruleset conditions.
  - **Solution**: Display relevant error messages and block addition to cart.
- **Product Release Date Not Reached**: If the product is not yet available for sale.
  - **Solution**: Inform the user that the product is not yet released.
- **General Troubleshooting**: Guide users to check inputs and retry; log errors for admin review.

## 8. Testing and Validation:
- **Test Case 1**: Add product with sufficient stock to the cart.
- **Test Case 2**: Attempt to add out-of-stock product and verify error is shown.
- **Test Case 3**: Try to add a private product and ensure it is blocked.
- **Test Case 4**: Add products with variants and confirm correct variant is added.
- **Test Case 5**: Complete checkout flow with physical products, including shipping details.
- **Test Case 6**: Complete checkout flow with digital products, skipping shipping.
- **Test Case 7**: Verify cart icon updates correctly on add/remove actions.
- **Test Case 8**: Test ruleset validation failures prevent cart additions.

## 9. Change Log:
- **2025-05-27**: Initial implementation of the Cart feature covering product selection, validation, cart management, and checkout flow.


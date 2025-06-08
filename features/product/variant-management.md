# Feature Name: Variant Management for Physical Products

**Version number:** 1.0.0  
**Date created/updated:** 2025/05/27

## 🔍 Description
This feature allows merchants to manage product variants during the product creation process for physical products. Variants enable merchants to define different options (such as color or size) for their products. Each combination of variants results in a unique SKU, and merchants can assign specific prices, quantities, and external IDs to each SKU. This feature helps merchants offer customizable options to their customers, allowing better control over inventory and product details.

## 👤 User Stories
- As a **merchant**, I want to **create variants for my physical products**, so that **I can offer multiple options (e.g., color, size) to my customers.**
- As a **merchant**, I want to **assign SKUs, prices, and quantities to each variant**, so that **I can manage inventory and sales effectively.**
- As a **merchant**, I want to **define custom variant types** (such as custom attributes) so that **I can tailor the product options to meet my store’s needs.**

## 🧠 Business Logic

### Step 1: **Clicking "Add Variant"**
- **Logic**: When the merchant clicks the "Add Variant" button, a new component opens, allowing the merchant to define a variant type.
- **Rules**: The merchant must first choose a variant type (e.g., color, size, or custom). 
- **Validation**: No validation here; the merchant can freely define the type of variant.
- **Outcome**: The variant type input appears with the required fields to input variant data.

### Step 2: **Defining Variant Type**
- **Logic**: After selecting the variant type, input fields for the corresponding values appear.
  - For **Color**, the merchant must provide a color code (e.g., "#FFFF00") and a name for the color variant (e.g., "Yellow").
  - For **Size** or **Custom**, the merchant only needs to provide a name for the variant (e.g., "Small", "Large", or any custom attribute name).
- **Rules**: There is no restriction on the variant names or values.
- **Validation**: If "Color" is selected, the color must be defined in the #HEX format.
- **Outcome**: The merchant successfully defines the variant with the correct input type and values.

### Step 3: **Automatic SKU Generation**
- **Logic**: Once the merchant has defined the variants (e.g., colors and sizes), the system automatically generates SKUs for every possible combination.
  - If the merchant defines 3 colors and 3 sizes, the system will automatically generate 9 SKUs, each corresponding to a unique combination of color and size.
- **Rules**: SKUs are generated automatically based on the combinations of variants.
- **Validation**: The SKU generation logic ensures that every unique variant combination has its own SKU.
- **Outcome**: The system generates SKUs and assigns them to the product.

### Step 4: **Setting Prices and Quantities for SKUs**
- **Logic**: The merchant can either assign prices and quantities for each SKU individually or in bulk.
  - Each SKU must have a price and quantity, which can vary between different variants.
- **Rules**: 
  - A price and quantity must be defined for each SKU. 
  - The merchant can choose to enter prices and quantities one by one or in bulk for all SKUs.
- **Validation**: There is no limit on the number of variants or prices/quantities. The system just requires each SKU to have valid pricing and quantity.
- **Outcome**: Prices and quantities are set for all defined SKUs.

### Step 5: **Assigning External ID (Optional)**
- **Logic**: The merchant has the option to assign an external ID to each SKU, which can be used for syncing with external systems or inventory management.
- **Rules**: External ID is optional and not required for SKU creation.
- **Validation**: No validation is done on the external ID since it is optional and not unique.
- **Outcome**: If provided, the external ID is stored for each SKU.

### Step 6: **Finalizing the Product Creation**
- **Logic**: The product with its defined variants, SKUs, prices, quantities, and optional external IDs is finalized and saved.
- **Rules**: A product must have at least one SKU and no more than two variant types (e.g., color and size). However, these variants can have an unlimited number of values.
- **Validation**: The system will check that the product has at least one SKU and no more than two variant types.
- **Outcome**: The product is successfully created with variants, SKUs, and associated information.

## Testing and Validation:


## 📌 Change Log

| Version | Date       | Change Summary         | Author     |
|---------|------------|-------------------------|------------|
| 1.0     |  | Initial version created | Your Name  |

## 🔗 Dependencies
- **Product**: This feature is dependent on the product creation process. Variants can only be managed for physical products that have already been created.


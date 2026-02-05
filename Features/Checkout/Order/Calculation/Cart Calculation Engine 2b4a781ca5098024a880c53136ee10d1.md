# Cart Calculation Engine

**Feature ID:** CHK-CART-CALCULATION-001

**Category:** Checkout Domain | Core Calculation

**Actors:** Checkout Service, Storefront, External Services

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

## **Problem / Value**

The checkout system requires a reliable **calculation engine** that determines the financial structure of the shopping cart, including items, shipping, taxes, discounts, and the final total.

This engine must be **accurate, deterministic, UI-independent**, and serve as the foundation for:

- Checkout display
- Order creation
- Payment processing
- Commission engines
- Merchant payout calculations

Any miscalculation here results in **financial inconsistencies** across the entire platform.

## **Desired Outcome**

A modular calculation engine that:

- Takes a full cart input
- Independently computes all cost components
- Returns a normalized **CartSummary Object**
- Has no dependency on storefront or UI
- Is fully testable and platform-independent

---

# **2) Scope**

## **In Scope**

- Item totals
- Internal and external shipping
- Tax computation
- All discount types
- Coupon validation result processing
- Final cart total
- Multi-currency handling
- Per-item breakdown
- Financial rounding rules

## **Out of Scope**

- Saving/modifying the cart
- User actions (select shipping, apply payment)
- Payment routing
- Order creation
- Blockchain payments
- Affiliate/commission/final payout calculations
- UI rendering

---

# **3) Key User Journeys**

---

## **Journey 1 – Cart Calculation for Storefront Display**

1. Storefront requests cart details
2. Engine calculates all cost components
3. Storefront displays the CartSummary to the user

---

## **Journey 2 – Cart Calculation During Checkout**

1. User enters shipping/address/coupon
2. Checkout service calls the Calculation Engine
3. Engine returns the updated totals
4. Values are shown in the checkout review step

---

## **Journey 3 – Cart Recalculation After Coupon Application**

1. User enters a coupon
2. Coupon service validates it
3. Engine recalculates totals with the discount
4. Storefront updates the displayed summary

---

# **4) Business Logic Specification**

---

## **4.1 Item Total Calculation**

For each cart item:

```
itemTotal = itemPrice * quantity

```

`TotalItems` = sum of all itemTotals.

Supports:

- Physical products
- POD products
- Digital products

---

## **4.2 Shipping Calculation**

Engine receives two data types:

### **1. internalShipping**

- Merchant defined
- Flat or dynamic

### **2. externalShipping**

- Printful
- EasyPost
- Other carriers

Final formula:

```
TotalShipping = sum(internalShipping[]) + sum(externalShipping[])

```

---

## **4.3 Tax Calculation**

Based on product type:

- **Physical Tax** → Rules based on destination
- **POD Tax** → From Printful API
- **Digital Tax** → Always 0

```
TotalTax = physicalTax + podTax + digitalTax

```

---

## **4.4 Discount Calculation**

Discount types:

### **Item-level discounts**

Product-specific markdowns.

### **Cart-level discounts**

Coupon percentage or flat amount.

Formula:

```
totalDiscount = itemDiscounts + couponDiscount

```

Coupon types:

- Percentage (e.g. 15% off)
- Flat discount (e.g. $10 off)
- Free shipping (discount applied to shipping field)

---

## **4.5 Final Total Calculation**

```
totalCart = (TotalItems + TotalShipping + TotalTax) - totalDiscount

```

Engine rules:

- Minimum total = 0
- Rounding follows financial decimal rules
- Multi-currency safe calculations (no float errors)

---

---

# **6) Business Acceptance Criteria (BAC)**

1. Engine must correctly calculate TotalItems based on price × quantity.
2. Engine must sum internal & external shipping accurately.
3. Tax must respect product types (physical/pod/digital).
4. Discounts must apply exactly once.
5. Total formula = items + shipping + tax − discounts.
6. Final total cannot be negative.
7. Multi-currency amounts must be precise and rounded properly.
8. Output must match the CartSummary schema.
9. Engine must work independently of UI or payment modules.
10. No floating-point precision errors allowed.
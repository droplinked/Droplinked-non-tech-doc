# Product Tile

## **Product Tile**

### Feature ID:

**PRD-TILE-001**

### Title:

**Product Tile – Embed Product on External Websites**

### Category:

**Product Management / Storefront** | **Actors**: Merchant, Customer | **Channel**: Merchant Website / Droplinked Modal

### Status:

**Defined**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**

Merchants want to embed individual products directly on their external websites to drive sales without requiring customers to visit the Droplinked storefront. The **Product Tile** allows seamless integration of product listings via a simple HTML tag and provides a mini-checkout experience on the merchant’s site.

**Desired Outcome:**

- Merchants can embed products with `<Droplinked-Tile product-id='' />` by adding Droplinked tags to their website header.
- Customers can view the product tile, interact with it like a mini cart, and complete purchases via a modal.
- Merchants can edit tile design to match their website branding.
- Customers complete all steps for checkout without leaving the website, including viewing order tracking URLs.

---

### 2) **Scope – In / Out**

**In:**

- Embed products on external websites using the Droplinked Tile tag.
- Display product information, images, and pricing inside the tile.
- Enable mini-cart and modal checkout on the website.
- Tile supports customer checkout steps:
    1. Enter email (mandatory)
    2. Add note (optional)
    3. Enter shipping address (for physical products)
    4. Select shipping method (for physical products)
    5. Apply coupon code
    6. Select payment type (Stripe, Coinbase, Web3 wallet)
    7. Complete payment and display order URL
- Merchants can customize the tile design to match their website theme.

**Out:**

- Full storefront embedding (only single product per tile).
- Multi-currency display or advanced analytics inside the tile (handled in separate features).
- Bulk product tiles on a single page (each tile must be separate).

---

### 3) **Key User Journeys**

- **Journey 1:**
    - **Role/Trigger**: Merchant adds Droplinked tags to website header
    - **Behavior/Action**: Embeds `<Droplinked-Tile product-id='' />` on page
    - **Value/Outcome**: Product is displayed on the website as a mini-cart
- **Journey 2:**
    - **Role/Trigger**: Merchant edits tile design
    - **Behavior/Action**: Updates colors, layout, and display elements
    - **Value/Outcome**: Tile matches website branding
- **Journey 3:**
    - **Role/Trigger**: Customer clicks the tile’s buy button
    - **Behavior/Action**: Modal checkout opens
    - **Value/Outcome**: Customer completes purchase without leaving the website
- **Journey 4:**
    - **Role/Trigger**: Customer completes checkout steps
    - **Behavior/Action**:
        - Enter email
        - Add note (optional)
        - Enter shipping address (if physical)
        - Select shipping option (if physical)
        - Apply coupon code
        - Select payment type
        - Pay
    - **Value/Outcome**: Customer receives order confirmation and sees tracking URL

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1**: Product tile renders correctly on merchant website after embedding tag → Pass/Fail
- **BAC 2**: Mini-cart and modal checkout opens correctly on tile click → Pass/Fail
- **BAC 3**: Customers must complete all checkout steps; mandatory fields enforced → Pass/Fail
- **BAC 4**: Merchants can edit tile design and see updates reflected on website → Pass/Fail
- **BAC 5**: Payment completion triggers order creation in Droplinked and displays tracking URL → Pass/Fail
- **BAC 6**: Tile supports physical and digital products with shipping and tax rules applied correctly → Pass/Fail
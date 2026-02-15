# Feature Solutions - Scale & Intersection

---

## Feature 1: Shop Currency

### Question 1: What happens to previous product and order prices when currency changes?
**Solution:**
- Completed orders remain saved in the original currency and don't change
- Existing products: currency change only allowed for new shops or when no products exist

### Question 2: What if the payment provider doesn't support the selected currency?
**Solution:**
- Payment methods that don't support the shop's currency are automatically disabled and not shown
- Error message: "This payment method is not available for your selected currency"

### Question 3: What if merchant and affiliate want to sell simultaneously with different currencies?
**Solution:**
- Both shops must have the same currency (can't have one USD and one EUR)
- When affiliate is activated, the shop's currency is locked and cannot be changed
- OR: If currencies differ, they receive internal credit instead of direct payment

### Question 4: What should fixed-price coupon codes be based on?
**Solution:**
- Each coupon is saved with the shop's currency at the time of creation
- At the time of use, the coupon price is converted to the shop's current currency

### Question 5: What currency should shop credit be based on?
**Solution:**
- Option 1: Saved as USD, displayed in local currency in dashboard
- Option 2: Saved as USD and displayed as USD (without conversion)

### Question 6: What currency should subscription be purchased in?
**Solution:**
- Subscription page always in USD
- Display of equivalent local currency only for information (optional)

### Question 7: What about Printful and EasyPost costs that are in USD?
**Solution:**
- Costs saved in USD
- If shop currency is non-USD, currency difference is factored in when calculating profit
- OR: Currency conversion fee added to customer

---

## Feature 2: Merchant Direct Payment

### Question 1: What if the merchant's PayPal/Stripe account onboarding remains incomplete?
**Solution:**
- Show "In Progress" status in dashboard
- Payment method disabled until onboarding is complete
- Reminder email sent to complete onboarding
- If not completed within 7 days, onboarding is cancelled

### Question 2: What if they make sales without connecting an account?
**Solution:**
- If account not connected, payment goes to Droplinked
- Internal credit created for merchant to withdraw later
- Warning before activation: "Connect your account to receive direct payments"

### Question 3: How should they set up wallets for different crypto tokens and networks?
**Solution:**
- Each network requires a separate wallet
- Whitelist of tokens: only supported tokens can be selected
- Address validation for each network (EVM, Solana, etc.)
- Store mapping of network → wallet → supported tokens

### Question 4: What if they haven't set up a wallet but want to activate payment?
**Solution:**
- Setting up wallet for crypto is mandatory
- Activation button disabled until wallet is set up
- Tooltip: "First connect your wallet in settings"

### Question 5: Coinbase Commerce doesn't have account connection - what happens when a customer pays with it?
**Solution:**
- Since account cannot be connected, direct payment splitting is not possible
- Droplinked receives the money and splits it later (like internal credit)
- OR: Use Coinbase Webhook to trigger split after payment

---

## Feature 3: Merchant Wallet (Non-Custodial)

### Question 1: How should Circle and Xion wallets be managed?
**Solution:**
- "My Wallets" tab in settings
- List of wallets with status (active/inactive)
- Operations: create, deactivate, delete (if balance is zero)
- Select default wallet for receiving funds

### Question 2: How do they connect to payment method and receive their tokens?
**Solution:**
- System automatically selects appropriate wallet
- OR: Manual selection of wallet for each payment method
- After first transaction, wallet is locked for that network

### Question 3: What should the UI/UX be?
**Solution:**
- Wallet card with truncated address, balance, network
- QR Code for receiving funds
- Transaction history
- Copy address button

### Question 4: For Circle which creates on multiple networks, how should addresses be displayed?
**Solution:**
- Tab for each supported network
- Separate address for each network
- Warning: "Only send tokens of this network to this address"
- Network icon next to address

### Question 5: How do they withdraw money or NFT?
**Solution:**
- "Withdraw" button on wallet page
- Form: destination address, amount, network
- Validation: sufficient balance, address format
- Two-step confirmation (email or 2FA)
- NFT list with send option

---

## Feature 4: Affiliate

### Question 1: What if 2 shops don't have the same currency?
**Solution:**
- Both shops must have the same currency
- If one is USD and one is EUR, affiliate is not possible

### Question 2: If we make direct payment splitting mandatory and they have different payment methods?
**Solution:**
- Primary preference: automatic payment via smart contract (On-Chain)
- Payment to wallet addresses of both parties on blockchain
- Transaction stored on Chain

### Question 3: What if the middle shop in affiliate wants to change their currency?
**Solution:**
- Currency change is blocked if product is in affiliate
- Message: "First remove the product from affiliate"

### Question 4: How should crypto payment for affiliate order be handled?
**Solution:**
- If both parties have set up wallets: automatic split via smart contract
- If both parties haven't set up wallets: Droplinked receives money and splits later (like credit)
- OR: Make wallet setup mandatory before activating affiliate

### Question 5: What if an affiliate has products from multiple different shops?
**Solution:**
- Affiliate products should not be "add to cart"
- "Buy Now" button for each product
- Each product goes directly to its own checkout
- OR: Separate shopping cart for each shop

### Question 6: If someone's shop was previously referred by someone else, and now an affiliate sells it, how is commission calculated?
**Solution:**
- Priority to affiliate (since they made the direct sale)
- OR: Split between referral and affiliate (e.g., 1% to referral, rest to affiliate)
- Configurable in admin panel

### Question 7: What do we do with discount codes on affiliate orders?
**Solution:**
- Discount codes only created by affiliate
- Discount amount deducted from affiliate's commission

### Question 8: What if affiliate cancels affiliate capability for their products?
**Solution:**
- Products change to "Out of Stock" (not deleted)
- Existing orders have 30 days to complete
- Notification to affiliate: "Your products are no longer available"

### Question 9: What if affiliate commission percentage changes?
**Solution:**
- Change only applies to new orders
- Previous orders calculated with previous percentage
- Store history of percentage changes

---

## Feature 5: Product On-Chain Record

### Question 1: What are the requirements and how should it be implemented?
**Solution:**
- ERC-721 or ERC-1155 smart contract
- Metadata: name, description, image, price, product ID
- Cheap networks: Polygon, Base, BSC
- Merchant wallet for signing transactions
- Store images on IPFS

### Question 2: How should NFT transfer to buyer work after sale (direct or affiliate)?
**Solution:**
- After payment confirmation, NFT automatically transferred to buyer's wallet
- For affiliate: NFT transferred to buyer, not affiliate
- OR: New mint for each purchase to buyer's address

### Question 3: How do we store manufacturing, sales, purchase, and payment split info in NFT?
**Solution:**
- Metadata fields:
  - `creator`: Creator's address
  - `affiliator`: Affiliate's address (if applicable)
  - `buyer`: Buyer's address
  - `revenue_split`: Split percentage (e.g., {"affiliator": 15, "merchant": 85})
  - `transaction_hash`: Payment transaction hash
- Array of transaction history
- Public API for querying

### Question 4: What happens if a recorded product is refunded?
**Solution:**
- NFT returns to merchant's wallet (transfer back)
- Refund record stored on blockchain
- "Remint" option for reselling
- OR: Burn (destroy) the refunded NFT


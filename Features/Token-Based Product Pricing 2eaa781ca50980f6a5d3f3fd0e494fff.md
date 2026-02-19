# Token-Based Product Pricing

# Token-Based Product Pricing (Crypto/USDT) for Web3 Shops

### Feature ID:

**[TBP-001]**

### Title:

**Cryptocurrency Token Pricing for Web3-Enabled Shops**

### Category:

**Web3 / Product Management** | **Actors**: Web3 Merchant, Customer | **Channel**: Merchant Dashboard, Shop Front (Web3)

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Web3-native merchants need to price and sell their products using cryptocurrency tokens (USDT, ETH, native tokens) rather than fiat currencies. This enables seamless integration with blockchain-based checkout flows and appeals to crypto-native customer bases who prefer token-based transactions.

**Desired Outcome:**

- Web3-enabled shops can set product prices in cryptocurrency tokens (USDT, USDC, ETH, etc.)
- Product catalog displays prices in both token amount and approximate fiat equivalent
- Checkout process supports token-based payments through connected wallets
- Merchants can choose which tokens to accept per product or shop-wide
- Automatic price conversion based on real-time exchange rates
- Clear distinction between Web2 (fiat-only) and Web3 (token-enabled) shops

---

### 2) **Scope – In / Out**

**In Scope:**

- **Merchant Dashboard:**
    - Token pricing toggle for Web3 shops
    - Token selection per product (USDT, USDC, ETH, native chain tokens)
    - Price input in token amounts with fiat reference
    - Shop-wide default token settings
    - Token pricing display preferences
- **Product Management:**
    - Set prices in crypto tokens during product creation/editing
    - Variant-level token pricing (different tokens for different variants)
    - Token price history and conversion tracking
- **Shop Front:**
    - Display product prices in selected token (e.g., "50 USDT")
    - Show approximate fiat equivalent (e.g., "≈ $50.00 USD")
    - Token icon/symbol indicators
- **Checkout:**
    - Token payment flow through Web3 wallets
    - Real-time price oracle for token-fiat conversion
    - Multi-token support in single cart
    - Token approval and transaction signing

**Out of Scope:**

- Fiat currency checkout for token-priced products (pure crypto flow)
- Token staking or yield-earning features
- NFT-based product pricing
- Cross-chain token bridging within checkout
- Advanced DeFi integrations (lending, borrowing)
- Token price prediction or hedging tools

---

### 3) **Key User Journeys**

---

**Journey 1: Web3 Merchant Enables Token Pricing**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Has Web3-enabled shop | Dashboard loads with Web3 features |
| 2 | Merchant | Navigates to Shop Settings | Settings page displayed |
| 3 | Merchant | Enables "Token Pricing" toggle | Token pricing feature activated |
| 4 | Merchant | Selects default token for shop (e.g., USDT) | Default token saved |
| 5 | Merchant | Configures display options (show fiat equivalent: yes/no) | Preferences saved |
| 6 | System | Updates shop to accept token payments | Token pricing mode enabled |

---

**Journey 2: Merchant Sets Product Price in Tokens**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to Products → Add New | Product creation form loads |
| 2 | Merchant | Enters product details (name, description, images) | Details saved |
| 3 | Merchant | Selects "Token Pricing" option | Token input fields appear |
| 4 | Merchant | Chooses token (USDT, ETH, etc.) | Token selected |
| 5 | Merchant | Enters price amount in tokens (e.g., "100") | Price validated |
| 6 | System | Shows fiat equivalent based on current rate (e.g., "≈ $100.05 USD") | Conversion displayed |
| 7 | Merchant | Saves product | Product created with token pricing |
| 8 | System | Displays product on shop front with token price | "100 USDT" shown |

---

**Journey 3: Customer Views Token-Priced Product**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Navigates to Web3 shop | Shop front loads |
| 2 | Customer | Browses product catalog | Products displayed with token prices |
| 3 | Customer | Views product detail page | "Price: 50 USDT ≈ $49.85 USD" shown |
| 4 | Customer | Selects variant (if applicable) | Price updates if variant has different token/amount |
| 5 | Customer | Clicks "Add to Cart" | Item added to cart |

---

**Journey 4: Customer Checkout with Token Payment**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Proceeds to checkout with token-priced items | Checkout page loads |
| 2 | System | Displays cart summary in tokens | "Total: 150 USDT" |
| 3 | Customer | Connects Web3 wallet (MetaMask, etc.) | Wallet connection established |
| 4 | System | Detects wallet network and token balance | Balance checked |
| 5 | Customer | Reviews token total and confirms | Confirmation displayed |
| 6 | Customer | Clicks "Pay with USDT" | Token approval transaction triggered |
| 7 | Customer | Approves token spend in wallet | Approval confirmed on-chain |
| 8 | System | Processes payment transaction | Transaction submitted |
| 9 | System | Waits for blockchain confirmation | Confirmation received |
| 10 | System | Creates order with "Paid" status | Order confirmed, receipt shown |

---

**Journey 5: Merchant Views Token Sales Report**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to Reports → Sales | Sales report page loads |
| 2 | Merchant | Filters by date range | Report updates |
| 3 | System | Displays sales in tokens and fiat equivalent | "Total: 500 USDT (≈ $498.50 USD)" |
| 4 | Merchant | Exports report | CSV/Excel with both token and fiat values |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | Web3 shops can enable token pricing feature |  |
| BAC-2 | Merchants can set product prices in cryptocurrency tokens |  |
| BAC-3 | Supported tokens include: USDT, USDC, ETH, native chain tokens |  |
| BAC-4 | Token prices display on shop front with token symbol |  |
| BAC-5 | Approximate fiat equivalent shown alongside token price |  |
| BAC-6 | Token prices use real-time exchange rate oracles |  |
| BAC-7 | Token approval flow works correctly during checkout |  |
| BAC-8 | Payment transaction is processed on blockchain |  |
| BAC-9 | Order is created only after blockchain confirmation |  |
| BAC-10 | Variant-level token pricing is supported |  |
| BAC-11 | Merchants can view sales reports in both tokens and fiat |  |
| BAC-12 | Token pricing is restricted to Web3-enabled shops only |  |
| BAC-13 | Shop-wide default token can be configured |  |
| BAC-14 | Customers can pay with connected Web3 wallet |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-18 | Behdad | Initial document creation | Web3 feature specification |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Token Pricing** | Setting product prices in cryptocurrency tokens instead of fiat currency |
| **Web3 Shop** | Shop enabled with blockchain/Web3 features including crypto payments |
| **Token Standard** | ERC-20 for fungible tokens (USDT, USDC), native tokens (ETH, MATIC) |
| **Price Oracle** | External service providing real-time token-to-fiat exchange rates |
| **Token Approval** | Blockchain transaction allowing a contract to spend user's tokens |
| **Fiat Equivalent** | Approximate fiat currency value of a token amount based on exchange rate |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Token Pricing Configuration**

```
STRUCTURE ProductPricing:
    pricingType: "FIAT" | "TOKEN"
    fiatPrice: Money (amount, currency) // Always maintained for reference
    tokenPrice: TokenAmount (amount, tokenAddress, tokenSymbol)
    displayOptions: {
        showFiatEquivalent: boolean
        primaryDisplay: "TOKEN" | "FIAT"
    }

SUPPORTED_TOKENS = [
    { symbol: "USDT", name: "Tether USD", type: "ERC20", decimals: 6 },
    { symbol: "USDC", name: "USD Coin", type: "ERC20", decimals: 6 },
    { symbol: "ETH", name: "Ethereum", type: "NATIVE", decimals: 18 },
    { symbol: "MATIC", name: "Polygon", type: "NATIVE", decimals: 18 },
    { symbol: "BNB", name: "BNB", type: "NATIVE", decimals: 18 }
]

FUNCTION setTokenPrice(productId, tokenSymbol, amount):
    product = GET product WHERE id = productId
    
    token = FIND token IN SUPPORTED_TOKENS WHERE symbol = tokenSymbol
    IF NOT token:
        RETURN error("Unsupported token")
    
    // Get current exchange rate
    exchangeRate = callPriceOracle(tokenSymbol, product.shop.baseCurrency)
    
    product.pricing.pricingType = "TOKEN"
    product.pricing.tokenPrice = {
        amount: amount,
        tokenSymbol: tokenSymbol,
        tokenAddress: token.contractAddress,
        decimals: token.decimals
    }
    product.pricing.fiatPrice = {
        amount: amount * exchangeRate,
        currency: product.shop.baseCurrency
    }
    
    SAVE product
    RETURN success
```

---

### **FL-2: Price Display Logic**

```
FUNCTION getDisplayPrice(product, customerPreferences):
    
    IF product.pricing.pricingType == "FIAT":
        RETURN formatMoney(product.pricing.fiatPrice)
    
    ELSE IF product.pricing.pricingType == "TOKEN":
        tokenPrice = product.pricing.tokenPrice
        fiatPrice = product.pricing.fiatPrice
        
        displayString = ""
        
        IF product.pricing.displayOptions.primaryDisplay == "TOKEN":
            displayString = f"{tokenPrice.amount} {tokenPrice.tokenSymbol}"
            
            IF product.pricing.displayOptions.showFiatEquivalent:
                displayString += f" ≈ {formatMoney(fiatPrice)}"
        
        ELSE:
            displayString = formatMoney(fiatPrice)
            displayString += f" ({tokenPrice.amount} {tokenPrice.tokenSymbol})"
        
        RETURN displayString
```

---

### **FL-3: Real-Time Price Oracle Integration**

```
FUNCTION callPriceOracle(tokenSymbol, targetCurrency):
    
    cacheKey = f"{tokenSymbol}_{targetCurrency}"
    cachedRate = GET cached_rate WHERE key = cacheKey
    
    // Return cached if recent (< 5 minutes)
    IF cachedRate AND cachedRate.timestamp > NOW() - 5 minutes:
        RETURN cachedRate.value
    
    // Fetch fresh rate
    TRY:
        rate = CALL externalPriceAPI.getRate(tokenSymbol, targetCurrency)
        
        CACHE rate WITH:
            key: cacheKey
            value: rate
            timestamp: NOW()
            ttl: 300 seconds
        
        RETURN rate
    
    CATCH error:
        IF cachedRate:
            // Use stale cache as fallback
            RETURN cachedRate.value
        ELSE:
            THROW error("Unable to fetch exchange rate")
```

---

### **FL-4: Token Checkout Flow**

```
FUNCTION processTokenCheckout(cart, walletAddress):
    
    // Calculate total per token
    tokenTotals = {}
    FOR item IN cart.items:
        IF item.product.pricing.pricingType == "TOKEN":
            token = item.product.pricing.tokenPrice.tokenSymbol
            amount = item.product.pricing.tokenPrice.amount * item.quantity
            tokenTotals[token] += amount
    
    // Check wallet balances
    FOR token, amount IN tokenTotals:
        balance = CALL blockchain.getBalance(walletAddress, token)
        IF balance < amount:
            RETURN error(f"Insufficient {token} balance")
    
    // Create pending order
    order = CREATE order:
        status: "PENDING_PAYMENT"
        items: cart.items
        tokenTotals: tokenTotals
        walletAddress: walletAddress
        createdAt: NOW()
        expiresAt: NOW() + 2 hours
    
    // Generate payment instructions
    paymentData = {
        orderId: order.id,
        tokenTransfers: []
    }
    
    FOR token, amount IN tokenTotals:
        tokenContract = GET token.contractAddress WHERE symbol = token
        paymentData.tokenTransfers.push({
            token: token,
            tokenAddress: tokenContract,
            amount: amount,
            recipient: PLATFORM_WALLET_ADDRESS
        })
    
    RETURN {
        order: order,
        paymentData: paymentData,
        nextStep: "AWAITING_TOKEN_APPROVAL"
    }
```

---

### **FL-5: Token Payment Processing**

```
FUNCTION processTokenPayment(orderId, transactionHash):
    
    order = GET order WHERE id = orderId
    
    IF order.status != "PENDING_PAYMENT":
        RETURN error("Invalid order status")
    
    IF order.expiresAt < NOW():
        SET order.status = "EXPIRED"
        RETURN error("Payment window expired")
    
    // Verify transaction on blockchain
    txReceipt = CALL blockchain.getTransactionReceipt(transactionHash)
    
    IF NOT txReceipt:
        RETURN { status: "PENDING", message: "Transaction not yet confirmed" }
    
    IF txReceipt.status != "SUCCESS":
        SET order.status = "PAYMENT_FAILED"
        LOG failure reason
        RETURN error("Transaction failed on blockchain")
    
    // Validate transaction details
    FOR expectedTransfer IN order.paymentData.tokenTransfers:
        actualTransfer = FIND transfer IN txReceipt.transfers WHERE
            transfer.token == expectedTransfer.token AND
            transfer.to == expectedTransfer.recipient AND
            transfer.amount >= expectedTransfer.amount
        
        IF NOT actualTransfer:
            SET order.status = "PAYMENT_FAILED"
            RETURN error("Payment amount mismatch")
    
    // Payment successful
    SET order.status = "PAID"
    SET order.paidAt = NOW()
    SET order.transactionHash = transactionHash
    
    // Update inventory
    FOR item IN order.items:
        DECREMENT item.product.inventory BY item.quantity
    
    // Send confirmation
    NOTIFY customer("Payment confirmed - Order #{order.id}")
    NOTIFY merchant("New token order received - Order #{order.id}")
    
    RETURN { status: "SUCCESS", order: order }
```

---

### **FL-6: Variant-Level Token Pricing**

```
STRUCTURE ProductVariant:
    id: UUID
    sku: String
    attributes: Map (color, size, etc.)
    pricing: {
        type: "INHERIT" | "OVERRIDE"
        inheritFrom: "PRODUCT"
        tokenPrice: TokenAmount (if OVERRIDE)
        fiatPrice: Money (if OVERRIDE)
    }

FUNCTION getVariantPrice(variantId):
    variant = GET variant WHERE id = variantId
    
    IF variant.pricing.type == "INHERIT":
        product = GET product WHERE id = variant.productId
        RETURN product.pricing
    
    ELSE IF variant.pricing.type == "OVERRIDE":
        RETURN variant.pricing
```

---

### B3) Behavioral Flow

```
[Merchant Enables Token Pricing]
    ↓
[Configure Shop Default Token]
    ↓
[Create/Edit Product]
    ├─ Select Token (USDT, ETH, etc.)
    ├─ Enter Amount
    └─ System Calculates Fiat Equivalent
    ↓
[Product Displayed on Shop Front]
    ├─ Shows: "100 USDT"
    └─ Shows: "≈ $99.50 USD"
    ↓
[Customer Adds to Cart]
    ↓
[Checkout - Web3 Wallet Required]
    ├─ Connect Wallet
    ├─ Review Token Total
    └─ Approve Token Spend
    ↓
[Blockchain Transaction]
    ├─ Submit Payment
    ├─ Wait Confirmation
    └─ Verify Amount
    ↓
[Order Confirmed]
    ├─ Inventory Updated
    └─ Notifications Sent
```

---

### B4) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Price oracle is down | Use last cached rate, show warning about price approximation |
| **EC-2:** Token price volatility during checkout | Lock price for 15 minutes, show countdown |
| **EC-3:** Customer has insufficient token balance | Show error with current balance, suggest different token or top-up |
| **EC-4:** Token approval transaction fails | Allow retry, show specific error from wallet |
| **EC-5:** Payment transaction times out | Check blockchain for pending tx, allow extended wait |
| **EC-6:** Customer pays wrong amount | Mark as partial payment, notify merchant, request additional payment |
| **EC-7:** Token contract is paused/frozen | Show error, suggest alternative token |
| **EC-8:** Network congestion (high gas) | Warn about slow confirmation, allow payment window extension |
| **EC-9:** Merchant changes price during customer checkout | Honor price at time of checkout start (price lock) |
| **EC-10:** Customer uses wrong network | Detect network mismatch, prompt to switch networks |

---

### B5) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Enable token pricing for Web3 shop | Toggle activates, token options appear |
| TC-2 | Set product price in USDT | Price saved, fiat equivalent calculated |
| TC-3 | View token-priced product on shop front | Shows "100 USDT ≈ $99.50 USD" |
| TC-4 | Checkout with USDT payment | Token approval flow works, transaction processes |
| TC-5 | Insufficient token balance | Error shown, suggests adding funds |
| TC-6 | Price oracle fetch failure | Uses cached rate, shows approximation warning |
| TC-7 | Variant with different token price | Correct variant price displayed and charged |
| TC-8 | Payment transaction timeout | Extended wait allowed, can check status |
| TC-9 | Wrong network selected | Detects mismatch, prompts network switch |
| TC-10 | View sales report with token orders | Shows token amounts and fiat equivalents |
| TC-11 | Token price changes during checkout | Price locked at checkout start, honored |
| TC-12 | Try token pricing on non-Web3 shop | Feature unavailable, fiat pricing only |
| TC-13 | Multi-token cart checkout | Each token paid separately, all confirmed |
| TC-14 | Token contract paused | Error shown, suggests alternative token |

---

**Document End**

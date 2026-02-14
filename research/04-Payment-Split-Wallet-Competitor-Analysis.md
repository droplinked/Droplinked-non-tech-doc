# Feature Competitor Analysis: Payment Split & Wallet Management

## Executive Summary

This document analyzes how platforms handle payment splitting, crypto wallet management, and non-custodial wallet settings for merchants.

---

## Competitor Analysis

### 1. Stripe Connect (Payment Splitting)

**How They Handle Payment Split:**
- **Model**: Marketplace platform with automated splitting
- **Types**: Separate charges and transfers, or destination charges
- **Fees**: Split among recipients or borne by platform
- **Timing**: Instant or delayed transfers

**Key Features:**
- Multi-party payments
- Automatic fund routing
- Onboarding for connected accounts
- Compliance and tax handling
- Global payout support

**Payment Flow:**
1. Customer pays platform
2. Platform retains fee
3. Remainder split among recipients
4. Funds transferred to connected accounts
5. Connected accounts receive payouts

**BACs:**
1. Platform controls split percentages
2. Connected accounts must be verified
3. Split can happen before or after charge
4. Platform can hold funds if needed
5. Automated tax form generation (1099-K, etc.)

**Strengths:**
- Mature, battle-tested infrastructure
- Comprehensive compliance
- Global coverage
- Excellent developer experience
- Multiple split models

**Weaknesses:**
- High fees (2.9% + 30¢ + platform fee)
- Connected account onboarding complexity
- Limited crypto support
- Holds and reserves for risk management

---

### 2. Splits.org (On-Chain Payment Splitting)

**How They Handle Payment Split:**
- **Model**: Smart contract-based revenue sharing
- **Type**: Immutable split contracts on EVM chains
- **Cost**: Gas only (no protocol fees)
- **Chains**: Ethereum, Polygon, Base, Optimism, Arbitrum, etc.

**Key Features:**
- Immutable split contracts
- Multiple recipients with percentages
- Gas-efficient distributions
- Composable with other contracts
- Open source and audited

**Contract Types:**
- **Split**: Basic percentage-based splitting
- **Waterfall**: Sequential distribution with recoup
- **Swapper**: Auto-swap to stablecoins before split
- **Vesting**: Time-based release

**BACs:**
1. Create split contract with recipients and percentages
2. Send funds to contract address
3. Anyone can trigger distribution
4. Recipients withdraw their share
5. Immutable once created (prevents tampering)

**Strengths:**
- Zero protocol fees
- Fully on-chain (transparent)
- Immutable (trustless)
- Gas efficient
- Composable with DeFi

**Weaknesses:**
- Requires crypto knowledge
- Gas costs for creation and distribution
- Immutable (cannot change recipients)
- No fiat off-ramp
- Self-service (no customer support)

---

### 3. Circle Wallets (Custodial Solution)

**How They Handle Wallet Management:**
- **Model**: Custodial wallet infrastructure
- **Type**: Programmable wallets with API control
- **Custody**: Circle holds keys, merchant controls via API
- **Compliance**: Full KYC/AML, regulated entity

**Key Features:**
- Multi-chain support (EVM, Solana)
- Gasless transactions (sponsored gas)
- Wallet recovery mechanisms
- Scalable infrastructure
- Enterprise security

**Wallet Types:**
- **User-controlled**: User owns keys ( MPC technology)
- **Developer-controlled**: Developer manages keys via API
- **Smart contract wallets**: Programmable with recovery

**BACs:**
1. Create wallets via API
2. No private key management for end users
3. Gas sponsorship available
4. Recovery mechanisms built-in
5. Compliance handled by Circle

**Strengths:**
- No user friction (no seed phrases)
- Recoverable accounts
- Gas sponsorship
- Enterprise-grade security
- Regulatory compliance

**Weaknesses:**
- Custodial (platform risk)
- API dependency
- Costs for high volume
- Limited to supported chains

---

### 4. MetaMask Institutional (Non-Custodial for Organizations)

**How They Handle Wallet Management:**
- **Model**: Non-custodial with institutional controls
- **Type**: Multi-sig, policy enforcement, compliance
- **Custody**: Client holds keys, MetaMask provides interface
- **Features**: Transaction policies, reporting, DeFi access

**Key Features:**
- Multi-signature wallets
- Transaction policies and approval workflows
- Compliance reporting
- DeFi protocol integration
- Role-based access control

**BACs:**
1. Organization controls private keys
2. Multi-sig requirements configurable
3. Transaction policies enforceable
4. Audit trail for all activity
5. Integration with DeFi protocols

**Strengths:**
- Non-custodial (self-sovereign)
- Institutional controls
- Full DeFi access
- Audit and compliance tools

**Weaknesses:**
- Complex setup
- Requires crypto expertise
- No customer support for key loss
- High responsibility on organization

---

### 5. SolSplits (Solana Payment Splitting)

**How They Handle Payment Split:**
- **Model**: Solana-based revenue sharing
- **Type**: On-chain split contracts
- **Cost**: ~0.005 SOL per transaction
- **Features**: Automated distributions, composable

**Key Features:**
- Low-cost transactions
- Fast settlement (seconds)
- SPL token support
- NFT revenue sharing
- API and SDK available

**BACs:**
1. Create split with recipients and percentages
2. Funds automatically split on receipt
3. Recipients can withdraw anytime
4. Supports multiple tokens
5. Ongoing automated splits available (1.5% fee)

**Strengths:**
- Very low transaction costs
- Fast settlement
- Simple API
- Solana ecosystem integration

**Weaknesses:**
- Solana-only
- Requires SOL for gas
- Newer platform (less battle-tested)
- Ecosystem limitations

---

### 6. Coinbase Commerce (Crypto Payments)

**How They Handle Wallet Management:**
- **Model**: Custodial payment processing
- **Type**: Merchant receives crypto to Coinbase account
- **Custody**: Coinbase holds until withdrawal
- **Conversion**: Auto-convert to USD available

**Key Features:**
- Multiple crypto support (BTC, ETH, USDC, etc.)
- Checkout widgets and APIs
- Automatic conversion to fiat
- Invoice generation
- Accounting exports

**BACs:**
1. Customer pays to Coinbase-managed address
2. Funds held in Coinbase account
3. Merchant can withdraw to external wallet
4. Auto-convert to USD available
5. Transaction history and reporting

**Strengths:**
- Trusted brand
- Easy setup
- Automatic fiat conversion
- Good reporting

**Weaknesses:**
- Custodial (funds held by Coinbase)
- Withdrawal fees
- Limited chain support
- KYC requirements

---

## Best Practices Summary

### 1. **Flexible Split Configuration**
- Multiple recipients with custom percentages
- Support for both flat and percentage splits
- Minimum and maximum amount limits
- Override capabilities for platform fees

### 2. **Clear Wallet Custody Model**
- **Custodial**: Platform manages keys (easier UX, recoverable)
- **Non-Custodial**: User manages keys (sovereign, complex)
- **Hybrid**: User controls keys, platform provides interface

### 3. **Gas Management**
- Gas estimation before transactions
- Gas sponsorship options (platform pays)
- Multi-chain support with cost comparison
- Batch operations to save gas

### 4. **Recovery Mechanisms**
- Seed phrase backup (non-custodial)
- Social recovery (smart contract wallets)
- Platform recovery (custodial)
- Multi-sig for added security

### 5. **Compliance Integration**
- KYC/AML for large transactions
- Tax reporting (1099, etc.)
- Transaction monitoring
- Audit trails

### 6. **Multi-Chain Strategy**
- Support high-volume chains (Polygon, Base)
- Support premium chains (Ethereum)
- Support emerging chains (Solana, etc.)
- Clear cost/benefit for each

---

## Droplinked Recommendations

### Immediate Implementation

1. **Hybrid Wallet Model** (Droplinked Innovation)
   - **Non-Custodial Networks** (SKALE, Redbelly, Xion): System-managed wallets
   - **External Networks** (Polygon, Base, Ethereum): User connects MetaMask
   - Clear separation of responsibilities

2. **Payment Split Configuration** (like Stripe Connect)
   - Multiple wallet addresses per chain
   - Percentage allocation (must total 100%)
   - Validation before save
   - UI for easy management

3. **Wallet Creation Flow** (like Circle)
   - One-click wallet creation
   - Circle integration for non-custodial
   - Address display and copy functionality
   - No private key exposure

4. **TokenPay Integration**
   - Select accepted tokens per network
   - Modal interface for selection
   - Save and apply to checkout

### Future Enhancements

1. **Smart Contract Splits** (like Splits.org)
   - On-chain automatic distribution
   - Immutable split contracts
   - Gas-efficient batch distributions

2. **Gas Sponsorship**
   - Platform pays gas for merchant operations
   - Reduces friction for non-crypto users
   - Cost passed through or absorbed

3. **Multi-Sig Support**
   - Multiple approvals for large transactions
   - Role-based permissions
   - Enhanced security for merchants

4. **Auto-Conversion**
   - Convert crypto payments to stablecoins
   - Reduce volatility risk
   - Off-ramp to fiat

---

## Feature Conflicts Considerations

### Conflict with Affiliate Network
- **Issue**: Payment split between Affiliator and Co-seller
- **Solution**: Platform holds funds, distributes as credits
- **Alternative**: Smart contract split for on-chain payments

### Conflict with Shop Currency
- **Issue**: Crypto payments settle in tokens, not shop currency
- **Solution**: Credit system converts to shop currency equivalent
- **Implementation**: Real-time conversion rates at transaction time

### Conflict with On-Chain Inventory
- **Issue**: Wallet lock for recording vs. payment receiving
- **Solution**: Clear wallet purpose separation
- **Implementation**: Separate wallet tracking for different features

### Conflict with Non-Custodial Settings
- **Issue**: User confusion about custody model
- **Solution**: Clear labeling and education
- **Implementation**: Tooltips, help text, clear distinction in UI

---

## Conclusion

**Winner**: Circle for custodial simplicity + Splits.org for on-chain splitting

**Droplinked Differentiation**: Hybrid model with clear network-based custody separation, integrated with broader e-commerce features

**Key Success Factors**: Clear custody model, flexible split configuration, low gas costs, and seamless integration with checkout

**Recommended Approach**: Hybrid custody model (Circle for non-custodial networks, MetaMask for external), with platform-level payment splitting for fiat and on-chain splits for crypto-native merchants

# PDR-005: Non-Custodial Wallet Management

**Status:** Draft  
**Author:** Product Management  
**Date:** 2026-02-12  
**Version:** 1.0  
**Related:** PDR-001 (Wallet/Payment), PDR-004 (NFT Recording)

---

## 1. Executive Summary

The Non-Custodial Wallet Management system empowers merchants with full self-sovereignty over their crypto assets. Unlike custodial solutions where the platform holds funds, non-custodial wallets ensure merchants maintain complete control of their private keys, with the platform facilitating transactions but never taking possession of funds.

### Key Decisions

1. **True non-custodial**: Platform never holds private keys or funds
2. **Multi-wallet per merchant**: Different wallets for different networks and purposes
3. **Transaction signing required**: Merchant must approve every transaction
4. **Wallet verification mandatory**: Signature proof required for ownership
5. **Integration with recording**: Can use same wallet for NFT recording and payments

---

## 2. Problem Statement

### Current Pain Points

1. **Counterparty risk**: Platform holds funds in credit system
2. **Withdrawal delays**: Merchants wait for payouts
3. **Limited control**: Can't choose which wallet receives funds
4. **Security concerns**: Centralized honeypot risk
5. **Complexity**: Non-custodial seems scary to non-technical users
6. **Recovery**: What happens if merchant loses wallet access?

### User Stories

**As a crypto-native merchant**, I want to:

- Connect my own wallet and receive payments directly
- Never have funds touch the platform's custody
- Control which wallet address receives which payments
- Use hardware wallets for maximum security
- Approve each transaction myself
- Withdraw funds immediately without delays

**As a security-conscious merchant**, I want to:

- Maintain full control of my private keys
- Verify every transaction before signing
- Use multi-signature wallets for business accounts
- Have clear audit trails of all transactions

**As the platform**, we need to:

- Support major wallet types and connection methods
- Guide merchants through non-custodial setup
- Handle transaction failures gracefully
- Provide fallbacks for edge cases
- Maintain security without holding funds

---

## 3. Scope

### In Scope ✅

- **Wallet Connection**: MetaMask, WalletConnect, Coinbase Wallet
- **Multi-Network Support**: One wallet per network
- **Transaction Management**: Signing, broadcasting, monitoring
- **Balance Tracking**: Real-time balance updates
- **Security Features**: Verification signatures, 2FA for changes
- **Transaction History**: On-chain activity tracking
- **Gas Management**: Estimation, optimization
- **Integration**: With payments, NFT recording, affiliate splits

### Out of Scope ❌

- **Wallet creation**: We don't create wallets (merchant brings their own)
- **Seed phrase recovery**: Merchant responsible for backup
- **Multi-sig UI**: Support multi-sig wallets but no custom UI
- **Hardware wallet specific flows**: Generic WalletConnect support
- **Cross-chain swaps**: Use third-party bridges
- **Staking/Yield**: Focus on payments only

---

## 4. Architecture & Design

### 4.1 System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│            NON-CUSTODIAL WALLET ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  MERCHANT SIDE                                                  │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐   │
│  │   MetaMask   │     │WalletConnect │     │Coinbase      │   │
│  │   (Browser)  │     │  (Mobile)    │     │Wallet        │   │
│  └──────┬───────┘     └──────┬───────┘     └──────┬───────┘   │
│         │                    │                    │            │
│         └────────────────────┼────────────────────┘            │
│                              │                                 │
│                              ▼                                 │
│                   ┌────────────────────┐                      │
│                   │  Web3 Provider     │                      │
│                   │  (EIP-1193)        │                      │
│                   └─────────┬──────────┘                      │
│                             │                                  │
│  PLATFORM SIDE                │                                  │
│  ┌──────────────────────────┼──────────────────────────────┐  │
│  │                          ▼                              │  │
│  │  ┌────────────────────────────────────────────────────┐ │  │
│  │  │         Wallet Connection Manager                   │ │  │
│  │  │  • Connection lifecycle                            │ │  │
│  │  │  • Address validation                              │ │  │
│  │  │  • Network detection                               │ │  │
│  │  │  • Signature verification                          │ │  │
│  │  └─────────────────────┬──────────────────────────────┘ │  │
│  │                        │                                 │  │
│  │  ┌─────────────────────┼──────────────────────┐         │  │
│  │  ▼                     ▼                      ▼         │  │
│  │  ┌──────────┐  ┌──────────────┐  ┌──────────────────┐   │  │
│  │  │ Payment  │  │  NFT         │  │  Transaction     │   │  │
│  │  │ Router   │  │  Recording   │  │  Monitor         │   │  │
│  │  └──────────┘  └──────────────┘  └──────────────────┘   │  │
│  │                                                         │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                                 │
│  BLOCKCHAIN                                                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                     │
│  │Ethereum  │  │ Polygon  │  │   BSC    │                     │
│  │ Mainnet  │  │          │  │          │                     │
│  └──────────┘  └──────────┘  └──────────┘                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 Data Model

```typescript
// Non-Custodial Wallet
interface NonCustodialWallet {
  id: string;
  merchantId: string;
  
  // Wallet details
  address: string;
  networkId: string;           // 'ethereum', 'polygon', etc.
  chainId: number;
  
  // Connection method
  connection: {
    type: 'metamask' | 'walletconnect' | 'coinbase' | 'injected';
    sessionId?: string;        // For WalletConnect
    connectedAt: Date;
    lastActivity: Date;
  };
  
  // Verification
  verification: {
    message: string;           // "I verify ownership of this wallet for Droplinked..."
    signature: string;         // Signed message
    verifiedAt: Date;
    verified: boolean;
  };
  
  // Permissions
  permissions: {
    canReceivePayments: boolean;
    canRecordNFTs: boolean;
    canSignTransactions: boolean;
    isLocked: boolean;         // For recording wallets
  };
  
  // Status
  status: 'active' | 'disconnected' | 'network_mismatch' | 'error';
  
  // Security
  security: {
    requires2FAForChanges: boolean;
    lastChangedAt: Date | null;
    changeHistory: WalletChangeEvent[];
  };
}

// Wallet Change Event
interface WalletChangeEvent {
  changedAt: Date;
  changedBy: string;
  action: 'connected' | 'disconnected' | 'verified' | 'permissions_updated';
  oldValue?: any;
  newValue?: any;
  reason?: string;
}

// Pending Transaction
interface PendingTransaction {
  id: string;
  merchantId: string;
  walletAddress: string;
  networkId: string;
  
  // Transaction details
  type: 'payment_receipt' | 'nft_recording' | 'affiliate_split' | 'withdrawal';
  to: string;
  value: string;               // In wei
  data?: string;               // Contract call data
  gasLimit: number;
  gasPrice: string;
  nonce: number;
  
  // Status
  status: 'pending_signature' | 'signed' | 'broadcast' | 'mined' | 'failed';
  
  // Signatures
  signature?: string;
  signedAt?: Date;
  
  // Blockchain refs
  transactionHash?: string;
  blockNumber?: number;
  confirmations: number;
  
  // Timing
  createdAt: Date;
  expiresAt: Date;             // Signature request expires
  minedAt?: Date;
  
  // Error handling
  error?: {
    code: string;
    message: string;
    retryable: boolean;
  };
}

// Transaction Request (for merchant signature)
interface TransactionRequest {
  id: string;
  merchantId: string;
  
  // Display info
  title: string;
  description: string;
  from: string;
  to: string;
  value: string;               // Human readable
  valueUsd: number;
  
  // Gas info
  gasEstimate: {
    limit: number;
    price: string;
    totalCost: string;
    totalCostUsd: number;
  };
  
  // Network
  network: {
    id: string;
    name: string;
    chainId: number;
    currency: string;
  };
  
  // Security
  riskLevel: 'low' | 'medium' | 'high';
  warnings: string[];
  
  // Action required
  action: 'sign' | 'reject';
  expiresAt: Date;
}
```

### 4.3 Connection Flow

```
┌─────────────────────────────────────────────────────────────────┐
│              WALLET CONNECTION WORKFLOW                         │
└─────────────────────────────────────────────────────────────────┘

Step 1: Initiate connection
┌─────────────────────────────────────────────────────────────┐
│ Connect Non-Custodial Wallet                                 │
│                                                              │
│ Choose network:                                             │
│ (•) Ethereum Mainnet                                        │
│ ( ) Polygon                                                │
│ ( ) BSC                                                    │
│                                                              │
│ Choose wallet:                                              │
│ [MetaMask] [WalletConnect] [Coinbase Wallet]               │
│                                                              │
│ ℹ️ You will maintain full control of your funds.           │
│ ℹ️ Platform never holds your private keys.                 │
└─────────────────────────────────────────────────────────────┘

Step 2: Wallet approval
┌─────────────────────────────────────────────────────────────┐
│ MetaMask Request                                             │
│                                                              │
│ Droplinked wants to connect to your wallet.                 │
│                                                              │
│ Account: 0x742d...9f2a                                      │
│ Balance: 1.45 ETH                                           │
│ Network: Ethereum Mainnet                                   │
│                                                              │
│ Permissions requested:                                      │
│ • View wallet address                                       │
│ • Request transaction signatures                           │
│                                                              │
│ [Cancel]              [Connect]                             │
└─────────────────────────────────────────────────────────────┘

Step 3: Ownership verification
┌─────────────────────────────────────────────────────────────┐
│ Verify Wallet Ownership                                      │
│                                                              │
│ To verify you own this wallet, please sign this message:    │
│                                                              │
│ ┌───────────────────────────────────────────────────────┐  │
│ │ Droplinked Wallet Verification                        │  │
│ │                                                       │  │
│ │ Wallet: 0x742d...9f2a                                 │  │
│ │ Timestamp: 2026-02-12T10:30:00Z                       │  │
│ │ Nonce: 123456789                                      │  │
│ └───────────────────────────────────────────────────────┘  │
│                                                              │
│ [Sign Message]                                              │
│                                                              │
│ ℹ️ This proves you own the wallet. No funds will be moved. │
└─────────────────────────────────────────────────────────────┘

Step 4: Confirmation
┌─────────────────────────────────────────────────────────────┐
│ ✅ Wallet Connected Successfully!                            │
│                                                              │
│ Network: Ethereum Mainnet                                   │
│ Address: 0x742d...9f2a                                      │
│ Balance: 1.45 ETH (~$4,350)                                │
│                                                              │
│ This wallet will receive:                                   │
│ • Direct crypto payments from customers                    │
│ • Affiliate commissions (if applicable)                    │
│ • NFT royalties (if applicable)                            │
│                                                              │
│ [View Wallet] [Change Wallet] [Disconnect]                 │
└─────────────────────────────────────────────────────────────┘
```

### 4.4 Transaction Signing Flow

```
┌─────────────────────────────────────────────────────────────────┐
│            TRANSACTION SIGNING FLOW                             │
└─────────────────────────────────────────────────────────────────┘

Scenario: Customer paid with crypto, funds need to be transferred

Step 1: System prepares transaction
┌─────────────────────────────────────────────────────────────┐
│ Transaction Prepared for Signing                             │
│                                                              │
│ You have a new payment to receive!                          │
│                                                              │
│ From: Smart Contract 0xdef...789                            │
│ Amount: 0.5 ETH (~$1,500)                                   │
│                                                              │
│ This is an affiliate order split:                           │
│ • Your share: 0.425 ETH                                     │
│ • Co-seller commission: 0.075 ETH                          │
│                                                              │
│ Gas fee: 0.001 ETH (~$3)                                    │
│ Net amount: 0.424 ETH (~$1,497)                            │
└─────────────────────────────────────────────────────────────┘

Step 2: Signature request
┌─────────────────────────────────────────────────────────────┐
│ MetaMask Signature Request                                   │
│                                                              │
│ Droplinked requests transaction signature.                  │
│                                                              │
│ ┌───────────────────────────────────────────────────────┐  │
│ │ Transaction Details                                    │  │
│ │                                                       │  │
│ │ From: 0x742d...9f2a (Your wallet)                     │  │
│ │ To: Smart Contract                                    │  │
│ │ Value: 0 ETH                                          │  │
│ │ Gas Limit: 100,000                                    │  │
│ │ Max Fee: 0.001 ETH                                    │  │
│ │                                                       │  │
│ │ Function: claimPayment(orderId=12345)                │  │
│ └───────────────────────────────────────────────────────┘  │
│                                                              │
│ [Reject]              [Confirm]                             │
└─────────────────────────────────────────────────────────────┘

Step 3: Transaction broadcast
┌─────────────────────────────────────────────────────────────┐
│ Transaction Submitted                                        │
│                                                              │
│ Hash: 0xabc...123                                           │
│ Status: Pending...                                          │
│                                                              │
│ [View on Etherscan]                                         │
│                                                              │
│ ⏳ Awaiting confirmation...                                 │
└─────────────────────────────────────────────────────────────┘

Step 4: Confirmation
┌─────────────────────────────────────────────────────────────┐
│ ✅ Transaction Confirmed!                                    │
│                                                              │
│ Amount received: 0.424 ETH                                  │
│ Transaction: 0xabc...123                                    │
│ Block: #18,234,567                                          │
│ Confirmations: 12                                           │
│                                                              │
│ Your new balance: 1.869 ETH                                 │
│                                                              │
│ [View Transaction] [Back to Dashboard]                      │
└─────────────────────────────────────────────────────────────┘
```

---

## 5. Conflict Resolution

### Conflict 1: Wallet Disconnection During Active Transactions

**Scenario**: Merchant has pending payments. Disconnects wallet. New payments arrive.

**Resolution**:
```javascript
// When wallet disconnected
function onWalletDisconnect(wallet) {
  wallet.status = 'disconnected';
  
  // Queue incoming payments
  incomingPayments = getPendingPaymentsForWallet(wallet.address);
  
  FOR payment IN incomingPayments:
    payment.status = 'queued_wallet_disconnected';
    payment.queuedAt = now();
    
    // Notify merchant
    notify_merchant(
      "You have ${incomingPayments.length} pending payments. " +
      "Please reconnect your wallet to receive funds."
    );
  
  // Payments held in smart contract escrow
  // Merchant can claim anytime by reconnecting
}

// When wallet reconnected
function onWalletReconnect(wallet) {
  queuedPayments = getQueuedPayments(wallet.address);
  
  IF queuedPayments.length > 0:
    show_modal(
      "You have ${queuedPayments.length} payments to claim",
      queuedPayments,
      action: 'claim_all'
    );
}
```

### Conflict 2: Insufficient Gas Balance

**Scenario**: Merchant needs to sign transaction but wallet has 0 ETH for gas.

**Resolution**:
```javascript
// Before showing signature request
async function validateGasBalance(wallet, networkId) {
  const balance = await getWalletBalance(wallet.address, networkId);
  const estimatedGas = await estimateTransactionGas(networkId);
  
  IF balance < estimatedGas:
    return {
      valid: false,
      error: 'INSUFFICIENT_GAS',
      currentBalance: balance,
      requiredBalance: estimatedGas,
      solutions: [
        {
          label: 'Add funds to wallet',
          action: 'show_funding_options'
        },
        {
          label: 'Use platform credit instead',
          action: 'switch_to_credit',
          condition: hasCreditBalance(merchant)
        }
      ]
    };
  
  return { valid: true };
}

// Gas funding options
function showGasFundingOptions(networkId) {
  return {
    options: [
      {
        method: 'onramp',
        providers: ['moonpay', 'wyre', 'transak'],
        description: 'Buy ETH directly with credit card'
      },
      {
        method: 'bridge',
        description: 'Transfer from another wallet'
      },
      {
        method: 'faucet',
        available: networkId !== 'ethereum', // Only testnets/L2s
        description: 'Free test ETH'
      }
    ]
  };
}
```

### Conflict 3: Network Mismatch

**Scenario**: Merchant wallet connected to Ethereum. Customer pays on Polygon.

**Resolution**:
```javascript
// Customer checkout
function validatePaymentNetwork(customerNetwork, merchant) {
  const merchantWallet = merchant.wallets[customerNetwork];
  
  IF !merchantWallet:
    // Check if merchant has wallet on different network
    availableNetworks = Object.keys(merchant.wallets);
    
    return {
      canProceed: false,
      reason: 'NETWORK_MISMATCH',
      customerNetwork: customerNetwork,
      merchantNetworks: availableNetworks,
      solutions: [
        {
          label: 'Switch to ${availableNetworks[0]}',
          action: 'prompt_network_switch'
        },
        {
          label: 'Pay with different method',
          action: 'show_alternatives'
        }
      ]
    };
  
  return { canProceed: true };
}

// At checkout UI
IF networkMismatch:
  show_message(
    "⚠️ This merchant accepts payments on ${merchantNetworks.join(', ')}",
    "Please switch your wallet network or choose another payment method."
  );
```

### Conflict 4: Lost Wallet Access

**Scenario**: Merchant loses private key/seed phrase. Wallet has pending funds.

**Resolution**:
```
// Prevention first
ON wallet_connection:
  show_warning(
    "IMPORTANT: You are connecting a non-custodial wallet.",
    "• Platform CANNOT recover your wallet if you lose access",
    "• Write down your seed phrase and store it safely",
    "• Consider using a hardware wallet for large amounts",
    "• We recommend keeping a backup wallet connected"
  );

// If lost
IF merchant_reports_lost_access:
  // Platform CANNOT help recover funds
  // Funds are on blockchain, not platform custody
  
  response = {
    empathy: "We understand this is frustrating.",
    explanation: "Non-custodial wallets mean you control the keys. " +
                 "Without the private key or seed phrase, no one can " +
                 "access the funds - not even us.",
    options: [
      "Check if you have any backups",
      "Contact your wallet provider (MetaMask, etc.)",
      "Connect a new wallet for future payments",
      "If funds were significant, consult a blockchain forensics expert"
    ],
    prevention: "Always back up your seed phrase offline in multiple secure locations."
  };
```

---

## 6. Integration Points

### 6.1 Payment Processing (PDR-001)

```javascript
// Payment routing decision
function routePayment(order, merchant) {
  const paymentMethod = order.paymentMethod;
  
  IF paymentMethod === 'crypto':
    const networkId = order.cryptoNetwork;
    const wallet = merchant.wallets[networkId];
    
    IF wallet && wallet.permissions.canReceivePayments:
      // Non-custodial direct settlement
      return {
        method: 'non_custodial_direct',
        destination: wallet.address,
        requiresSignature: true,
        contract: 'PaymentRouter'
      };
    ELSE:
      // Fall back to credit system
      return {
        method: 'platform_credit',
        destination: 'platform_escrow',
        requiresSignature: false
      };
}
```

### 6.2 NFT Recording (PDR-004)

```javascript
// Recording wallet can also be non-custodial payment wallet
function setupWalletForRecording(wallet) {
  // Recording requires wallet lock
  wallet.permissions.canRecordNFTs = true;
  wallet.permissions.isLocked = true;
  wallet.lockReason = 'recording';
  
  // Also enable payments if not already
  IF !wallet.permissions.canReceivePayments:
    wallet.permissions.canReceivePayments = true;
  
  return wallet;
}

// Check if wallet suitable for recording
function validateWalletForRecording(wallet) {
  IF wallet.permissions.canRecordNFTs:
    return { valid: true };
  
  IF wallet.permissions.isLocked && !wallet.permissions.canRecordNFTs:
    return {
      valid: false,
      error: 'Wallet locked for other purposes',
      solution: 'Use a different wallet or network'
    };
  
  return { valid: true }; // Can enable recording
}
```

### 6.3 Affiliate Network (PDR-003)

```javascript
// Affiliate split with non-custodial wallets
async function processAffiliateSplit(order, affiliator, coSeller) {
  const affiliatorWallet = affiliator.wallets[order.networkId];
  const coSellerWallet = coSeller.wallets[order.networkId];
  
  // Both need non-custodial wallets for direct split
  IF affiliatorWallet?.permissions.canReceivePayments && 
     coSellerWallet?.permissions.canReceivePayments:
    
    // Use smart contract for trustless split
    const tx = await affiliateRouter.splitPayment({
      orderId: order.id,
      affiliatorWallet: affiliatorWallet.address,
      coSellerWallet: coSellerWallet.address,
      affiliatorShare: order.affiliatorShare,
      coSellerShare: order.coSellerShare,
      network: order.networkId
    });
    
    // Both merchants sign their respective claims
    return {
      method: 'non_custodial_split',
      transaction: tx,
      requiresSignatures: [affiliatorWallet.address, coSellerWallet.address]
    };
  ELSE:
    // Fall back to platform-mediated split
    return {
      method: 'platform_split',
      platformReceives: order.total,
      platformDistributes: true
    };
}
```

---

## 7. Security Architecture

### 7.1 Security Layers

```
┌─────────────────────────────────────────────────────────────────┐
│                    SECURITY LAYERS                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Layer 1: Wallet Level                                          │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │ • Hardware wallet support (via WalletConnect)             │ │
│  │ • Biometric authentication (mobile wallets)               │ │
│  │ • Private key never leaves merchant device                │ │
│  │ • Seed phrase backup (merchant responsibility)            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Layer 2: Connection Level                                      │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │ • HTTPS only                                              │ │
│  │ • CSP headers prevent XSS                                 │ │
│  │ • CORS restrictions on wallet APIs                        │ │
│  │ • Session timeout (disconnect after 30 min inactivity)   │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Layer 3: Transaction Level                                     │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │ • Merchant must sign EVERY transaction                    │ │
│  │ • Clear transaction preview before signing                │ │
│  │ • Risk warnings for unusual transactions                  │ │
│  │ • Gas estimation to prevent failed txs                    │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Layer 4: Platform Level                                        │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │ • Wallet address verification (signature proof)           │ │
│  │ • Rate limiting on connection attempts                    │ │
│  │ • IP logging for suspicious activity                      │ │
│  │ • 2FA required for wallet changes                         │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 7.2 Transaction Warnings

```javascript
const TRANSACTION_WARNINGS = {
  HIGH_GAS: {
    condition: gasCost > (transactionValue * 0.1),
    level: 'warning',
    message: 'Gas fee is >10% of transaction value. Consider waiting for lower gas prices.'
  },
  
  UNKNOWN_CONTRACT: {
    condition: !isKnownContract(transaction.to),
    level: 'high',
    message: 'Interacting with unknown smart contract. Verify address before signing.'
  },
  
  LARGE_AMOUNT: {
    condition: transactionValueUsd > 10000,
    level: 'warning',
    message: 'Large transaction amount. Double-check all details before confirming.'
  },
  
  SUSPICIOUS_METHOD: {
    condition: isSuspiciousMethod(transaction.data),
    level: 'critical',
    message: '⚠️ This transaction may transfer all your tokens. DO NOT SIGN unless you understand exactly what this does.',
    requiresConfirmation: true
  },
  
  NETWORK_MISMATCH: {
    condition: walletNetwork !== transactionNetwork,
    level: 'critical',
    message: 'Your wallet is on wrong network. Switch to ${transactionNetwork} before signing.',
    blocking: true
  }
};
```

---

## 8. User Experience

### 8.1 Wallet Dashboard

```
┌─────────────────────────────────────────────────────────────┐
│  👛 Non-Custodial Wallets                                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Connected Wallets                                           │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Ethereum Mainnet                                      │  │
│  │  Address: 0x742d...9f2a                                 │  │
│  │  Balance: 1.45 ETH (~$4,350)                           │  │
│  │  Status: ✅ Active (MetaMask)                          │  │
│  │  Last activity: 2 min ago                              │  │
│  │  [View on Etherscan]  [Disconnect]                     │  │
│  └───────────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Polygon                                              │  │
│  │  Address: 0x742d...9f2a                                 │  │
│  │  Balance: 450 MATIC (~$225)                            │  │
│  │  Status: ✅ Active (WalletConnect)                     │  │
│  │  [View on Polygonscan]  [Disconnect]                   │  │
│  └───────────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  BSC                                                  │  │
│  │  Not connected                                         │  │
│  │  [Connect Wallet]                                      │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  ⚠️ Security Recommendations                                 │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  ✓ Your wallets are verified                           │  │
│  │  ✓ 2FA enabled for wallet changes                      │  │
│  │  ⚠️ Consider backing up your seed phrases              │  │
│  │  ℹ️ Platform cannot recover lost wallets               │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  Recent Transactions                                         │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Received: 0.425 ETH from Affiliate Split             │  │
│  │  5 min ago • Confirmed ✅                              │  │
│  │  [View Details]                                        │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  [+ Add Wallet]                                              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 8.2 Transaction History

```
┌─────────────────────────────────────────────────────────────┐
│  📜 Transaction History                                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Filter: [All Networks ▼] [All Types ▼] [Date Range ▼]     │
│                                                              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Received Payment                                      │  │
│  │  +0.425 ETH (~$1,275)                                 │  │
│  │  From: Affiliate Smart Contract                       │  │
│  │  Network: Ethereum • Feb 12, 2026, 10:30 AM          │  │
│  │  Tx: 0xabc...123 • 12 confirmations                   │  │
│  │  [View on Etherscan] [Download Receipt]               │  │
│  └───────────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  NFT Recording                                         │  │
│  │  -0.005 ETH (Gas fee)                                 │  │
│  │  Token ID: #12345 on Polygon                          │  │
│  │  Feb 10, 2026, 2:15 PM                                │  │
│  │  [View NFT] [View Transaction]                        │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  [Load More]                                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 9. Implementation Roadmap

### Phase 1: Core Connection (Weeks 1-4)

- [ ] MetaMask integration
- [ ] WalletConnect integration
- [ ] Connection lifecycle management
- [ ] Basic transaction signing

### Phase 2: Multi-Network (Weeks 5-8)

- [ ] Network switching
- [ ] Multi-wallet support
- [ ] Balance tracking
- [ ] Transaction monitoring

### Phase 3: Security & UX (Weeks 9-12)

- [ ] Wallet verification flow
- [ ] Transaction warnings
- [ ] 2FA for changes
- [ ] Security audit

### Phase 4: Advanced Features (Weeks 13-16)

- [ ] Hardware wallet support
- [ ] Gas optimization
- [ ] Batch transactions
- [ ] Recovery procedures

### Phase 5: Optimization (Weeks 17-20)

- [ ] Performance tuning
- [ ] Mobile optimization
- [ ] Documentation
- [ ] Support tools

---

## 10. Success Metrics

1. **Adoption Rate**: >40% of crypto-accepting merchants use non-custodial
2. **Connection Success**: >95% successful wallet connections
3. **Transaction Success**: >98% signed transactions succeed
4. **Security Incidents**: 0 lost funds due to platform error
5. **Support Tickets**: <5% of merchants need support for wallet issues

---

## 11. Open Questions

1. Should we subsidize gas fees for new merchants?
2. How do we handle ERC-20 token approvals efficiently?
3. Do we need social recovery options (guardians)?
4. Should we support account abstraction (ERC-4337)?
5. How do we handle hardware wallet firmware updates?

---

*Last Updated: 2026-02-12*  
*Next Review: After Phase 1 completion*

# Crypto Payment Page - Web3 Payment Flow

## 

### Feature ID:

**[CHK-PAYMENT-WEB3-405]**

### Title:

**Web3 Native Token Payment Flow**

### Category:

**Checkout Domain** | **Payment** | **Blockchain**

### Status:

**Defined**

### Owner:

Behdad

---

## 1) **Summary**

### Problem/Value:

Customers need a secure, dedicated interface for cryptocurrency payments that handles wallet connections, token selection, multi-chain support, and real-time price updates. This Web3 Payment Page replaces the previous in-checkout dropdown to properly manage complex blockchain interactions including wallet extensions, network switching, token approvals, and dynamic conversion rates.

### Desired Outcome:

- Navigate to dedicated **Web3 Payment Page** when "Crypto" is selected in Checkout
- **Step 1: Wallet Connection** – Display supported wallets (MetaMask, Trust Wallet, Phantom) with installation detection
- **Step 2: Network Selection** – Select network group (EVMs or Solana) after wallet connection
- **Step 3: Token Selection** – Show Total in USD and Token amount; searchable modal for Token-Network pairs
- **Step 4: Wallet Management** – View connected wallet, copy address, disconnect, or switch wallets
- **Step 5: Payment Execution** – Real-time price updates every 15 seconds; handle balance checks and conversion rate changes
- **Step 6: Transaction Processing** – Loading states, wallet signature prompts, success/failure handling
- **Feedback:** Success redirects to Order Confirmed; failures remain on page with retry option

---

## 2) **Scope – In / Out**

### **In Scope:**

- **Wallet Support:** MetaMask, Trust Wallet, Phantom (with extension detection)
- **Network Groups:** EVM chains (Ethereum, Polygon, BSC, Base, etc.) and Solana
- **Installation Flow:** Detect extension presence; show install links for Chrome/Firefox if missing
- **Network Selection:** Group selection (EVMs/Solana) before token selection
- **Token Display:** Dual pricing (USD + Token amount) with real-time updates every 15 seconds
- **Token Picker:** Searchable modal with Token-Network pairs based on merchant activation
- **Wallet Actions:** Connect, disconnect, copy address, view connected wallets list
- **Balance Validation:** Check sufficient balance; disable pay button if insufficient
- **Conversion Rate Monitoring:** Alert if rate changes significantly affecting payment ability
- **Loading States:** Processing modal with retry option if extension doesn't open
- **Auto-close:** Modal closes automatically after 5 seconds or manual close

### **Out of Scope:**

- Fiat on-ramp (buying crypto with card)
- Token swapping/converting
- Multi-signature wallets
- Hardware wallet direct integration

---

## 3) **Key User Journeys**

### **Journey 1: Successful Payment (Happy Path)**

1. Customer on Checkout → selects "Crypto" → navigates to **Web3 Payment Page**
2. Page displays: **Total $10.50 USD** + **Connect Wallet** button
3. Customer clicks **Connect Wallet** → Wallet Selection Modal opens
4. Modal shows: MetaMask, Trust Wallet, Phantom
5. Customer selects **MetaMask** → Network Group Selection appears (EVMs / Solana)
6. Customer selects **EVMs** → Modal shows loading state
7. MetaMask extension opens → Customer approves connection
8. Modal shows **Success** message → auto-closes after 5 seconds
9. Page refreshes automatically → shows connected state:
   - Total: **$10.50 USD** | **25 USDC**
   - Connected wallet icon + truncated address
10. Customer clicks token area → **Token Selection Modal** opens
11. Modal lists available Token-Network pairs (searchable)
12. Customer selects **USDC - Polygon** → Modal closes → Token amount updates
13. Customer clicks **Pay** → **Processing Modal** opens (loading state)
14. MetaMask opens for signature → Customer approves
15. System waits for blockchain confirmation → Webhook confirms success
16. Modal shows **Success** → auto-closes → Redirect to **Order Confirmed** page

---

### **Journey 2: Wallet Extension Not Installed**

1. Customer on Web3 Payment Page → clicks **Connect Wallet**
2. Wallet Selection Modal opens → Customer clicks **Trust Wallet**
3. System detects extension **NOT installed**
4. Modal updates to **Install Extension** view:
   - Links: Chrome Web Store, Firefox Add-ons
   - Instructions on how to install
5. Customer installs extension → returns to page → refreshes
6. Customer clicks **Trust Wallet** again → Shows **Connect** option
7. Proceed with normal connection flow

---

### **Journey 3: Network Selection & Connection**

1. Customer selects wallet (e.g., MetaMask) from modal
2. Network Group Selection appears:
   - **EVMs** (Ethereum, Polygon, BSC, Base, etc.)
   - **Solana**
3. Customer selects **EVMs** → Modal enters loading state
4. Extension opens automatically for connection approval
5. If extension doesn't open → Customer clicks **Retry** button
6. After approval → Success message → Modal auto-closes
7. Page refreshes showing connected wallet

---

### **Journey 4: Insufficient Balance**

1. Customer connected with valid wallet
2. Page displays: Total $10.50 USD | 25 USDC
3. Customer clicks **Pay**
4. System validates wallet balance
5. **Error Message:** "Your wallet balance is insufficient to complete this transaction. Top up or connect a different wallet."
6. Pay button **disabled** → Customer can switch wallet or add funds

---

### **Journey 5: Conversion Rate Change**

1. Customer on payment page with token selected
2. Every **15 seconds** → Conversion rate updates automatically
3. Token amount recalculates based on current rate
4. If rate changes significantly causing insufficient balance:
   - **Error Message:** "Your balance for the selected token is no longer sufficient. Choose another."
   - Pay button disabled until token changed

---

### **Journey 6: Wallet Doesn't Support Shop Tokens**

1. Customer connects wallet that has no supported tokens
2. Page displays error: **"No supported tokens found in this wallet. Switch to a different wallet."**
3. Pay button **disabled**
4. Customer can:
   - Click wallet area → Open connected wallets modal
   - Disconnect current wallet
   - Connect different wallet with supported tokens

---

### **Journey 7: Transaction Failed/Rejected**

1. Customer clicks **Pay** → Processing Modal opens
2. Wallet extension opens for signature
3. Customer **rejects** transaction in wallet (or on-chain failure)
4. Modal shows **Failure** message
5. Modal closes (manually or auto after 5 seconds)
6. Customer **remains on Web3 Payment Page**
7. Customer can:
   - Retry payment by clicking **Pay** again
   - Change token/network
   - Go back to Checkout to select different payment method

---

### **Journey 8: Switch Token**

1. Customer on connected payment view
2. Clicks on token display (e.g., "25 USDC")
3. **Token Selection Modal** opens
4. Customer uses **search** to find token or network
5. Selects new Token-Network pair (e.g., "ETH - Ethereum")
6. Modal closes → Display updates with recalculated token amount
7. Price updates based on current conversion rate

---

### **Journey 9: Wallet Management**

1. Customer clicks on **connected wallet display** (icon + address)
2. **Wallet Modal** opens showing:
   - Connected wallet: Icon + Name + Truncated Address
   - Actions: Copy Address, Disconnect, Connect New Wallet
3. Customer clicks **Copy Address** → Address copied to clipboard
4. Customer clicks **Disconnect** → Wallet disconnected → Page refreshes to initial state
5. To change wallet address: Customer changes account in extension → Refreshes page

---

### **Journey 10: Pending Until Webhook/Timeout**

1. Customer approves transaction in wallet
2. Transaction broadcast to blockchain
3. Page shows **Processing** state in modal
4. Order status = **"Pending"**
5. System waits for webhook confirmation:
   - **Success:** Order status → "Paid" → Redirect to Order Confirmed
   - **Timeout:** Error message "Transaction timeout. Please check your wallet or try again." → Remain on page → Can retry

---

## 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** Web3 Payment Page must be a dedicated page, separate from main checkout
- **BAC 2:** Supported wallets displayed: MetaMask, Trust Wallet, Phantom
- **BAC 3:** System must detect wallet extension installation:
  - Installed → Show "Connect" option
  - NOT installed → Show install links for Chrome/Firefox
- **BAC 4:** After wallet selection, display Network Group selection (EVMs / Solana)
- **BAC 5:** Connection modal must show loading state and retry option if extension doesn't open
- **BAC 6:** Modal auto-closes after 5 seconds on success/failure (or manual close)
- **BAC 7:** Dual pricing must always display: Fiat (USD) + Token Amount
- **BAC 8:** Token-Network pairs must be displayed in searchable modal based on merchant activation
- **BAC 9:** Conversion rates must update every 15 seconds with automatic token amount recalculation
- **BAC 10:** Pay button must be disabled with error message if:
  - Wallet has no supported tokens
  - Insufficient balance
  - Conversion rate change makes balance insufficient
- **BAC 11:** Wallet management modal must allow: Copy address, Disconnect, Connect new wallet
- **BAC 12:** Clicking Pay must trigger loading modal → wallet signature → wait for webhook
- **BAC 13:** On success: Auto-close modal → redirect to Order Confirmed page
- **BAC 14:** On failure: Show error → remain on Web3 Payment Page → allow retry
- **BAC 15:** Order remains "Pending" until webhook confirms success, failure, or timeout

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-14 | Behdad | Complete rewrite with new Web3 flow: network groups, real-time rates, enhanced wallet management | Updated requirements |
| 2026-02-01 | Behdad | Rewrote AI-Centric Layer without technical implementation details | SSoT business focus |
| 2026-01-31 | Behdad | Initial version for dedicated Crypto Payment Page | Checkout simplification |

---

---

# [PART 2: AI-CENTRIC LAYER – Functional Logic & Rules]

---

## **B1) Definitions & Glossary**

| Term | Definition |
| --- | --- |
| **Web3 Payment Page** | Dedicated page for completing cryptocurrency payments with wallet integration |
| **Network Group** | Category of blockchains: EVMs (Ethereum Virtual Machine chains) or Solana |
| **Token-Network Pair** | Combination of cryptocurrency + blockchain (e.g., "USDC - Polygon") |
| **Dual Pricing** | Display of both fiat amount ($10.50 USD) and crypto equivalent (25 USDC) |
| **Conversion Rate** | Real-time exchange rate between USD and selected cryptocurrency |
| **Extension Detection** | Checking browser for installed wallet extensions (window.ethereum, window.solana, etc.) |
| **Webhook** | Backend notification confirming blockchain payment success/failure |

---

## **B2) Detailed Functional Rules**

### **R1. Page Structure & Flow**

- Web3 Payment Page is **separate** from main checkout
- Customer arrives after selecting "Crypto" on checkout
- Page flow: Connect Wallet → Select Network Group → Token Selection → Pay

### **R2. Wallet Selection Modal Rules**

| Wallet | Detection Method | Not Installed | Installed |
| --- | --- | --- | --- |
| MetaMask | window.ethereum | Show Chrome + Firefox install links | Show "Connect" + Network Group selection |
| Trust Wallet | window.ethereum/trust | Show Chrome + Firefox install links | Show "Connect" + Network Group selection |
| Phantom | window.solana | Show Chrome + Firefox install links | Show "Connect" + Network Group selection |

### **R3. Network Group Selection**

- After wallet selection → Display Network Group options:
  - **EVMs:** Ethereum, Polygon, BSC, Base, SKALE, Bitlayer, XION, XRPLSidechain
  - **Solana:** Solana mainnet
- Customer selects group → Modal enters loading state
- Extension automatically opens for connection approval

### **R4. Connection Flow**

- IF extension not installed:
  - Show install links for Chrome Web Store and Firefox Add-ons
  - Include brief installation instructions
  - After install + refresh → Return to wallet selection
- IF extension installed:
  - Show "Connect" button
  - After network group selection → Extension prompts for approval
  - On approval → Success message → Auto-close modal (5s)
  - On rejection → Error message → Retry option

### **R5. Connected State Display**

After connection, display:
- **Total USD:** $10.50 USD
- **Token Amount:** 25 USDC (clickable to change)
- **Connected Wallet:** Icon + Truncated Address (clickable for actions)
- **Pay Button:** Initiates payment (disabled if validation fails)

### **R6. Token Selection Modal Rules**

- Opens when customer clicks token display area
- Shows all **Token-Network pairs** activated by merchant
- Format: "[Token] - [Network]" (e.g., "USDC - Polygon")
- **Searchable:** By token name OR network name
- Selection → Modal closes → Token amount recalculates

### **R7. Real-Time Price Updates**

- Conversion rates refresh **every 15 seconds**
- Token amount recalculates automatically
- IF new rate makes balance insufficient:
  - Display error: "Your balance for the selected token is no longer sufficient. Choose another."
  - Disable Pay button

### **R8. Wallet Management Modal**

- Opens when customer clicks wallet display area
- Shows connected wallet: Icon + Name + Truncated Address
- Actions available:
  - **Copy Address:** Copy full wallet address to clipboard
  - **Disconnect:** Remove wallet connection → Page refreshes
  - **Connect New Wallet:** Return to wallet selection modal
- To change wallet address: Change in extension → Refresh page

### **R9. Validation Rules**

| Validation | Condition | Result |
| --- | --- | --- |
| No Supported Tokens | Wallet has none of merchant's activated tokens | Error: "No supported tokens found in this wallet. Switch to a different wallet." + Disable Pay |
| Insufficient Balance | Wallet balance < Required amount | Error: "Your wallet balance is insufficient to complete this transaction. Top up or connect a different wallet." + Disable Pay |
| Rate Change Impact | New rate makes balance insufficient | Error: "Your balance for the selected token is no longer sufficient. Choose another." + Disable Pay |

### **R10. Pay Now Execution Rules**

1. Customer clicks **Pay**
2. Validate balance and token support (disable if failed)
3. Open **Processing Modal** with loading state
4. Trigger wallet extension for signature
5. IF extension doesn't open → Show **Retry** button
6. Customer approves in wallet → Transaction broadcast
7. Order status = **"Pending"**
8. Wait for webhook confirmation:
   - **Success:** Modal shows success → Auto-close (5s) → Redirect to Order Confirmed
   - **Failure:** Modal shows error → Close → Remain on page
   - **Timeout:** Error: "Transaction timeout. Please check your wallet or try again."

---

## **B3) UI/UX States & Triggers**

| State | Customer Sees | Trigger |
| --- | --- | --- |
| **Initial** | Total USD + Connect Wallet button | Page load (no wallet connected) |
| **Wallet Modal Open** | List of wallets: MetaMask, Trust Wallet, Phantom | Click Connect Wallet |
| **Extension Missing** | Install links (Chrome + Firefox) + Instructions | Wallet selected, extension not found |
| **Network Selection** | EVMs / Solana options | Wallet selected, extension found |
| **Connecting** | Loading spinner + "Opening wallet..." | Network group selected |
| **Connection Failed** | Error message + Retry button | Extension didn't open after timeout |
| **Connected** | Dual pricing, token, wallet info, Pay button | Connection approved |
| **Token Modal Open** | Searchable Token-Network pairs list | Click token display |
| **Wallet Modal Open** | Connected wallet + Copy/Disconnect/Connect New | Click wallet display |
| **Processing Payment** | Loading modal: "Processing transaction..." | Click Pay |
| **Success** | Success message → Auto-close → Redirect | Webhook confirms success |
| **Error** | Error message + Close button | Transaction fails/rejected/timeout |

---

## **B4) Edge Cases & Error Handling**

| Edge Case | Expected Behavior |
| --- | --- |
| Wallet extension not installed | Show Chrome Web Store + Firefox Add-ons links with instructions |
| Customer rejects connection | Show "Connection rejected" error, allow retry |
| Customer rejects transaction | Show "Transaction rejected" error, remain on page, allow retry |
| Extension doesn't open automatically | Show Retry button in loading modal |
| Wrong network selected | Prompt to switch network in wallet extension |
| Insufficient token balance | Disable Pay button, show insufficient balance message |
| Conversion rate increases beyond balance | Show rate change error, disable Pay until token changed |
| Wallet has no supported tokens | Show "No supported tokens" error, disable Pay |
| Transaction timeout (webhook) | Show timeout error, remain on page, allow retry |
| Wallet disconnects mid-flow | Detect disconnect, refresh page to initial state |
| Customer closes browser during payment | Order stays "Pending", customer can return and check status |
| Customer clicks Back button | Return to checkout, can select different payment method |

---

## **B5) Token-Network Pairs Reference**

| Network | Available Tokens |
| --- | --- |
| Polygon | USDC |
| Binance (BSC) | BNB, USDC, USDT |
| SKALE | USDC |
| Bitlayer | USDC |
| Ethereum | ETH, USDC, USDT |
| XION | BNB |
| Base | ETH |
| XRPLSidechain | USDT, ETH |
| Solana | SOL, USDC |

*Configurable by merchant in Shop Builder settings*

---

## **B6) Business Logic Decision Tree**

```
CUSTOMER ON WEB3 PAYMENT PAGE
│
├──► Display: Total USD + Connect Wallet button
│
├──► Click Connect Wallet
│     │
│     └──► Wallet Selection Modal Opens
│           │
│           ├──► MetaMask / Trust Wallet / Phantom
│                 │
│                 ├──► NOT Installed
│                 │     └──► Show Install Links (Chrome + Firefox)
│                 │           └──► After Install + Refresh → Return
│                 │
│                 └──► Installed
│                       │
│                       ├──► Show Network Group Selection (EVMs / Solana)
│                       │
│                       ├──► Customer Selects Network
│                       │     └──► Loading State → Open Extension
│                       │
│                       ├──► Extension Opens (or Retry if not)
│                       │
│                       ├──► Customer Approves/Rejects
│                       │     ├──► Approved → Success Message → Auto-close (5s)
│                       │     └──► Rejected → Error → Retry
│                       │
│                       └──► Page Refreshes → Connected State
│                             │
│                             ├──► Display: Total USD + Token Amount
│                             │
│                             ├──► Token Validation
│                             │     ├──► No Supported Tokens → Error + Disable Pay
│                             │     └──► Has Supported Tokens → Enable Token Selection
│                             │
│                             ├──► Every 15s: Update Conversion Rate
│                             │     └──► Recalculate Token Amount
│                             │     └──► Check Balance Sufficiency
│                             │
│                             ├──► Click Token → Token Modal
│                             │     └──► Select Token-Network → Close → Update Display
│                             │
│                             ├──► Click Wallet → Wallet Modal
│                             │     ├──► Copy Address
│                             │     ├──► Disconnect
│                             │     └──► Connect New Wallet
│                             │
│                             └──► Click Pay
│                                   │
│                                   ├──► Validate Balance
│                                   │     ├──► Insufficient → Error + Disable
│                                   │     └──► Sufficient → Continue
│                                   │
│                                   ├──► Processing Modal (Loading)
│                                   │
│                                   ├──► Extension Opens for Signature
│                                   │     ├──► Doesn't Open → Retry Button
│                                   │     └──► Opens → Customer Approves/Rejects
│                                   │
│                                   ├──► Transaction Broadcast
│                                   │     ├──► Approved → Wait Webhook
│                                   │     │     ├──► Success → Paid → Order Confirmed
│                                   │     │     └──► Timeout → Error → Retry
│                                   │     └──► Rejected → Error → Retry
│                                   │
│                                   └──► Customer Can Go Back to Checkout
```

---

## **B7) Test Scenarios for QA**

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| **T1** | MetaMask installed, connect EVM, pay with USDC-Polygon | Success, redirect to Order Confirmed |
| **T2** | Phantom not installed, click Phantom | Show Chrome + Firefox install links |
| **T3** | Install Phantom, return, click Phantom, select Solana | Shows Connect → Network Selection → Connect |
| **T4** | Connect wallet, reject connection in popup | Show "Connection rejected", can retry |
| **T5** | Connected, change token from USDC to ETH | Token modal opens, select ETH, price recalculates |
| **T6** | Search "ETH" in token modal | Shows: ETH-Ethereum, ETH-Base, ETH-XRPLSidechain |
| **T7** | Search "Polygon" in token modal | Shows: USDC-Polygon |
| **T8** | Click Pay with insufficient balance | Error: "Insufficient balance", Pay disabled |
| **T9** | Rate changes making balance insufficient | Error: "Balance no longer sufficient", Pay disabled |
| **T10** | Wallet with no supported tokens | Error: "No supported tokens found", Pay disabled |
| **T11** | Click Pay, reject in wallet | Error: "Transaction rejected", remain on page |
| **T12** | Complete payment, webhook confirms | Order = Paid, redirect to success |
| **T13** | Complete payment, webhook timeout | Error: "Transaction timeout", can retry |
| **T14** | Click wallet display → Copy address | Address copied to clipboard |
| **T15** | Click wallet display → Disconnect | Wallet disconnected, page refreshes |
| **T16** | Wait 15 seconds on payment page | Conversion rate updates, token amount recalculates |
| **T17** | Extension doesn't open automatically | Retry button appears in loading modal |
| **T18** | Switch wallet account in extension → Refresh | New wallet address displayed |

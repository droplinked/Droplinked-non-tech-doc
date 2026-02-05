# Payment Methods Display - Native Token Payments

# Crypto Payment Page - Native Token Payments

**Feature ID:** CHK-PAYMENT-CRYPTO-405

**Category:** Checkout Domain | Payment | Blockchain

**Actors:** Customer

**Status:** Defined

**Owner:** Behdad

**Last Updated:** January 31, 2026

---

# **1) Summary**

### **Problem / Value**

Customers wishing to pay with cryptocurrency need a dedicated, secure, and user-friendly interface that handles wallet connection, token selection, and transaction verification. This dedicated **Crypto Payment Page** replaces the previous in-checkout dropdown method to properly handle the complexity of Web3 interactions (wallet extensions, token approvals, network switching, gas fees).

### **Desired Outcome**

- When "Crypto" is selected in Checkout, navigate to the dedicated **Crypto Payment Page**.
- **Step 1: Wallet Selection** – Display supported wallets (currently Phantom & MetaMask; more wallets will be added later).
- **Step 2: Wallet Connection** – Handle "Install Chrome Extension" prompt if wallet not installed; connect if installed.
- **Step 3: Token Selection** – Show Total Amount in USD and selected Token. Token-Network pairs displayed in a searchable modal.
- **Step 4: Wallet Account Selection** – Allow switching wallet accounts.
- **Step 5: Payment Execution** – "Pay Now" triggers wallet signature → Loading state → Result.
- **Feedback:** Loading states during transaction processing; success redirects to "Order Confirmed" page.
- **Fallback:** On failure/error, remain on page with error message; customer can retry or return to checkout.
- **Pending Handling:** Order remains "Pending" until webhook confirms result or timeout error occurs.

---

# **2) Scope – In / Out**

### **In Scope**

- **Wallet Support (Current):** Phantom (Solana/EVM), MetaMask (EVM).
- **Future Expansion:** Architecture supports adding more wallets later.
- **Extension Detection:** Detect `window.solana` (Phantom) or `window.ethereum` (MetaMask). Link to Chrome Web Store if missing.
- **Token Display:** Dual pricing (USD fiat amount & Token equivalent amount).
- **Token Picker Modal:**
    - Full list of supported Token-Network pairs
    - Searchable by token name or network
    - Each entry shows: Token Name – Network Name
- **Wallet Actions:** Connect, Switch Account, Pay.
- **Loading States:** After clicking Pay Now, show loading until result received.
- **Webhook Integration:** Order stays "Pending" until webhook confirms payment or timeout.

### **Out of Scope**

- **Fiat On-Ramp:** Buying crypto with card inside this flow.
- **Non-Native Tokens:** Tokens not whitelisted by the Merchant/Platform.
- **Token Swapping:** Converting one token to another before payment.
- **Multi-signature wallets.**

---

# **3) Key User Journeys**

---

## **Journey 1 – Successful Crypto Payment (Happy Path)**

**Step 1:** Customer is on Checkout page with cart total > $0.

**Step 2:** Customer selects "Crypto" payment method.

**Step 3:** Customer is navigated to the **Crypto Payment Page**.

**Step 4:** Page displays list of supported wallets: Phantom, MetaMask.

**Step 5:** Customer clicks on MetaMask.

**Step 6:** System checks if MetaMask extension is installed.

**Step 7:** MetaMask is installed → Sub-options appear: "Connect" button.

**Step 8:** Customer clicks Connect → MetaMask extension prompts for approval.

**Step 9:** Customer approves connection in MetaMask.

**Step 10:** Page updates to show connected state with payment details:

- **Total:** $10.50 USD
- **Token Amount:** 25 USDC (default token)

**Step 11:** Customer clicks on Token display area.

**Step 12:** Token Selection Modal opens showing all Token-Network pairs:

- USDC - Polygon
- USDC - Ethereum
- USDT - Ethereum
- ETH - Ethereum
- BNB - Binance
- etc.

**Step 13:** Modal has search functionality (filter by token name or network).

**Step 14:** Customer selects "USDT - Polygon".

**Step 15:** Modal closes. Price updates to show equivalent USDT amount.

**Step 16:** Customer can click wallet area to see connected accounts and switch if needed.

**Step 17:** Customer clicks **"Pay Now"** button.

**Step 18:** Page enters **Loading State** ("Processing transaction...").

**Step 19:** MetaMask prompts customer to sign/approve transaction.

**Step 20:** Customer approves in MetaMask.

**Step 21:** System waits for blockchain confirmation (order in "Pending" status).

**Step 22:** Webhook confirms successful payment.

**Step 23:** Order status updated to **"Paid"**.

**Step 24:** Customer is redirected to **Order Confirmed** page.

---

## **Journey 2 – Wallet Extension Not Installed**

**Step 1:** Customer is on Crypto Payment Page.

**Step 2:** Customer clicks on "Phantom" wallet.

**Step 3:** System detects Phantom extension is NOT installed.

**Step 4:** Under Phantom option, display: **"Install Chrome Extension"** link.

**Step 5:** Customer clicks link → Opens Chrome Web Store for Phantom extension.

**Step 6:** Customer installs extension and returns to page.

**Step 7:** Customer clicks Phantom again → Now shows "Connect" option.

**Step 8:** Proceed with normal connection flow.

---

## **Journey 3 – Transaction Failed/Rejected**

**Step 1:** Customer has connected wallet and selected token.

**Step 2:** Customer clicks "Pay Now".

**Step 3:** Page enters Loading State.

**Step 4:** Customer **rejects** transaction in wallet extension (or transaction fails on-chain).

**Step 5:** Page exits Loading State.

**Step 6:** Error message displayed: "Transaction Rejected" or "Payment Failed: [Reason]".

**Step 7:** Customer remains on Crypto Payment Page.

**Step 8:** Customer can:
- Retry by clicking "Pay Now" again
- Go back to Checkout to select a different payment method (Credit Card, PayPal, etc.)

---

## **Journey 4 – Pending Until Webhook/Timeout**

**Step 1:** Customer completes transaction signing in wallet.

**Step 2:** Transaction is broadcast to blockchain.

**Step 3:** Page shows Loading/Processing state.

**Step 4:** System waits for webhook confirmation from blockchain.

**Step 4A – Webhook confirms success:**

- Order status → "Paid"
- Redirect to Order Confirmed page

**Step 4B – Timeout (no webhook response in expected time):**

- Display error: "Transaction timeout. Please check your wallet or try again."
- Order remains in "Pending" status
- Customer can retry or contact support

---

## **Journey 5 – Switch Token**

**Step 1:** Customer is on connected payment view showing default token.

**Step 2:** Customer clicks on Token display (e.g., "25 USDC").

**Step 3:** Token Selection Modal opens.

**Step 4:** Customer uses search to find "ETH".

**Step 5:** Results show: "ETH - Ethereum", "ETH - Base", etc.

**Step 6:** Customer selects "ETH - Ethereum".

**Step 7:** Modal closes.

**Step 8:** Display updates: Token amount recalculated based on current exchange rate.

**Step 9:** Customer proceeds with payment.

---

## **Journey 6 – Switch Wallet Account**

**Step 1:** Customer is on connected payment view.

**Step 2:** Customer clicks on Wallet display area (showing connected address).

**Step 3:** List of available wallet accounts displayed.

**Step 4:** Customer selects different account or connects new one.

**Step 5:** Display updates with new account.

**Step 6:** Customer proceeds with payment.

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Crypto Payment Page must be a dedicated page/view, separate from main checkout.

### **BAC 2:** Supported wallets displayed: Phantom, MetaMask (more to be added later).

### **BAC 3:** System must detect if wallet extension is installed:

- If installed → Show "Connect" option
- If NOT installed → Show "Install Chrome Extension" link

### **BAC 4:** Token-Network pairs must be displayed in a searchable modal:

- Format: "[Token Name] - [Network Name]"
- Search by token name or network name
- Full list of supported combinations

### **BAC 5:** Dual pricing must always be displayed:

- Fiat amount (USD): "$10.50 USD"
- Token amount: "25 USDC"

### **BAC 6:** Token selection must dynamically update the displayed token amount based on current exchange rate.

### **BAC 7:** Customer must be able to switch wallet accounts from the payment interface.

### **BAC 8:** "Pay Now" button click must:

- Trigger loading state immediately
- Initiate wallet signature request
- Remain in loading until result received

### **BAC 9:** Order remains in "Pending" status until:

- Webhook confirms successful payment → Status = "Paid"
- Webhook confirms failure → Status = "Failed"
- Timeout occurs → Error displayed, status remains "Pending"

### **BAC 10:** On successful payment:

- Order status = "Paid"
- Redirect to Order Confirmed page

### **BAC 11:** On failed/rejected transaction:

- Display error message with reason
- Remain on Crypto Payment Page
- Customer can retry or go back to select different payment method

### **BAC 12:** No country-based restrictions on crypto payment availability.

### **BAC 13:** If customer navigates back to checkout, they can select a different payment method (Credit Card, PayPal, Coinbase Commerce).

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Rewrote AI-Centric Layer without technical implementation details | SSoT business focus |
| 2026-01-31 | Behdad | Complete rewrite for new dedicated Crypto Payment Page flow | Checkout simplification - new crypto UX |

---

---

# [PART 2: AI-CENTRIC LAYER – Functional Logic & Rules]

---

## **B1) Definitions & Glossary**

| Term | Definition |
| --- | --- |
| **Crypto Payment Page** | A dedicated page (separate from checkout) where customers complete cryptocurrency payments |
| **Wallet** | A browser extension (Phantom, MetaMask) that stores customer's cryptocurrency and signs transactions |
| **Token** | A specific cryptocurrency (e.g., USDC, ETH, BNB) |
| **Network** | The blockchain where the token exists (e.g., Polygon, Ethereum, Binance) |
| **Token-Network Pair** | Combination of token + network displayed together (e.g., "USDC - Polygon") |
| **Dual Pricing** | Showing both fiat amount ($10.50 USD) and crypto equivalent (25 USDC) |
| **Wallet Connection** | Linking customer's wallet extension to the payment page |
| **Webhook** | Backend notification that confirms whether blockchain payment succeeded or failed |

---

## **B2) Detailed Functional Rules**

### **R1. Page Structure & Flow**

- Crypto Payment Page is a **separate page** from main checkout
- Customer arrives here after selecting "Crypto" on checkout page
- Page flow: Wallet Selection → Connect → Token Selection → Pay

### **R2. Wallet Display Rules**

| Wallet | Detection | If Not Installed | If Installed |
| --- | --- | --- | --- |
| Phantom | Check browser | Show "Install Chrome Extension" link | Show "Connect" button |
| MetaMask | Check browser | Show "Install Chrome Extension" link | Show "Connect" button |
- More wallets will be added in future (architecture must support expansion)
- Wallets displayed as list/grid with logos
- Customer clicks wallet → system checks if installed

### **R3. Connection Flow Rules**

- IF wallet not installed:
    - Show "Install Chrome Extension" link
    - Link opens Chrome Web Store page for that wallet
    - After install, customer returns and can connect
- IF wallet installed:
    - Show "Connect" button
    - On click → Wallet extension prompts for approval
    - Customer approves in wallet
    - Page transitions to connected state

### **R4. Connected State Display**

After successful connection, show:

- **Fiat Total:** "$10.50 USD" (order total in dollars)
- **Token Amount:** "25 USDC" (equivalent in selected token)
- **Selected Token:** Icon + Amount + Symbol (clickable to change)
- **Connected Wallet:** Address (truncated, e.g., "0x12...34") (clickable to switch)
- **Pay Now Button:** Initiates payment

### **R5. Token Selection Modal Rules**

- Opens when customer clicks on token display area
- Shows complete list of all Token-Network pairs
- Format: "[Token Name] - [Network Name]"
    - Example: "USDC - Polygon", "ETH - Ethereum", "BNB - Binance"
- Has search bar to filter by token name OR network name
- Customer selects pair → Modal closes → Price recalculates with new token

### **R6. Wallet Account Switching**

- Customer clicks on wallet address area
- Shows list of available accounts in that wallet
- Customer can select different account
- Customer can disconnect and connect different wallet

### **R7. Pay Now Execution Rules**

1. Customer clicks "Pay Now"
2. Page enters **loading state** immediately
3. Wallet extension prompts customer to sign/approve transaction
4. Customer approves in wallet
5. Transaction sent to blockchain
6. Order status = **"Pending"**
7. System waits for webhook confirmation
8. **IF webhook confirms success:**
    - Order status = "Paid"
    - Redirect to Order Confirmed page
9. **IF webhook confirms failure OR timeout:**
    - Display error message
    - Customer remains on page
    - Can retry or go back to checkout

### **R8. Order Status Flow**

| Event | Order Status |
| --- | --- |
| Customer clicks Pay Now | Pending |
| Webhook confirms success | Paid |
| Webhook confirms failure | Failed |
| Timeout (no webhook response) | Pending (with error shown) |
| Customer rejects in wallet | No change (can retry) |

---

## **B3) UI/UX States & Triggers**

| State | What Customer Sees | Trigger |
| --- | --- | --- |
| **Wallet Selection** | List of wallets (Phantom, MetaMask) | Page load |
| **Extension Missing** | "Install Chrome Extension" link under wallet | Extension not found |
| **Extension Found** | "Connect" button under wallet | Extension detected |
| **Connecting** | Loading spinner on Connect button | Customer clicks Connect |
| **Connected** | Payment view with dual pricing, token, wallet info | Connection approved |
| **Token Modal Open** | Full list of Token-Network pairs with search | Customer clicks token area |
| **Processing Payment** | Full-page loading: "Processing transaction..." | Customer clicks Pay Now |
| **Wallet Prompt** | Message: "Check your wallet extension to approve" | Transaction initiated |
| **Success** | Redirect to Order Confirmed | Webhook confirms |
| **Error** | Error message + Retry button | Transaction fails/rejected/timeout |

---

## **B4) Edge Cases & Error Handling**

| Edge Case | Expected Behavior |
| --- | --- |
| **Wallet extension not installed** | Show "Install Chrome Extension" link with Chrome Web Store URL |
| **Customer rejects connection** | Show "Connection rejected" message, allow retry |
| **Customer rejects transaction** | Show "Transaction rejected" message, remain on page, allow retry |
| **Wrong network in wallet** | Prompt customer to switch network (wallet handles this) |
| **Insufficient token balance** | Wallet shows warning; ideally check and disable Pay Now with message |
| **Transaction timeout (webhook slow)** | Show "Transaction timeout. Please check your wallet or contact support." |
| **Wallet disconnects mid-flow** | Detect disconnect, prompt to reconnect |
| **Customer closes browser during payment** | Order stays "Pending"; customer can return and check status |
| **Exchange rate changes during flow** | Rate locked at time of Pay Now click; display shown rate |
| **Customer clicks Back button** | Return to checkout; can select different payment method |

---

## **B5) Token-Network Pairs Reference**

| Network | Available Tokens |
| --- | --- |
| Polygon | USDC |
| Binance | BNB, USDC, USDT |
| SKALE | USDC |
| Bitlayer | USDC |
| Ethereum | ETH, USDC, USDT |
| XION | BNB |
| Base | ETH |
| XRPLSidechain | USDT, ETH |

*This list is configurable by merchant in Shop Builder.*

---

## **B6) Business Logic Decision Tree**

```
CUSTOMER ON CRYPTO PAYMENT PAGE
│
├──► See list of wallets (Phantom, MetaMask)
│
├──► Customer clicks wallet
│     │
│     ├──► NOT installed → Show "Install Extension" link
│     │     └──► After install → Return → Click again
│     │
│     └──► Installed → Show "Connect" button
│           │
│           ├──► Click Connect → Wallet prompts approval
│           │     │
│           │     ├──► Approved → Show payment view
│           │     └──► Rejected → Show error, retry
│           │
│           └──► Payment View
│                 │
│                 ├──► Shows: Total (USD) + Token Amount
│                 │
│                 ├──► Click Token → Open Token Modal
│                 │     └──► Select Token-Network → Close → Price updates
│                 │
│                 ├──► Click Wallet → Switch account
│                 │
│                 └──► Click "Pay Now"
│                       │
│                       ├──► Loading state
│                       ├──► Wallet prompts signature
│                       │     │
│                       │     ├──► Approved → Wait for webhook
│                       │     │     │
│                       │     │     ├──► Success → Paid → Order Confirmed
│                       │     │     └──► Timeout → Error → Retry
│                       │     │
│                       │     └──► Rejected → Error → Retry
│                       │
│                       └──► Customer can also go Back to checkout

```

---

## **B7) Test Scenarios for QA**

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| **T1** | MetaMask installed, connect, pay with USDC-Polygon | Success, redirect to Order Confirmed |
| **T2** | Phantom not installed, click Phantom | Show "Install Chrome Extension" link |
| **T3** | Install extension, return, click Phantom | Now shows "Connect" button |
| **T4** | Connect wallet, reject connection in popup | Show "Connection rejected", can retry |
| **T5** | Connected, change token from USDC to ETH | Token modal opens, select ETH, price recalculates |
| **T6** | Search "ETH" in token modal | Shows: ETH-Ethereum, ETH-Base, ETH-XRPLSidechain |
| **T7** | Search "Polygon" in token modal | Shows: USDC-Polygon |
| **T8** | Click Pay Now, reject in wallet | Error: "Transaction rejected", remain on page |
| **T9** | Complete payment, webhook confirms | Order = Paid, redirect to success |
| **T10** | Complete payment, webhook times out | Error: "Transaction timeout", can retry |
| **T11** | Switch wallet account mid-flow | New account selected, can proceed with payment |
| **T12** | Click Back/Cancel, return to checkout | Checkout page shows, can select Credit Card/PayPal |
# Payments and Wallets Settings

# **Feature ID:** SET-PAY-METHODS-009

# **Category:** Merchant Settings | Financial | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need full control over how they accept payments from customers. This includes choosing specific fiat providers (like Stripe or PayPal) and configuring crypto wallets for receiving funds.
This feature provides a centralized settings page where merchants can toggle payment methods on/off, create their internal non-custodial wallet, and configure destination addresses for crypto payouts with split-payment capabilities. These settings directly dictate which payment options are displayed to the customer at checkout.

### **Desired Outcome**

- Provide a "Payments and Wallets" tab in Store Settings.
- Allow merchants to toggle 3rd party providers (Stripe, PayPal, Coinbase) On/Off.
- Stripe should be enabled by default for new merchants.
- "Paymob" should be visible but disabled (coming soon).
- Allow merchants to generate a Circle (Non-custodial) wallet with one click.
- Allow merchants to input EVM and Solana wallet addresses for receiving payments.
- Support "Split Payments" for crypto wallets (multiple addresses with defined percentages totaling 100%).
- Allow merchants to select which Networks/Tokens they accept via a "Tokenpay" modal.
- Ensure all saved settings are reflected on the Checkout page.

---

# **2) Scope – In / Out**

### **In Scope**

- **Payment Providers Section:**
    - Toggles for Stripe, PayPal, Coinbase Commerce.
    - Stripe default state: **ON**.
    - Paymob displayed but **disabled** (UI only).
- **Merchant Wallet Section:**
    - "Create Wallet" button for first-time users (Circle integration).
    - Display Wallet Address once created.
- **Crypto Wallets Section (EVM & Solana):**
    - Input fields for Wallet Addresses.
    - Ability to add multiple wallets per chain.
    - **Percentage Input:** If multiple wallets are added, a percentage field is required.
    - **Validation:** Total percentage must equal 100%.
- **Tokenpay Section:**
    - List of available Networks/Tokens.
    - Modal to toggle specific tokens On/Off.
- **Global Save:**
    - "Save Settings" button to persist all configurations.
    - Impact: Updates the available payment methods on the Customer Checkout page.

### **Out of Scope**

- The actual transaction processing (handled in Checkout domain).
- Connecting the specific merchant account to the provider (handled in Feature *MCH-PAY-CONN-008*).
- KYC verification for wallets.

---

# **3) Key User Journeys**

---

### **Journey 1 – Configure Payment Providers**

**Step 1:** Merchant navigates to **Settings > Payments and Wallets**.
**Step 2:** Merchant sees Stripe is **ON** by default.
**Step 3:** Merchant toggles PayPal **ON**.
**Step 4:** Merchant sees Paymob is grayed out/disabled.
**Step 5:** Merchant clicks **Save**.
**Result:** Customers will see both Stripe and PayPal options at checkout.

---

### **Journey 2 – Create Internal Wallet (Circle)**

**Step 1:** Merchant sees "Merchant Wallet" section.
**Step 2:** System shows "No Wallet Created" and a **"Create Wallet"** button.
**Step 3:** Merchant clicks "Create Wallet".
**Step 4:** System generates the non-custodial wallet.
**Step 5:** UI updates to show the **Wallet Address** instead of the button.

---

### **Journey 3 – Configure Crypto Split Payment**

**Step 1:** Merchant goes to "EVM Wallet" or "Solana Wallet" section.
**Step 2:** Enters Wallet Address A and sets percentage to **60%**.
**Step 3:** Adds a second row, enters Wallet Address B.
**Step 4:** System requires the remaining **40%**. Merchant enters 40%.
**Step 5:** Merchant clicks **Save**.
**Result:** Future crypto payments will be automatically split 60/40 between these addresses.

---

### **Journey 4 – Select Accepted Tokens (Tokenpay)**

**Step 1:** Merchant clicks on **Tokenpay** settings.
**Step 2:** A modal opens showing lists of networks (e.g., Polygon, Ethereum, Solana).
**Step 3:** Merchant toggles "USDC on Polygon" **ON** and "ETH on Ethereum" **OFF**.
**Step 4:** Closes modal and clicks **Save**.
**Result:** Customers can only pay using the selected tokens.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Stripe toggle must be **ON** by default for new accounts.
**BAC 2:** Paymob toggle must be visible but non-interactive (disabled state).
**BAC 3:** The "Create Wallet" button (Circle) must disappear after successful creation, replaced by the wallet address.
**BAC 4:** Crypto Wallet inputs (EVM/Solana) are optional.
**BAC 5:** **Validation:** If multiple wallets are entered for a single chain, the sum of percentages must equal exactly **100%**. If not, "Save" is blocked with an error.
**BAC 6:** The Tokenpay modal must correctly save the list of enabled/disabled networks/tokens.
**BAC 7:** Clicking "Save" must persist all changes to the database.
**BAC 8:** **Integration Check:** The Checkout Page must dynamically render payment methods based *only* on what is enabled in this settings page.
# Connecting Merchant Payment Accounts

# **Feature ID:** MCH-PAY-CONN-008

# **Category:** Merchant Settings | Financial | **Actors:** Merchant | **Channel:** Web Dashboard

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Currently, without a direct payment provider connection, merchant earnings may default to internal "Shop Credit," requiring manual withdrawal or delaying access to funds.
This feature allows merchants to connect their existing **Stripe** or **PayPal** accounts directly to Droplinked. By doing so, the merchant's share of revenue from customer orders is automatically and immediately routed to their connected external account, streamlining the payout process and reducing reliance on internal platform credits.

### **Desired Outcome**

- Merchants can access a "Payments and Wallets" section in Settings.
- Merchants can initiate a connection flow for Stripe and PayPal via a "Connect" button.
- The system redirects the merchant to the provider’s authentication flow (in the current tab) and handles the return callback.
- Upon successful connection, the UI updates the button from "Connect" to **"View"**.
- Future orders automatically route the merchant’s revenue share to the connected account instead of the Shop Credit wallet.

---

# **2) Scope – In / Out**

### **In Scope**

- **UI:** New section in "Settings > Payments and Wallets" with Stripe and PayPal rows.
- **Connection Flow:**
    - "Connect" button logic.
    - Redirection to provider (Stripe Connect / PayPal Partner flow).
    - Handling success/failure callbacks.
- **Post-Connection UI:**
    - Changing button text to "View" (or "Manage").
    - "View" button redirects to the respective provider’s dashboard.
- **Backend Logic:**
    - Storing the connected Account ID securely.
    - **Payout Logic Update:** If a provider is connected, the order splitting logic must route funds to this account immediately upon payment capture, bypassing "Shop Credit".

### **Out of Scope**

- Creating a new Stripe/PayPal account (user must do this on the provider's side).
- Handling KYC or compliance for the merchant’s Stripe/PayPal account (handled by the provider).
- Managing disputes or chargebacks within Droplinked (managed on the provider dashboard).

---

# **3) Key User Journeys**

---

### **Journey 1 – Connect Payment Provider**

**Step 1:** Merchant navigates to **Settings > Payments and Wallets**.
**Step 2:** Merchant sees "Stripe" and "PayPal" options with a **"Connect"** button.
**Step 3:** Merchant clicks "Connect" on Stripe.
**Step 4:** The current tab redirects to the Stripe onboarding/login page.
**Step 5:** Merchant completes the authorization on Stripe.
**Step 6:** Merchant is redirected back to Droplinked Settings.
**Step 7:** System verifies the connection and updates the button to **"View"**.

---

### **Journey 2 – View Connected Account**

**Step 1:** Merchant navigates to **Settings > Payments and Wallets**.
**Step 2:** Merchant sees the status as connected.
**Step 3:** Merchant clicks **"View"**.
**Step 4:** System redirects the merchant to their Stripe/PayPal dashboard to manage their funds externally.

---

### **Journey 3 – Automated Payout (System Logic)**

**Step 1:** A Customer places an order for the Merchant’s product.
**Step 2:** Payment is processed.
**Step 3:** System detects the Merchant has a **Connected Account** (Stripe/PayPal).
**Step 4:** System automatically splits the payment and sends the Merchant's share directly to their connected account.
**Step 5:** The Merchant's "Shop Credit" balance remains unchanged (funds are settled externally).

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** The "Payments and Wallets" settings page must display Stripe and PayPal options.
**BAC 2:** Clicking "Connect" must redirect the user to the correct provider OAuth URL in the **current tab**.
**BAC 3:** Upon returning from the provider:
- If successful, the button must change to **"View"**.
- If failed/cancelled, the button must remain **"Connect"** and show an error message.
**BAC 4:** Clicking "View" must open the Merchant's external account dashboard.
**BAC 5:** **Crucial:** Once connected, the backend order processing must flag the merchant for "Direct Payout."
**BAC 6:** Validated orders for connected merchants must route funds to the external account, **not** the internal Shop Credit wallet.
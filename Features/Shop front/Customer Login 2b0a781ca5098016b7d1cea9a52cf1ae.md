# Customer Login

**Feature ID:** STF-LOGIN-101

**Category:** Storefront / Authentication

**Actors:** Merchant, Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Storefront is the customer-facing experience where visitors browse products and make purchases.

To manage carts, order history, wallet-based features, and Web3 interactions, customers must be able to **log in** using flexible methods determined by the merchant.

### **Desired Outcome**

- Merchant chooses which authentication methods are available.
- Storefront displays only those enabled options.
- Customers can log in using Google or supported Web3 wallets.
- First-time login automatically creates a customer profile.
- Returning users reconnect to their existing account and their cart.

---

# **2) Scope – In / Out**

### **In Scope**

- Merchant toggles login methods (Unstoppable Domains, Phantom, MetaMask, Google).
- Storefront Login Modal displaying only enabled methods.
- Customer authentication via selected provider.
- First-time user account creation.
- Returning user session restoration.
- Cart restoration after login.
- Wallet login error handling (missing extension → open install page).
- Profile icon in header showing user session status.
- Profile popup menu displaying wallet address or email.
- Visual representation for email users (first letter of email as avatar).
- Logout functionality via profile popup menu.

### **Out of Scope**

- Password-based login (email/password).
- Two-factor authentication.
- Account settings or customer profile editing.
- Admin-side impersonation.
- Social login providers other than Google.
- Wallet linking after login (multi-wallet linking).

---

# **3) Key User Journeys**

---

### **Journey 1 – Merchant Enables Login Methods**

1. Merchant goes to **Settings → Payments & Wallets** (Shop Builder).
2. Sees a list of supported login providers.
3. Toggles ON the methods they want to allow.
4. Must enable **at least one** method.
5. Storefront will immediately reflect these options.

---

### **Journey 2 – Customer Opens Login Modal**

1. Customer opens Storefront.
2. Clicks on **Profile icon** in the header.
3. **If not logged in:** Login Modal opens.
4. Modal shows **only** the methods enabled by the merchant.
5. **If already logged in:** Profile popup menu opens (see Journey 7).

---

### **Journey 3 – Customer First-Time Login**

1. Customer selects a login provider.
2. Completes Google or wallet authentication.
3. System detects no existing customer record.
4. Creates a new Customer entity.
5. Logs them in successfully.

---

### **Journey 4 – Customer Returning Login**

1. Customer selects login method.
2. System finds existing account.
3. Logs them in.
4. Previous cart is restored (if any).

---

### **Journey 5 – Customer Wallet Login (MetaMask / Phantom / UD)**

1. Customer selects a wallet login method.
2. If wallet extension **is not installed**:
    - Show error.
    - Open a **new tab** to wallet installation page.
3. If installed:
    - Wallet signature request pops up.
    - Upon acceptance, customer logs in.
    - Account is created or attached.

---

### **Journey 6 – Failed Authentication**

1. Customer rejects wallet signature OR Google token fails.
2. Show appropriate error:
    - “Connection rejected.”
    - “Login failed, please try again.”

---

### **Journey 7 – Logged-In User Profile Menu**

1. After successful login, customer sees the **Profile icon** in the header.
2. **If logged in via wallet:** Icon displays wallet avatar or default icon.
3. **If logged in via Google/email:** Icon displays the **first letter of the email address** as avatar.
4. Customer clicks on the Profile icon.
5. A **popup menu** appears at the top of the page showing:
    - User's wallet address (if wallet login) OR email address (if Google login).
    - **Logout button**.
6. Customer clicks **Logout**.
7. System logs out the customer and clears the session.
8. Profile icon returns to the logged-out state.
9. Cart is cleared or saved based on merchant configuration.

---

# **4) Business Acceptance Criteria (BAC)**

### **BAC 1:** Merchant must be able to enable or disable each login method individually.

### **BAC 2:** At least one login method must be enabled; otherwise, storefront login is disabled.

### **BAC 3:** The Storefront Login Modal must show only the methods enabled by the merchant.

### **BAC 4:** First-time login must automatically create a new customer profile.

### **BAC 5:** Returning customers must log into their existing account, and their cart must be restored.

### **BAC 6:** Wallet login must display a clear error and redirect to installation if the browser lacks the required extension.

### **BAC 7:** All login failures must display clear error messages without exposing internal system details.

### **BAC 8:** After successful login, clicking the profile icon must open a popup menu showing the user's wallet address or email.

### **BAC 9:** For email-based logins, the profile icon must display the first letter of the email address as the avatar.

### **BAC 10:** The profile popup menu must include a clearly visible Logout button.

### **BAC 11:** Clicking Logout must immediately clear the user session and return the profile icon to its logged-out state.
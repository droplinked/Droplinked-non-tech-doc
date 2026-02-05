# Google Authentication for Login & Signup

# **Feature ID:** IAA-AUT-004

# **Category:** Authentication | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants must be able to authenticate quickly and securely without entering passwords. Adding Google OAuth streamlines onboarding, reduces friction, and improves conversion from both login and signup screens.

### **Desired Outcome**

- Enable merchants to authenticate via Google from both Login and Signup pages.
- Auto-create a merchant account if the Google email does not exist.
- Auto-login merchants if the Google email already exists.
- Redirect merchants correctly based on onboarding completion.
- Maintain standard OAuth security without requiring a password.

---

# **2) Scope – In / Out**

### **In Scope**

- Google Authentication button labeled **“Google Account”** on:
    - Login page
    - Signup page
- OAuth flow for retrieving Google account primary email.
- Automatic merchant account creation if email does not exist.
- Skipping password creation for Google-authenticated accounts.
- Automatic login for merchants whose Google email already exists.
- Redirect logic:
    - New merchant → Onboarding
    - Existing merchant → Dashboard
- Basic error handling via toast (e.g., Google auth cancellation, failed token retrieval).

### **Out of Scope**

- Password creation for Google-authenticated merchants.
- Token refresh logic or long-term sessions (not required).
- UI styling compliant with Google branding (not mandated).
- Multi-provider authentication (Facebook, Apple, etc.).
- Backend Google OAuth configuration (client ID, redirect URIs).

---

# **3) Key User Journeys**

### **Journey 1 – Google Signup (New Merchant)**

As a **Merchant**, when I authenticate with Google and I do not have an existing account, the system creates one and starts onboarding.

**Step 1:** Merchant clicks “Google Account” on the Signup page.

**Step 2:** System opens Google OAuth window.

**Step 3:** Merchant selects Google account and grants permission.

**Step 4:** System receives Google email and basic profile info.

**Step 5:** Backend checks if merchant exists → not found.

**Step 6:** System creates a new merchant account with Google email (no password).

**Step 7:** Merchant is redirected to onboarding.

---

### **Journey 2 – Google Login (Existing Merchant)**

As a **Merchant**, when I authenticate with Google and already have an account, I should be logged in directly.

**Step 1:** Merchant clicks “Google Account” on the Login page.

**Step 2:** System opens Google OAuth window.

**Step 3:** Merchant selects account.

**Step 4:** Backend matches email to existing merchant.

**Step 5:** System logs merchant in.

**Step 6:**

- If onboarding completed → redirect to Dashboard
- If onboarding incomplete → redirect to Onboarding

---

### **Journey 3 – Google Auth Failure**

As a **Merchant**, if the Google authentication fails, I should receive an error message.

**Step 1:** Merchant clicks “Google Account”.

**Step 2:** OAuth fails (user cancels, network issue, token error).

**Step 3:** System shows a toast error message.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** A “Google Account” button must be displayed on both Login and Signup pages.

**BAC 2:** When authenticated with Google:

- If email **does not exist**, system must create a new merchant account without requiring a password.
- If email **exists**, merchant must be logged in automatically.

**BAC 3:** Redirect logic must match:

- New merchant → Onboarding
- Existing merchant → Dashboard

**BAC 4:** System must retrieve the Google-provided primary email and necessary basic user info (fields flexible).

**BAC 5:** Errors during Google auth (cancellation, denial, token failure) must be displayed via toast.

**BAC 6:** Google-authenticated accounts must not require or prompt for a password during signup.
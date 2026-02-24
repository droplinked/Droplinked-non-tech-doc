# Merchant Login

# **Feature ID:** IAA-AUT-003

# **Category:** Authentication | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a secure, reliable, and simple way to access their account. A structured login flow with email validation, password visibility control, and onboarding completion logic ensures proper access control and smooth user experience.

### **Desired Outcome**

- Allow merchants to log in with a valid email and password.
- Validate email format in real-time and revalidate on server-side.
- Redirect merchants based on onboarding completion status.
- Provide access to password reset and account creation pages.

---

# **2) Scope – In / Out**

### **In Scope**

- Login screen with fields:
    - Email (with real-time format validation)
    - Password
- Password visibility toggle (show/hide).
- Submit via “Login” button.
- Logic flow after login:
    - If merchant has **completed onboarding** → redirect to **Dashboard**
    - If merchant has **not completed onboarding** → redirect to **Onboarding flow**
- Buttons & redirects:
    - “Reset Password” → navigates to Reset Password screen
    - “Join & Create” → navigates to Signup screen
- Error handling (toast or inline, flexible).
- Login rate limiting standards.

### **Out of Scope**

- Reset Password flow (separate PRD).
- Signup flow details (covered previously).
- Multi-factor authentication (if any).
- Long-term lockout or account recovery flows.

---

# **3) Key User Journeys**

### **Journey 1 – Logging In**

As a **Merchant**, when I enter my login credentials, the system authenticates me and routes me accordingly.

**Step 1:** Merchant enters email (real-time validation checks format).

**Step 2:** Merchant enters password.

**Step 3:** Merchant optionally toggles password visibility using the eye icon.

**Step 4:** Merchant clicks “Login”.

**Step 5:** System validates credentials on the server.

**Step 6:**

- If credentials valid and onboarding completed → redirect to Dashboard
- If credentials valid but onboarding not completed → redirect to Onboarding flow
- If invalid → display error message
- If user does not exist → display error message

---

### **Journey 2 – Reset Password**

As a **Merchant**, if I forgot my password, I can initiate a reset flow.

**Step 1:** Merchant clicks “Reset Password”.

**Step 2:** System navigates to Reset Password screen.

---

### **Journey 3 – Navigate to Signup**

As a **Merchant**, if I do not have an account, I can start registration.

**Step 1:** Merchant clicks “Join & Create”.

**Step 2:** System navigates to the Signup screen.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** System must validate email format in real-time before allowing login submission.

**BAC 2:** System must validate login credentials server-side and display an error if:

- Email not found
- Incorrect password

**BAC 3:** System must redirect authenticated merchants:

- To **Dashboard** if onboarding is completed
- To **Onboarding** if onboard is incomplete

**BAC 4:** Login attempts must be rate limited as follows:

- Maximum **5 failed attempts** within **10 minutes**
- If exceeded, temporary lock for **5 minutes**
- During lock: login button disabled and error message shown

**BAC 5:** Password visibility toggle must function to show/hide the password without changing input value.

**BAC 6:** “Reset Password” and “Join & Create” buttons must navigate to their respective screens.
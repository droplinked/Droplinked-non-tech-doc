# Reset Password Flow

# **Feature ID:** IAA-AUT-005

# **Category:** Authentication | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants who forget their password must be able to securely reset it. A structured, multi-step reset flow with OTP verification ensures ownership of the email and prevents unauthorized access. Enforcing password rules and preventing reuse of the previous password strengthens account security.

### **Desired Outcome**

- Allow merchants to request a password reset using their email.
- Verify email ownership via a 6-digit OTP sent to the entered email.
- Allow merchants to create a new secure password that follows platform rules.
- Prevent setting a password identical to the previous one.
- Redirect merchants to the login page after a successful reset.

---

# **2) Scope – In / Out**

### **In Scope**

- Reset Password entry point from Login page (“Reset Password” button).
- Reset Password screen with email input.
- Sending a 6-digit OTP to the merchant’s email.
- OTP expiry after **2 minutes**.
- OTP entry screen with 6 separate boxes.
- Error handling via **toast messages**.
- Setting a new password with the following rules:
    - One lowercase letter
    - One uppercase letter
    - One digit
    - One special character
    - Minimum 8 characters
- Double password entry (New Password + Confirm Password).
- Validating password difference from previous password.
- Redirecting user to Login upon successful reset.

### **Out of Scope**

- OTP resend functionality (not included).
- Password strength meter.
- Long-term account lockout logic.
- Any onboarding or signup logic.
- Changing password while logged in (separate feature).

---

# **3) Key User Journeys**

### **Journey 1 – Initiate Reset**

As a **Merchant**, when I forget my password, I can request a reset.

**Step 1:** Merchant clicks “Reset Password” on the Login page.

**Step 2:** System navigates to the Reset Password screen.

**Step 3:** Merchant enters their email.

**Step 4:** System sends a 6-digit OTP to the provided email.

**Step 5:** System navigates merchant to OTP entry screen.

---

### **Journey 2 – OTP Verification**

As a **Merchant**, after receiving a reset code, I verify ownership of my email.

**Step 1:** Merchant enters the 6-digit OTP in six input boxes.

**Step 2:** System verifies the OTP.

**Step 3:**

- If valid and within 2 minutes → navigate to “Create New Password” page
- If invalid or expired → display toast error

---

### **Journey 3 – Set New Password**

As a **Merchant**, I create a new secure password that meets platform rules.

**Step 1:** Merchant enters new password.

**Step 2:** Merchant re-enters password to confirm.

**Step 3:** System validates:

- Password rule compliance
- Passwords match
- Password is **not the same as previous password**
    
    **Step 4:**
    
- If valid → system resets password and redirects to Login page
- If invalid → show toast error(s)

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** System must send a 6-digit OTP when the email is submitted and enforce a **2-minute expiry**.

**BAC 2:** OTP must be entered through **six separate boxes** and validated on submit.

**BAC 3:** All error messages throughout the flow must be displayed via **toast**.

**BAC 4:** New password must meet the required rules:

- One lowercase letter
- One uppercase letter
- One special character
- One digit
- Minimum 8 characters

**BAC 5:** System must prevent password reset if the new password is **identical to the previous password** and show a toast error.

**BAC 6:** Upon successful password reset, the merchant must be redirected to the **Login** page.
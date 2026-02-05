# Confirm Email – OTP Verification Flow

# **Feature ID:** IAA-AUT-002

# **Category:** Authentication | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

After signup, merchants must verify ownership of their email to activate their account. Adding a structured OTP verification step ensures account authenticity, prevents fraudulent signups, and enforces security standards.

### **Desired Outcome**

- Allow merchants to input a 6-digit email OTP for verification.
- Provide error feedback via toast notifications.
- Allow merchants to request a resend of the OTP with controlled limitations.
- Redirect merchants to the onboarding flow after successful verification.

---

# **2) Scope – In / Out**

### **In Scope**

- Displaying a 6-box OTP input field (each box for 1 digit).
- Real-time focus auto-movement between boxes.
- Validating a 6-digit OTP against backend service.
- Toast-based error messaging for:
    - Invalid code
    - Expired code
- OTP expiry configured at **2 minutes**.
- “Resend Code” action that:
    - Sends a new OTP to the merchant’s email
    - Follows defined resend limitations
- Redirecting the merchant to the onboarding flow after successful verification.

### **Out of Scope**

- Countdown timer UI (not included).
- Details of onboarding steps (covered in a separate document).
- Multi-channel delivery (SMS, app) — email only.
- Rate limiting or lockout behavior beyond resend limits.

---

# **3) Key User Journeys**

### **Journey 1 – Enter OTP for Verification**

As a **Merchant**, when I receive a 6-digit code and enter it, the system verifies my account.

**Step 1:** Merchant lands on the Confirm Email page after signup.

**Step 2:** Merchant enters the 6-digit OTP using six input boxes.

**Step 3:** System validates the OTP.

**Step 4:** If valid and not expired, merchant is redirected to onboarding.

**Step 5:** If invalid or expired, system displays a toast error.

---

### **Journey 2 – Resend Code**

As a **Merchant**, if I did not receive the code, I can request a resend within defined limits.

**Step 1:** Merchant clicks “Resend Code”.

**Step 2:** System checks resend limits.

**Step 3:** If allowed, system sends a new OTP to the merchant’s email.

**Step 4:** If limit exceeded, system displays a toast informing the merchant to wait before retrying.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** System must validate OTP input as exactly 6 digits and handle entry via six separate boxes.

**BAC 2:** System must display toast notifications for invalid or expired OTP attempts.

**BAC 3:** System must enforce OTP expiry after **2 minutes**.

**BAC 4:** System must enforce resend limitations (defined below):

- Maximum **3 resends** within a rolling **15-minute** window
- Minimum **30 seconds** required between resend attempts
    
    **BAC 5:** Upon correct OTP entry, system must redirect the merchant to the onboarding flow.
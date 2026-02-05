# Merchant Signup & Email Verification Flow

## **Feature ID:** IAA-AUT-001

## **Category:** Authentication | **Actors:** Merchant

## **Status:** Released

## **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants must be able to quickly and securely create an account to access the platform. A structured signup flow with real-time password validation and email verification ensures account authenticity, improves onboarding efficiency, and enhances security.

### **Desired Outcome**

- Enable merchants to create an account using a valid email and a secure password.
- Support entry of an optional referral code.
- Automatically send a 6-digit verification code to the merchant’s email after successful signup.

---

# **2) Scope – In / Out**

### **In Scope**

- Signup entry point from landing page “Start Now” and header “Get Started”.
- Signup form with fields:
    - Email
    - Password (with real-time password rule validation)
    - Optional referral code
- Password rules:
    - One lowercase letter
    - One uppercase letter
    - One digit
    - One special character
    - Minimum 8 characters
- Password visibility toggle (show/hide).
- Submit behavior via **Sign Up** button.
- Email-based OTP (6-digit code) sent automatically upon successful signup.
- Redirect to OTP entry page.

### **Out of Scope**

- OTP resend rules, lockout rules (covered in a separate OTP document).
- Referral code validation logic (separate document).
- Post-verification redirect destination (covered in another document).
- Login, password reset, and other authentication flows.

---

# **3) Key User Journeys**

### **Journey 1 – Signup Initiation**

As a **Merchant**, when I click “Start Now” or “Get Started,” I am directed to the signup page.

**Step 1:** Merchant clicks “Start Now” on landing page or “Get Started” in the header.

**Step 2:** System routes merchant to the signup screen.

---

### **Journey 2 – Account Creation**

As a **Merchant**, when I enter my required credentials and submit, my account is created and an OTP is emailed to me.

**Step 1:** Merchant enters a valid email address.

**Step 2:** Merchant enters a password and sees real-time validation for password rules.

**Step 3:** Merchant optionally enters a referral code.

**Step 4:** Merchant taps the password visibility toggle to show/hide password if needed.

**Step 5:** Merchant clicks “Sign Up”.

**Step 6:** System validates input formats and creates the account.

**Step 7:** System sends a 6-digit OTP to the merchant’s email.

**Step 8:** System redirects merchant to the OTP entry page.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** The system must validate email format and all password rules before allowing form submission.

**BAC 2:** The system must allow entry of an optional referral code without blocking signup.

**BAC 3:** After successful signup, the system must send a 6-digit OTP to the merchant’s email and route them to the OTP entry screen.

---
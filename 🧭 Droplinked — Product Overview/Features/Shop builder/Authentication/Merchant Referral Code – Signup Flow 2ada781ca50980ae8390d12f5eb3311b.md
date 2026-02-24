# Merchant Referral Code – Signup Flow

# **Feature ID:** IAA-AUT-005

# **Category:** Authentication / Signup | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants can refer other merchants to the platform using a referral code. Capturing this during signup allows the platform to track referrals and credit the referring merchant for future sales, improving user acquisition and incentivizing existing merchants to promote the platform.

### **Desired Outcome**

- Allow new merchants to optionally enter a referral code during signup.
- Validate and link the new merchant to the referring merchant.
- Ensure the referral link is recorded for downstream revenue credit calculations.

---

# **2) Scope – In / Out**

### **In Scope**

- Optional referral code input field on merchant signup page.
- Validation of referral code format (if needed, per separate referral rules document).
- Linking new merchant account to the referring merchant in the backend.
- No referral code entry allowed after signup.
- Referral code capture works for both email/password signup and Google Auth signup flows.

### **Out of Scope**

- Calculating revenue credits for referrers (covered in Orders / Revenue PRD).
- Editing or changing referral codes post-signup.
- Referral program analytics (covered separately).

---

# **3) Key User Journeys**

### **Journey 1 – Enter Referral Code During Signup**

As a **new Merchant**, when I have a referral code from another merchant, I can enter it during signup.

**Step 1:** Merchant opens the signup page (email/password or Google Auth).

**Step 2:** Merchant optionally enters a referral code in the designated field.

**Step 3:** System validates the referral code format.

**Step 4:** If valid, system links the new merchant to the referring merchant.

**Step 5:** Merchant completes signup as usual (email/password or Google signup).

**Step 6:** System stores the referral relationship for downstream revenue crediting.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Referral code field must be optional during signup.

**BAC 2:** If a referral code is entered, the system must validate it and link the new merchant to the referring merchant.

**BAC 3:** Referral code linking must work for all signup methods (email/password, Google Auth).

**BAC 4:** Referral code cannot be changed after signup.

**BAC 5:** The referral relationship must be stored in the backend to support future credit calculations.
# Redirection

# Redirection

# *Feature ID:**REDIRECT-015

# **Category:** Onboarding | Authentication | **Actors:** Merchant | **Channel:** Web

# **Status:** Defined

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

When users visit the main URL (`droplinked.com`), the interface must adapt to their authentication status.
Guests should see clear calls to action ("Get Started", "Sign In"). Logged-in users should see access to their account (Profile Icon or Login Button).
Crucially, when a logged-in user attempts to access their account (by clicking the Profile/Login element), the system must route them correctly:

- To the **Dashboard** if their shop is fully set up.
- To the **Onboarding Settings** if they haven't completed their Shop Name or URL.

### **Desired Outcome**

- **Guest State:** Header displays "Get Started" and "Sign In".
- **Logged-In State:** Header displays "Profile Icon" or "Login Button".
- **Click Action (Logged In):**
    - If Shop Name & URL are set → Redirect to **Dashboard**.
    - If Shop Name or URL is missing → Redirect to **Onboarding Settings**.

---

# **2) Scope – In / Out**

### **In Scope**

- **Header UI Logic:** Toggling between Guest buttons and User Profile/Login button based on auth status.
- **Click Event Handling:** Intercepting the click on the Profile/Login element.
- **Data Check:** Verifying `shop_name` and `shop_url` existence.
- **Redirection:** Routing to Dashboard vs. Onboarding.

### **Out of Scope**

- The actual design of the landing page.
- The content of the onboarding forms.

---

# **3) Key User Journeys**

---

### **Journey 1 – Guest User (Not Logged In)**

**Step 1:** User visits `droplinked.com`.
**Step 2:** System detects **No** valid Auth Token.
**Step 3:** Header displays **"Get Started"** and **"Sign In"** buttons.

---

### **Journey 2 – Returning Merchant (Setup Complete)**

**Step 1:** User visits `droplinked.com` (already logged in).
**Step 2:** Header displays **"Profile Icon"** or **"Login Button"**.
**Step 3:** User clicks the Icon/Button.
**Step 4:** System checks Shop Profile → Found **Shop Name** & **URL**.
**Step 5:** System redirects user to **Merchant Dashboard**.

---

### **Journey 3 – New or Incomplete Merchant**

**Step 1:** User visits `droplinked.com` (already logged in).
**Step 2:** Header displays **"Profile Icon"** or **"Login Button"**.
**Step 3:** User clicks the Icon/Button.
**Step 4:** System checks Shop Profile → **Shop Name** or **URL** is missing.
**Step 5:** System redirects user to **Shop Onboarding Settings**.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** **Guest Header:** If user is not logged in, the header must show "Get Started" and "Sign In".
**BAC 2:** **Logged-In Header:** If user is logged in, the header must show the "Profile Icon" or "Login Button".
**BAC 3:** **Routing Logic:** Clicking the Profile Icon/Login Button triggers a check on Shop Name and Shop URL.
**BAC 4:** **Complete State:** If both Shop Name and URL exist, redirect to **Dashboard**.
**BAC 5:** **Incomplete State:** If either Shop Name or URL is missing, redirect to **Onboarding Settings**.
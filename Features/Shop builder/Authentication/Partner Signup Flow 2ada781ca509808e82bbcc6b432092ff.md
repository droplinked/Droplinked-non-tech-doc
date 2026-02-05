# Partner Signup Flow

# **Feature ID:** IAA-PART-006

# **Category:** Signup / Partner Programs | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Droplinked collaborates with various partners (e.g., Polygon, D3, Unstoppable Domains) to drive merchant acquisition. Each partner has a dedicated landing page with specific campaigns, eligibility conditions, and rewards.
We need a system to track users coming from these landing pages, verify their eligibility (often via wallet connection and asset ownership), and automatically assign rewards (e.g., Pro Plan) upon successful signup.
Crucially, we must prevent abuse by ensuring a single wallet address cannot be used for multiple signups to claim rewards repeatedly.

### **Desired Outcome**

- Dedicated landing pages for each partner.
- A centralized "Partners" section on the main site to navigate to these pages.
- **Conditional Logic:** Ability to enforce wallet checks (Badge/Domain ownership) before signup.
- **Reward System:** Automatically grant Pro Plan trials based on campaign rules.
- **Tracking:** Tag every user with their source campaign in the database.
- **Security:** Prevent duplicate signups with the same wallet address.
- **UX:** Clear error handling if wallet extensions are missing.

---

# **2) Scope – In / Out**

### **In Scope**

- **Partner List & Rules:**
    - **Dubai Commercity:** No Condition | No Reward.
    - **Gaia:** No Condition | No Reward.
    - **Base:** Condition: Connect Wallet | No Reward.
    - **Crossmint:** Condition: Add Card (Post-Signup) | Reward: 3 Months Pro.
    - **Polygon:** Condition: Connect Wallet + `.polygon` domain | Reward: 3 Months Pro.
    - **D3:** Condition: Connect D3 Wallet + Domain | Reward: 6 Months Pro.
    - **UD (Unstoppable Domains):** Condition: Connect Wallet + UD Badge | Reward: 3 Months Pro.
- **Flow Components:**
    - Partner Listing Page.
    - Partner Detail/Landing Page.
    - "Claim Now" Modal with Wallet Verification (for conditional partners).
    - Wallet Extension Detection & Error Messaging.
    - Signup Redirection.
    - Database Tracking (`campaign_id`, `wallet_address`).
- **Constraints:**
    - **Unique Wallet Rule:** A user cannot sign up a second time with a wallet address that has already been used, even if they meet the condition.

### **Out of Scope**

- Creating the actual creative assets for the landing pages.
- The logic for "Adding Card" for Crossmint (this is handled in the billing flow, but the *tracking* starts here).

---

# **3) Key User Journeys**

---

### **Journey 1 – Partner with Wallet Condition (Polygon, D3, UD, Base)**

**Step 1:** User visits Droplinked Main Landing → Clicks "Partners" in Header/Section.
**Step 2:** User sees the list of partners and clicks on one (e.g., **Polygon**).
**Step 3:** User lands on the **Polygon Partner Page**, seeing the offer ("3 Months Pro") and condition ("Connect Wallet with .polygon domain").
**Step 4:** User clicks **"Claim Now"**.
**Step 5:** A **Modal** opens with a button: **"Check Wallet Eligibility"**.
**Step 6:** User clicks the button.
- **Scenario A (No Extension):** System detects missing wallet extension → Shows specific error message: "Please install [Wallet Name] extension to proceed."
- **Scenario B (Extension Present):** Wallet extension opens → User approves connection.
**Step 7:** System verifies the condition (e.g., checks for `.polygon` domain).
- **Failure:** Show error: "Eligibility criteria not met."
- **Success:** Show success message: "You are eligible!"
**Step 8:** User clicks **"Claim"** (or "Proceed to Signup").
**Step 9:** System checks if this wallet address has been used before.
- **If Used:** Error: "This wallet has already been used for a signup."
- **If New:** Redirect to **Signup Page**.
**Step 10:** User completes signup.
**Step 11:** System records the user with `campaign_source = polygon` and activates the **Pro Plan** (3 Months).

---

### **Journey 2 – Partner without Pre-Signup Wallet Check (Dubai, Gaia, Crossmint)**

**Step 1:** User visits Droplinked Main Landing → Clicks "Partners".
**Step 2:** User clicks on a partner (e.g., **Dubai Commercity** or **Crossmint**).
**Step 3:** User lands on the Partner Page.
**Step 4:** User clicks **"Claim Now"**.
**Step 5:** System **immediately redirects** to the **Signup Page** (No wallet check modal).
**Step 6:** User completes signup.
**Step 7:** System records the user with `campaign_source = [partner_name]`.
- **For Crossmint:** The 3-month Pro Plan is triggered *only after* the user adds a card in the dashboard (handled by billing logic, but tracked via this campaign source).
- **For Dubai/Gaia:** No reward is applied.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** **Campaign Tracking:** Every user signing up through a partner flow must have a `campaign_id` or `source` tag stored in the database for reporting.
**BAC 2:** **Unique Wallet Constraint:** The system must enforce a strict "One Signup per Wallet" rule. If a wallet address exists in the DB (associated with any user), a new signup attempt with that wallet must be blocked with a clear error message.
**BAC 3:** **Extension Error Handling:** If the user attempts to connect a wallet but lacks the required browser extension, the system must display a user-friendly error instructing them to install it.
**BAC 4:** **Reward Activation:**

- **Polygon, UD:** 3 Months Pro Plan activated immediately upon signup.
- **D3:** 6 Months Pro Plan activated immediately upon signup.
- **Crossmint:** 3 Months Pro Plan activated *after* card entry (post-signup).
- **Base, Gaia, Dubai:** No automatic plan activation.
**BAC 5:** **Eligibility Logic:**
- **Base:** Connect Wallet only.
- **Polygon:** Connect Wallet + Own `.polygon` domain.
- **D3:** Connect D3 Wallet + Own Domain.
- **UD:** Connect Wallet + Own UD Badge.
**BAC 6:** **Navigation:** Users must be able to access specific partner pages via the "Partners" section on the main landing page.
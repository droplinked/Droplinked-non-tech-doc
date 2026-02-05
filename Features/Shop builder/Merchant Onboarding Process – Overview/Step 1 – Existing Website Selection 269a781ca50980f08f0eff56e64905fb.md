# Step 1 – Existing Website Selection

## **Step 1 – Existing Website Selection**

### Feature ID:

**ONB-STEP1-001**

### Title:

**Existing Website Selection**

### Category:

**Onboarding / Merchant Setup** | **Actors**: Merchant | **Channel**: Merchant Dashboard

### Status:

**Defined**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**

Merchants need to indicate whether they already have an existing website to determine the initial setup flow. This ensures the onboarding process can adapt and provide the right guidance for merchants with or without a pre-existing site.

**Desired Outcome:**

- Merchants select whether they have an existing website.
- The system directs them to the next appropriate step based on their selection.

---

### 2) **Scope – In / Out**

**In:**

- Display a simple choice: **Yes** or **No**.
- Selection recorded and used to guide the onboarding flow.

**Out:**

- Detailed setup for merchants who selected **Yes** (not implemented yet).
- Automatic website integration.

---

### 3) **Key User Journeys**

- **Journey 1:**
    - **Role/Trigger**: Merchant signs up and enters onboarding
    - **Behavior/Action**: Selects **No** for existing website
    - **Value/Outcome**: Proceeds to Step 2 – Shop Details & AI Setup

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1**: Merchant must select one of the options to proceed → Pass/Fail
- **BAC 2**: Selection is saved and used to determine next onboarding step → Pass/Fail

---
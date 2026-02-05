# Step 4 – Plan Upgrade / AI Feature

---

## **Step 4 – Plan Upgrade / AI Feature**

### Feature ID:

**ONB-STEP4-004**

### Title:

**Plan Upgrade for AI Feature Access**

### Category:

**Onboarding / Merchant Setup** | **Actors**: Merchant | **Channel**: Merchant Dashboard

### Status:

**Defined**

### Owner:

**PM X**

---

### 1) **Summary**

**Problem/Value:**

Some onboarding features, like AI-assisted shop setup (logo, hero section, URL, name), are only available for AI-enabled plans. Merchants must be able to upgrade their plan to access AI tools if they attempt to use them.

**Desired Outcome:**

- AI-assisted generation is only accessible to merchants with AI-enabled plans.
- If the merchant selects AI feature without proper plan, a modal prompts plan upgrade.
- Merchant can choose to upgrade and access AI features or skip; skipping disables AI permanently.
- After upgrading, AI-assisted setup can generate shop URL, name, logo, and hero section suggestions based on business description and category.

---

### 2) **Scope – In / Out**

**In:**

- Display plan upgrade modal when AI feature is selected by a non-AI plan merchant.
- Allow merchants to upgrade plan directly from modal.
- Enable AI feature only for AI-enabled plans.
- Capture business description and category for AI generation after upgrade.

**Out:**

- AI generation for non-AI plans without upgrade.
- Multi-step AI generation beyond shop metadata (logo, hero, URL, name).
- Automatic plan upgrade without merchant consent.

---

### 3) **Key User Journeys**

- **Journey 1:**
    - **Role/Trigger**: Merchant selects AI-assisted setup during Step 2
    - **Behavior/Action**: System detects current plan is not AI-enabled
    - **Value/Outcome**: Plan upgrade modal is displayed
- **Journey 2:**
    - **Role/Trigger**: Merchant chooses to upgrade plan
    - **Behavior/Action**: Merchant confirms upgrade
    - **Value/Outcome**: AI feature becomes available; can generate shop URL, name, logo, and hero section
- **Journey 3:**
    - **Role/Trigger**: Merchant skips plan upgrade
    - **Behavior/Action**: AI feature is disabled permanently
    - **Value/Outcome**: Merchant cannot use AI-assisted generation

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1**: Plan upgrade modal displays correctly for non-AI plan merchants → Pass/Fail
- **BAC 2**: Merchant can upgrade plan and access AI features → Pass/Fail
- **BAC 3**: Merchant skipping the upgrade disables AI feature permanently → Pass/Fail
- **BAC 4**: AI-generated shop metadata (URL, name, logo, hero) is correctly suggested after upgrade → Pass/Fail

---
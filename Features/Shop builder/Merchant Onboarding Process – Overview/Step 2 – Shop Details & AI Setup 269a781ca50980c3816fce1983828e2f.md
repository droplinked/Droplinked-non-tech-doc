# Step 2 – Shop Details & AI Setup

---

## **Step 2 – Shop Details & AI Setup**

### Feature ID:

**ONB-STEP2-002**

### Title:

**Shop Details & AI Setup**

### Category:

**Onboarding / Merchant Setup** | **Actors**: Merchant | **Channel**: Merchant Dashboard

### Status:

**Defined**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**

Merchants need to provide essential shop information to initialize their storefront. Optional AI-assisted setup allows faster creation of shop metadata and design for eligible plans. This step ensures merchants can set up their shop URL, name, and optionally logo, hero section, description, and default products.

**Desired Outcome:**

- Merchant enters shop URL and name (mandatory).
- Optional fields include logo, hero section, and description.
- AI-assisted generation available for eligible plans.
- Merchant can enable **Create Inspirational Inventory** to generate 3 default products.
- URL uniqueness is validated.

---

### 2) **Scope – In / Out**

**In:**

- Enter mandatory shop URL and name.
- Optional logo, hero section, description.
- Enable **Create Inspirational Inventory** toggle (off by default).
- AI-assisted setup:
    - Merchant writes business description and selects business category.
    - System generates shop URL, name, logo, hero section suggestions.
    - Plan upgrade modal displayed if merchant’s plan does not allow AI.
- Validate shop URL uniqueness and suggest alternatives if already taken.

**Out:**

- Automatic AI setup for non-AI plans (merchant must upgrade).
- Multi-language shop setup.
- Integration with existing websites (Yes option from Step 1).

---

### 3) **Key User Journeys**

- **Journey 1:**
    - **Role/Trigger**: Merchant reaches Step 2
    - **Behavior/Action**: Enters shop URL and name
    - **Value/Outcome**: Mandatory information validated; proceed to optional fields
- **Journey 2:**
    - **Role/Trigger**: Merchant optionally adds logo, hero section, description
    - **Behavior/Action**: Fills optional fields
    - **Value/Outcome**: Shop metadata is enriched
- **Journey 3:**
    - **Role/Trigger**: Merchant enables **Create Inspirational Inventory** toggle
    - **Behavior/Action**: System generates 3 default products
    - **Value/Outcome**: Shop has starter products for inspiration
- **Journey 4:**
    - **Role/Trigger**: Merchant uses AI feature
    - **Behavior/Action**: Writes business description, selects category, clicks “Generate with AI”
    - **Value/Outcome**: Shop URL, name, logo, and hero section suggestions generated (only if plan allows)
    - **If plan not AI-enabled**: Plan upgrade modal displayed; AI disabled until upgrade
- **Journey 5:**
    - **Role/Trigger**: Merchant enters shop URL
    - **Behavior/Action**: System validates uniqueness
    - **Value/Outcome**: If URL exists, merchant must modify it

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1**: Shop URL and name are mandatory → Pass/Fail
- **BAC 2**: Optional fields (logo, hero section, description) can be left empty → Pass/Fail
- **BAC 3**: Create Inspirational Inventory toggle generates 3 default products if enabled → Pass/Fail
- **BAC 4**: AI-assisted generation only works for AI-enabled plans → Pass/Fail
- **BAC 5**: Plan upgrade modal displayed correctly if AI feature selected on non-AI plan → Pass/Fail
- **BAC 6**: Shop URL uniqueness validated; system suggests alternatives if taken → Pass/Fail
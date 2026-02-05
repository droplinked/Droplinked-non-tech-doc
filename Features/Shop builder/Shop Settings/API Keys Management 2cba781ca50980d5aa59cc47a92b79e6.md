# API Keys Management

# **Feature ID:** DEV-API-KEY-011

# **Category:** Developer Tools | Settings | **Actors:** Merchant | **Channel:** Web Dashboard

# **Status:** Defined

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants and developers need programmatic access to the Droplinked infrastructure to build custom storefronts, integrate with external applications, or manage their shop data remotely. To do this securely, they require an authentication mechanism.
This feature introduces a **Developers** tab in Settings, allowing merchants to generate, view (one-time), and manage API Keys. It supports keys for both the existing shop and the creation/management of new headless shops.

### **Desired Outcome**

- Provide a "Developers" tab within Store Settings.
- Display a list of active keys with metadata (Name, Type, Partial Key, Expiry, Status).
- Allow creation of API Keys for "Current Shop" or "New Shop".
- Enforce mandatory fields (Name, Logo, URL) when creating a key for a "New Shop".
- Securely display the full API Key **only once** immediately after creation.
- Allow merchants to **Regenerate** a key (invalidating the old one) if lost.
- Allow merchants to **Revoke** a key to permanently disable access.
- Enforce a limit of **1 API Key per Shop**.

---

# **2) Scope – In / Out**

### **In Scope**

- **UI Location:** Settings > Developers.
- **Dashboard View:**
    - Empty State (if no keys exist).
    - Data Table: Name, Type, Partial Key (e.g., `sk_...49b9a6`), Created Date, Expires Date, Status.
- **Creation Flow:**
    - Input: Key Name.
    - Dropdown: Type ("Current Shop" / "New Shop").
    - **Conditional Logic:** If "New Shop" is selected, require: Shop Name, Logo, URL.
    - **Expiration:** Date Picker (Default value = Current Date + 1 Year).
- **Security:**
    - The full API Key (e.g., `a9f88e56-dc6b...`) is displayed in a modal **only once** upon success.
    - Subsequent views only show the Partial Key.
- **Management:**
    - **Regenerate:** Invalidates old key, generates new one, shows new key once.
    - **Revoke:** Deletes/Deactivates the key.
- **Constraints:** Max 1 API Key allowed per shop entity.

### **Out of Scope**

- Granular permission scopes (Read/Write/Admin) – (Assuming "Full Access" for now).
- Analytics or usage logs for the API Key.
- Webhook configuration (handled in a separate feature).

---

# **3) Key User Journeys**

---

### **Journey 1 – Create API Key for Current Shop**

**Step 1:** Merchant navigates to **Settings > Developers**.
**Step 2:** Merchant clicks **"New API Key"**.
**Step 3:** Modal opens. Merchant enters **Name**.
**Step 4:** Selects Type: **"Current Shop"**.
**Step 5:** Checks Expiration Date (default is next year).
**Step 6:** Clicks **Generate**.
**Step 7:** Success Modal appears showing the full API Key (e.g., `a9f88e56...`).
**Step 8:** Merchant copies the key and closes the modal.
**Step 9:** Table updates to show the new key record.

---

### **Journey 2 – Create API Key for New Shop**

**Step 1:** Merchant clicks **"New API Key"**.
**Step 2:** Selects Type: **"New Shop"**.
**Step 3:** Additional fields appear. Merchant fills: **Shop Name**, **Logo**, **URL**.
**Step 4:** Merchant sets Expiration Date.
**Step 5:** Clicks **Generate**.
**Step 6:** Success Modal displays the full API Key once.

---

### **Journey 3 – Regenerate Lost Key**

**Step 1:** Merchant forgets their API key.
**Step 2:** In the Developers table, Merchant clicks **Regenerate** on the existing key.
**Step 3:** System warns: "The old key will stop working immediately."
**Step 4:** Merchant confirms.
**Step 5:** Success Modal displays the **new** full API Key once.
**Step 6:** The old key is invalidated.

---

### **Journey 4 – Revoke Key**

**Step 1:** Merchant wants to remove access.
**Step 2:** Merchant clicks **Revoke** in the table.
**Step 3:** System confirms deletion.
**Step 4:** Key is removed/invalidated.
**Step 5:** Merchant can now create a new key if needed.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** The "Developers" tab must be accessible under Settings.
**BAC 2:** **Table Display:** Must show Name, Type, Partial Key (masked), Created, Expires, and Status.
**BAC 3:** **Creation Logic:**
- "Current Shop" requires Name + Expiry.
- "New Shop" requires Name + Expiry + Shop Name + Logo + URL.
**BAC 4:** **Expiration Default:** The date picker must default to exactly 1 year from the current date.
**BAC 5:** **Security Display:** The full API Key must be visible **only** in the success modal immediately after creation/regeneration. It must never be retrievable again.
**BAC 6:** **Constraint:** A shop cannot have more than 1 active API Key. The "New API Key" button should be disabled or prompt to regenerate if a key already exists for the current shop.
**BAC 7:** **Regenerate:** Must replace the existing key hash with a new one and invalidate the previous key immediately.
**BAC 8:** **Revoke:** Must permanently disable the key.
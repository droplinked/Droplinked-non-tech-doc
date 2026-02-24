# API Key Management

### Feature ID:

**API-KEY-001**

### Title:

**Shop API Key Management – Create, List, Expire**

### Category:

**Developer Platform / Integrations** | **Actors**: Merchant, Developer | **Channel**: Merchant Dashboard (Shop Builder)

### Status:

**Defined**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**

Merchants need a secure way to integrate external systems with Droplinked via authenticated API access. Without API keys, developers cannot call Droplinked APIs safely, and merchants cannot manage or revoke access when needed.

**Desired Outcome:**

- Merchant can access an **API Keys** page, view existing keys, **expire** old keys, and **create** new ones.
- Two create scenarios:
    1. **Current Shop** – only **one** active API key allowed.
    2. **New Shop** – create a shop by providing **name** and **URL** (required) and optional **logo**, then create its API key.
- Every API key must have an **Expire Date**.
- Newly created key is shown **once**; afterwards only a **hashed** representation is displayed.
- External developers can use the API key to access Droplinked APIs; **API documentation** is available and accessible.
- **Traceability**: If a new shop is created through an external API call, it must be linked back to the API key used to create it.

---

### 2) **Scope – In / Out**

**In:**

- **API Keys page** in Merchant Dashboard:
    - List existing API keys for the merchant’s shops.
    - Show metadata: key label, shop name, created date, **expire date**, status (active/expired), and **hashed** key (masked).
- **Create API key**:
    - Scenario A: **Current Shop** – enforce **maximum 1 active key** per current shop.
    - Scenario B: **New Shop** – require **name** and **URL** (mandatory), **logo** (optional), then create key for that shop.
    - **Expire Date** is **mandatory** for all new keys.
    - Show the **plain key exactly once** after creation; require merchant to copy it.
- **Expire API key**:
    - Merchant can expire any active key; expired keys immediately lose access.
- **API Documentation access** link available from the page.
- **Tracking**: Shops created via external API must record which API key was used to create them.

**Out:**

- Key rotation automation and reminders (future phase).
- Fine-grained scopes/permissions per key (future phase).
- Rate limiting and IP allowlists (platform-level features documented elsewhere).
- Regenerating the same key value (keys are immutable; create new if needed).

---

### 3) **Key User Journeys**

- **Journey 1 – View Keys**
    - **Role/Trigger:** Merchant opens **API Keys** page.
    - **Behavior/Action:** System lists keys with masked/hash view, status, created/expire dates.
    - **Value/Outcome:** Merchant has visibility over active/expired credentials.
- **Journey 2 – Create Key for Current Shop**
    - **Role/Trigger:** Merchant clicks **Create Key** for current shop.
    - **Behavior/Action:** If no active key exists, merchant selects **Expire Date** and creates key. If an active key exists, system blocks creation with guidance to expire the existing key first.
    - **Value/Outcome:** Exactly one active key per current shop.
- **Journey 3 – Create Key for New Shop**
    - **Role/Trigger:** Merchant selects **Create for New Shop**.
    - **Behavior/Action:** Merchant enters **Shop Name** (required), **Shop URL** (required), **Logo** (optional), and **Expire Date** (required); system creates shop and its API key.
    - **Value/Outcome:** New shop is created and ready for integration with a valid API key. System records which API key created it if done through external API.
- **Journey 4 – One-Time Display**
    - **Role/Trigger:** After successful key creation.
    - **Behavior/Action:** System shows the **plain API key once** with a “Copy” action and warning that it won’t be shown again.
    - **Value/Outcome:** Merchant securely stores the key; future views are hashed/masked only.
- **Journey 5 – Expire Key**
    - **Role/Trigger:** Merchant clicks **Expire** on an active key.
    - **Behavior/Action:** System asks for confirmation; on confirm, key becomes **Expired** and cannot be used for API calls.
    - **Value/Outcome:** Immediate revocation of access.
- **Journey 6 – Access API Docs**
    - **Role/Trigger:** Merchant clicks **API Documentation** link.
    - **Behavior/Action:** System opens developer docs describing authentication and endpoints.
    - **Value/Outcome:** External developers can implement integrations using the provided key.

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** API Keys page lists all keys with **masked/hash** value, created date, **expire date**, and status (active/expired) → Pass/Fail.
- **BAC 2:** Creating a key for **Current Shop** is **blocked** if an active key already exists; user must expire the existing key first → Pass/Fail.
- **BAC 3:** Creating a key for a **New Shop** requires **name** and **URL** (mandatory) and accepts **logo** (optional) → Pass/Fail.
- **BAC 4:** **Expire Date is mandatory** for every new API key; creation fails without it → Pass/Fail.
- **BAC 5:** The **plain API key** is displayed **exactly once** after creation; subsequent views show only a **hashed** representation → Pass/Fail.
- **BAC 6:** Expiring a key immediately prevents its use for authenticated API calls → Pass/Fail.
- **BAC 7:** API documentation is visible and accessible from the API Keys page → Pass/Fail.
- **BAC 8:** External API calls authenticated with a valid, unexpired key succeed; calls with expired/invalid keys fail with proper errors → Pass/Fail.
- **BAC 9:** When a **new shop** is created via external APIs, the system records and shows **which API key created the shop** for traceability → Pass/Fail.
# Ruleset Engine

**Feature ID:** SF-RULESETS-311

**Category:** Storefront | Web3 Access Control

**Actors:** Customer

**Status:** Defined

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants can attach Web3 rulesets to collections. Storefront must enforce these rules, showing the correct access/discount logic to customers based on their wallet ownership.

### **Desired Outcome**

- Product pages must show a ruleset badge when applicable.
- Storefront must detect customer wallet login state and verify their NFT holdings.
- Ruleset status must update to **Locked** or **Pass**.
- Based on ruleset type (Gated vs Discount), purchasing and pricing logic must follow the rules.

---

# **2) Scope – In / Out**

### **In Scope**

- Displaying ruleset information on product pages
- Detecting wallet login status
- On-chain verification of NFT holdings
- Determining Pass/Locked status
- Applying or removing discounts before checkout
- Enforcing gated purchase restrictions
- Showing NFT purchase link when provided

### **Out of Scope**

- Backend creation or editing of rulesets (covered in Shop Builder PRD)
- Checkout infrastructure
- Wallet login system (covered in Storefront Login PRD)

---

# **3) Key User Journeys**

---

## **Journey 1 – Customer Views Product Page (Ruleset Attached)**

### **UI must show these items inside a “Ruleset Info Box”:**

- **Ruleset Type:**
    - “Gated Access”
    - “Discount Access – X% Off”
- **Ruleset Status:**
    - **Locked** (default)
    - **Pass** (only if NFT validation passed)
- **Gating Message** (required from merchant)
- **NFT Purchase Link** (optional, shown only if merchant provided it)

Status defaults to **Locked** until wallet validation occurs.

---

## **Journey 2 – Customer Logged in With Wallet**

### **Step 1:** Customer logs in with a Web3 wallet (MetaMask, Phantom, Unstoppable, etc.)

### **Step 2:** Storefront fetches wallet address

### **Step 3:** Ruleset Validation Runs:

- Check if wallet holds NFTs from **any** of the contract addresses entered by merchant
- Check if balance meets or exceeds **Minimum Assets Required**

### **Result Outcomes:**

### ❌ If NFT balance < required → **Locked**

- UI shows:
    - “Locked”
    - Still shows ruleset info
    - Cannot buy → **Gated Product**

### ✅ If NFT balance ≥ required → **Pass**

- UI shows:
    - “Pass” badge
    - Updated state dynamically

---

## **Journey 3 – Ruleset Behavior by Type**

---

### **A) GATED Type**

| Status | Behavior |
| --- | --- |
| **Pass** | Customer can purchase product normally |
| **Locked** | Add-to-Cart button disabled → customer **cannot buy** |

---

### **B) DISCOUNT Type**

| Status | Behavior |
| --- | --- |
| **Pass** | Customer can buy + discount must apply automatically |
| **Locked** | Customer can still buy, **but discount does NOT apply** |
- Customer always sees original product price.
- If passed → show discounted price next to original.

---

## **Journey 4 – Customer Not Logged in With Wallet**

If the customer is **not** logged in with a Web3 wallet:

- Ruleset status stays **Locked**
- NFT verification is **not** executed
- Customer sees the full ruleset UI (type, message, status Locked)

### Behavior:

- **GATED:** Cannot buy (Add-to-cart disabled)
- **DISCOUNT:** Can buy normally (without discount)

---

# **4) Business Acceptance Criteria (BAC)**

### **BAC 1:** Product pages must detect if the product belongs to a collection with an active ruleset.

### **BAC 2:** Ruleset Info Box must display:

- Type (Gated or Discount)
- Discount % (if applicable)
- Status (Locked / Pass)
- Gating message
- NFT purchase link (optional)

### **BAC 3:** Customer without wallet login must always show **Locked** state.

### **BAC 4:** Wallet validation must verify:

- NFT balance on the selected blockchain
- One or more contract addresses
- Minimum assets required

### **BAC 5:** When customer **passes** the ruleset:

- Status becomes **Pass**
- UI updates without page refresh

### **BAC 6:** Gated Ruleset:

- If Locked → Add to Cart disabled
- If Pass → Add to Cart enabled

### **BAC 7:** Discount Ruleset:

- Pass → discount must apply
- Locked → customer still buys but without discount

### **BAC 8:** NFT validation must support multiple contract addresses (OR logic).

### **BAC 9:** If NFT purchase link exists, clicking it must open in a new tab.
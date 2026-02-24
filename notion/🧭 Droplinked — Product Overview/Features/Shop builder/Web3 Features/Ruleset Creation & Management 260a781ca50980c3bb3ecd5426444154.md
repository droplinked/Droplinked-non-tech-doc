# Ruleset Creation & Management

**Feature ID:** SB-RULESETS-310

**Category:** Shop Builder | Web3 Access Control

**Actors:** Merchant

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need a simple way to create Web3-based access rules for collections. A “Ruleset” enables:

- **Gated access**: Only customers holding certain digital assets/NFTs can view or purchase products.
- **Discount access**: Customers holding certain assets receive a discount.

This creates Web3-enabled membership, exclusivity, and reward systems.

### **Desired Outcome**

- Merchant can attach a ruleset to any collection.
- Rulesets can be created, viewed, edited, and deleted.
- All products inside the collection inherit the ruleset automatically.
- Storefront displays the ruleset on product pages.

---

# **2) Scope – In / Out**

### **In Scope**

- Viewing the “Ruleset” option inside a collection.
- Creating a ruleset through a modal.
- Editing an existing ruleset.
- Inputs required for Gated or Discount ruleset.
- Validation for required fields.
- Saving and attaching the ruleset to the collection.
- Updating all products in that collection with ruleset metadata.
- Storefront visibility indicator.

### **Out of Scope**

- On-chain validation logic (handled in Storefront Ruleset Engine PRD).
- Checkout discount application (covered in Checkout PRD).
- Complex rule combinations (only one active ruleset per collection).

---

# **3) Key User Journeys**

---

## **Journey 1 – Open Collection & Create a Ruleset**

**Step 1:** Merchant navigates to **Shop Builder → Collections**.

**Step 2:** Selects a collection.

**Step 3:** Clicks **Ruleset** → Ruleset Creation Modal opens.

**Step 4:** Merchant completes required fields and saves the ruleset.

**Step 5:** The ruleset is now attached to the collection.

---

## **Journey 2 – Fill Ruleset Fields**

Merchant sees the following fields in the modal:

### **(A) Blockchain Network – Required**

- Merchant selects a blockchain (ETH, Polygon, etc.).
- Required.

### **(B) Contract Address – Required**

- Merchant must enter **minimum 1 contract address**, no upper limit.
- Allows multiple inputs.
- Must be valid blockchain address format.

### **(C) Gating Message – Required**

- A short message displayed to customers. Example:
    
    *“You must own 1 of these NFTs to access this collection.”*
    

### **(D) Minimum Assets Required – Required**

- Minimum: **1**
- Description:
    
    *Set the minimum number of digital assets/NFTs a user must hold to meet this ruleset.*
    

### **(E) NFT URL – Optional**

- A link where customers can buy the NFT.

### **(F) Ruleset Type (Toggle): Gated OR Discount – Required**

- **Toggle OFF → Gated Ruleset**
- **Toggle ON → Discount Ruleset**

If **Discount** is selected → additional required field appears:

### **(G) Discount Percentage – Required only if Discount Type**

- Integer between **1–99**.

---

## **Journey 3 – Save Ruleset**

**Step 1:** Merchant clicks **Save**.

**Step 2:** Validation runs.

**Step 3:** Ruleset attaches to the selected collection.

**Step 4:** All products inside the collection inherit the ruleset metadata.

---

## **Journey 4 – Edit Ruleset**

**Step 1:** Merchant clicks **Edit Ruleset** on the collection.

**Step 2:** Same modal opens with pre-filled data.

**Step 3:** Merchant edits fields and saves again.

**Step 4:** Updated ruleset is applied across all products in the collection.

---

## **Journey 5 – Storefront Visibility**

For every product inside a collection with a ruleset:

- Product page shows a “Web3 Access Rule” block.
- Shows ruleset type (Gated or Discount).
- Shows the Gating Message.
- Shows a blockchain icon + link to **Verify on Blockchain** (opens in new tab).
- If Discount → discount info is visible.

---

# **4) Business Acceptance Criteria (BAC)**

### **BAC 1:** Merchant must be able to open a collection and see a “Ruleset” action.

### **BAC 2:** Creating a ruleset must require:

- Blockchain Network
- At least 1 Contract Address
- Gating Message
- Minimum Assets Required
- Ruleset Type (Gated or Discount)

### **BAC 3:** If ruleset type is **Discount**, a valid discount percentage (1–99) becomes required.

### **BAC 4:** Merchant must be able to add unlimited contract addresses.

### **BAC 5:** Ruleset must attach to the specific collection; all its products inherit the ruleset automatically.

### **BAC 6:** Edited rulesets must override previous metadata for all products in the collection.

### **BAC 7:** On the storefront, products in a ruleset-enabled collection must show ruleset information.

### **BAC 8:** Gated rulesets and discount rulesets must be clearly differentiated in UI, based on toggle position.
# Product Release Date

# **Feature ID:** IAA-PROD-406

# **Category:** Product Management | **Actors:** Merchant

# **Status:** Released

# **Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need the ability to schedule when a product becomes purchasable in the Shop Frontend. Release Date allows merchants to control product launch timing, create anticipation, and prevent premature purchases. The feature ensures products are visible but not buyable until the scheduled date and displays a countdown timer to customers.

### **Desired Outcome**

- Allow merchants to optionally enable Release Date for any product.
- Ensure only future dates (starting from tomorrow) can be selected.
- Products with Release Date appear in Shop Frontend but are not purchasable until the scheduled date.
- Display a message and a countdown timer indicating when the product becomes available.
- Prevent creating or updating products with a Release Date in the past or today.

---

# **2) Scope – In / Out**

### **In Scope**

- Optional Release Date setting for all product types (Physical, Digital, POD).
- Validation to allow only dates from tomorrow onwards.
- Countdown timer in Shop Frontend for products with Release Date.
- Display a “Not available until [Release Date]” message in Shop Frontend.
- Prevent saving products if Release Date is earlier than tomorrow.

### **Out of Scope**

- Automatic email notifications for upcoming products (handled separately).
- Release date scheduling for draft products (applies only when publishing).
- Bulk release date updates.
- Timezone-specific adjustments (assumes merchant and Shop Frontend share same timezone).

---

# **3) Key User Journeys**

---

### **Journey 1 – Set a Product Release Date**

As a **Merchant**, I can schedule when my product becomes purchasable.

**Step 1:** Merchant opens the product editor.

**Step 2:** Merchant enables the optional Release Date feature.

**Step 3:** Merchant selects a date starting from tomorrow or later.

**Step 4:** Merchant clicks Save or Publish.

**Step 5:** System validates the date; rejects if it is today or in the past.

**Step 6:** Product is saved with the Release Date applied.

---

### **Journey 2 – Customer View Before Release Date**

As a **Customer**, I can see products that are not yet available but know when they will be purchasable.

**Step 1:** Customer opens the Shop Frontend.

**Step 2:** Product with Release Date is visible.

**Step 3:** System displays a message: “Available on [Release Date]”.

**Step 4:** Countdown timer shows remaining time until the product can be purchased.

**Step 5:** “Add to Cart” button is disabled until Release Date is reached.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Release Date must be optional and can be enabled per product.

**BAC 2:** Only dates from tomorrow onward are valid. Attempting to save a product with today or past date must be rejected.

**BAC 3:** Products with a valid Release Date appear in Shop Frontend but are not purchasable until the scheduled time.

**BAC 4:** Countdown timer and “Not available until [Release Date]” message must be displayed in Shop Frontend.

**BAC 5:** Feature must work consistently for Physical, Digital, and POD products.

**BAC 6:** Saving or publishing a product with an invalid Release Date must trigger an error and prevent the operation.
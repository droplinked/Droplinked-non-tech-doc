# 📦 Feature Name: Shipping Management

**Version number:** 1.0.0  
**Date created/updated:** 2025-08-04

---

## 🔍 Description

The Shipping Management feature allows admin users to manage **Shipping Zones** and their associated **Shipping Rates**. This includes the ability to **create**, **edit**, **duplicate**, and **delete** both zones and rates. The feature supports accurate shipping cost configuration based on geographical areas and provides flexibility for handling various shipping methods.

---

## 👤 User Stories

- As an admin, I want to create new shipping zones so that I can organize shipping coverage by region.
- As an admin, I want to define shipping rates within each zone so that I can accurately calculate order delivery costs.
- As an admin, I want to edit, duplicate, or remove existing zones and rates for easy updates and adjustments to our shipping policies.

---

## 🧠 Business Logic

### Step 1: Access Shipping Management

**Logic:**  
Admin navigates to the **Shipping Management** page from the admin panel.

**Rules:**  
- This section is only accessible to users with the "admin" role.

**Validation:**  
- The page should load with a list of existing shipping zones, or an empty state if no zones are present.

**Outcome:**  
Admin sees either the list of zones or an empty state with a **"Add Shipping Profile"** button.

---

### Step 2: Add Shipping Zone

**Logic:**  
Admin clicks on **"Add Shipping Profile"** to open a modal form for creating a new shipping zone.

**Rules:**  
- Zone name is required.
- At least one shipping rate should be added before saving.
- Each rate must include name, cost, and delivery time.

**Validation:**  
- Required fields should be validated before submission.

**Outcome:**  
New shipping zone with defined rates is added to the list.

---

### Step 3: Edit Shipping Zone

**Logic:**  
Admin clicks the three-dot icon (⋯) on a zone card and selects **Edit**.

**Rules:**  
- The modal should open pre-filled with the zone’s current data.
- Admin can update zone name and modify/add/remove shipping rates.

**Outcome:**  
Zone is updated after clicking save.

---

### Step 4: Duplicate Shipping Zone

**Logic:**  
Admin selects **Duplicate** from the three-dot icon menu.

**Rules:**  
- The duplicated zone appears immediately with "(copy)" appended to its name.
- All associated rates are also duplicated.

**Outcome:**  
A new, editable copy of the zone is created.

---

### Step 5: Delete Shipping Zone

**Logic:**  
Admin selects **Remove** from the three-dot icon menu.

**Rules:**  
- A confirmation modal should appear before deletion.

**Outcome:**  
The zone is deleted upon confirmation.

---

### Step 6: Edit or Delete Shipping Rates within a Zone

**Logic:**  
Inside the Shipping Zone modal, each rate has a three-dot icon (⋯) for **Edit** and **Delete**.

**Rules:**  
- Edit opens a form to modify the rate details.
- Delete triggers a confirmation modal before removing the rate.

**Outcome:**  
Rates can be updated or removed individually.

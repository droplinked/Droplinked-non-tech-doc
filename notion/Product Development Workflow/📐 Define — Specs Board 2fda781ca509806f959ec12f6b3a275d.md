# 📐 Define — Specs Board

> Where requirements are documented, designs are created, and test cases are written.
> 

---

## 🎯 Purpose

The **Define** folder transforms accepted requests into detailed specifications. This is where Product, Design, and QA collaborate to create a complete blueprint before development begins.

---

## 📊 Statuses

```
┌─────────┐   ┌───────────────┐   ┌─────────────────┐   ┌─────────────┐
│ WAITING │ → │ WRITING SPEC  │ → │WAITING FOR      │ → │ RESEARCHING │
│         │   │   (DRAFT)     │   │  DESIGN         │   │             │
└─────────┘   └───────────────┘   └─────────────────┘   └──────┬──────┘
                                                               │
┌──────────────────────────────────────────────────────────────┘
│
│   ┌───────────┐   ┌─────────────┐   ┌─────────────────┐   ┌───────────────┐
└─► │ DESIGNING │ → │ SPEC REVIEW │ → │WAITING FOR      │ → │WRITING TEST   │
    │           │   │             │   │  TEST CASES     │   │  CASES        │
    └───────────┘   └─────────────┘   └─────────────────┘   └───────┬───────┘
                                                                    │
                                                            ┌───────▼───────┐
                                                            │ READY FOR DEV │
                                                            └───────────────┘
```

| Status | Description | Owner |
| --- | --- | --- |
| **Waiting** | Ready to start | - |
| **Writing Spec (Draft)** | Initial spec writing | Product/Lead |
| **Waiting for Design** | Queued for design | - |
| **Researching** | UX/UI research | Designer |
| **Designing** | Wireframe/UI creation | Designer |
| **Spec Review** | Review after design | Product/Lead |
| **Waiting for Test Cases** | Queued for QA | - |
| **Writing Test Cases** | Creating test scenarios | QA |
| **Ready for Dev** | Complete, ready for Build | - |

---

## 📋 Spec Ticket Format

Spec tickets are lightweight. The ticket only contains a title, a short problem statement/description, and links to attached docs.

### Ticket Fields

| Field | Type | Description |
| --- | --- | --- |
| **Title** | Short Text | Feature name |
| **Problem Statement / Description** | Long Text | Short summary of the problem |
| **Links (Attachments)** | URLs/Relations | Linked Request, Spec Document, Design, Test Cases |

---

## 🎨 Designer Responsibilities (Must be in linked docs)

Designer must add these items inside the spec document or design file linked from the ticket:

1. **Competitor Review**
- List all competitors reviewed (names + links)
- Quick notes per competitor (what works / what doesn’t)
1. **Conclusion Based on Competitors**
- Final decision and why it’s best for us
1. **Wireframes**
- Low/medium fidelity wireframes linked in ticket
1. **Decision Rationale**
- Explain why key UI/UX decisions were made
- Must be clear enough to revisit later

---

## 📄 Spec Document Structure

Every spec follows this standardized format based on the actual Droplinked documentation:

### Header Section

```markdown
# [Feature Title]

### Feature ID:
**[IAA-XXX-001]**

### Title:
**[Feature Title]**

### Category:
**[Module]** | **Actors**: [User Types] | **Channel**: [Web/Mobile/API]

### Status:
**[Writing Spec (Draft) / Released / etc]**

### Owner:
[Name]
```

### Part 1: Human-Readable Spec

#### 1) Summary

**Problem/Value:**
What problem are we solving? Why does it matter?

**Desired Outcome:**
- [Outcome 1]
- [Outcome 2]
- [Outcome 3]

#### 2) Scope – In / Out

**In:**
- ✅ [Feature item 1]
- ✅ [Feature item 2]

**Out:**
- ❌ [Item not included 1]
- ❌ [Item not included 2]

#### 3) Key User Journeys

**Journey 1: [Name]**
- **Step 1:** [Action]
- **Step 2:** [Action]
- **Step 3:** [Action]

**Journey 2: [Name]**
- **Step 1:** [Action]
- **Step 2:** [Action]

#### 4) Business Acceptance Criteria (BAC)

- **BAC 1:** [Criterion description]
- **BAC 2:** [Criterion description]
- **BAC 3:** [Criterion description]

#### 📜 Change Log

**⚠️ REQUIRED: Update this table for EVERY change made to the spec**

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| YYYY-MM-DD | [Name] | [Description] | [Reason] |

---

### Part 2: Edge Cases & UI Flow

#### Edge Cases & Error Handling

Document edge cases and how the system handles errors:

- **Edge Case 1:** [Description] → [Handling]
- **Edge Case 2:** [Description] → [Handling]
- **Error 1:** [Error scenario] → [Error message/action]

#### UI Flow (Source of Truth)

Simple arrow notation showing screens and user actions:

```
[Screen/Action 1]
    ↓
[Decision Point]
    ├─ [Condition A] → [Screen/Action 2A]
    └─ [Condition B] → [Screen/Action 2B]
                              ↓
                    [Next Screen/Action]
```

**Example:**
```
[Click "Billing" in Sidebar]
    ↓
[Check Subscription]
    ├─ Starter + No History → [Empty State] → [View Plans] → [Pricing Page]
    └─ Has Plan/History → [Billing Page]
                              ↓
                    [Overview Tab (Default)]
                              ├─ [View Current Plan Info]
                              ├─ [View Payment Details]
                              └─ [View Plans] → [Pricing Page]
                              ↓
                    [Invoices Tab]
                              ├─ [Search Invoices]
                              └─ [Click Invoice Row] → [Invoice Detail Page]
```

---

### For Large Features: Overview + Sub-features

**Overview Document** (at category level):
```markdown
# [Feature Category]

[Sub-feature 1]([Category]/[Sub-feature 1].md)
[Sub-feature 2]([Category]/[Sub-feature 2].md)
```

**Sub-feature Documents** (in folder):
- Each major screen/page gets its own detailed spec
- Full Part 1 and Part 2 sections
- Linked from overview

Example:
```
Subscription/
├── Subscription.md (Overview with links)
└── Subscription/
    ├── Pricing Page.md (Full spec)
    ├── Billing Page.md (Full spec)
    └── Invoice Detail Page.md (Full spec)
```

## 🧪 Test Case Format (with example)

Keep test cases short, clear, and testable. Always break the feature into **small, independent tasks** before writing test cases.

### ✅ How to break tasks

1. **Feature → Sub-features** (e.g., Product Type Toggle)
2. **Sub-feature → Scenarios** (default state, change state, error state)
3. **Scenario → Test Cases** (one action, one expected result)

### ✅ Test Case Header (example)

**1.3 Product Type Toggle**

- **Total TCs:** 3
- **Tester:** زهرا
- **Status:** ✅ Pass (3/3)
- **Environment:** Staging
- **Last Sprint:** Sprint 14
- **Date:** 1404/02/02

### ✅ Test Case Table (example)

| TC ID | Scenario Title | Type | Precondition | Given | When | Then | Expected Result | Test Data | Status | Bug |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-PRODUCT-1.3-001 | Verify Default Physical | UI | User logged in | Opens create page | Page loads | Physical selected | Toggle shows Physical | - | ✅ Pass | - |
| TC-PRODUCT-1.3-002 | Verify Toggle to Digital | Functional | On create page, Physical selected | Clicks Digital | Form switches | Digital form shows | Digital form shows | - | ✅ Pass | - |
| TC-PRODUCT-1.3-003 | Verify Toggle to Physical | Functional | On create page, Digital selected | Clicks Physical | Form switches | Physical form shows | Physical form shows | - | ✅ Pass | - |

**Rules:**

- One test case = one action + one expected result
- Use clear Given/When/Then for every row
- Status must be filled (✅ Pass / ❌ Fail / ⏳ Blocked)

## 📝 Example Spec Ticket

### SPEC: Product Image Upload System

### Problem Statement / Description

Merchants need an intuitive way to upload multiple product images with drag-and-drop support, reordering capability, and clear feedback during upload.

### Links (Attachments)

- 🔗 **Linked Request:** REQ-089
- 📄 **Spec Document:** [Link to Notion/Docs]
- 🎨 **Design:** [Link to Figma]
- 🧪 **Test Cases:** [Link to Test Sheet]

---

## 📈 Define Phase Flow

```
                            ┌─────────────────────────────┐
                            │      Request Accepted       │
                            │       (from Intake)         │
                            └─────────────┬───────────────┘
                                          │
                                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              DEFINE PHASE                                   │
│                                                                             │
│  ┌─────────┐    ┌──────────────┐    ┌─────────────┐    ┌──────────────┐    │
│  │ WAITING │ →  │WRITING SPEC  │ →  │WAITING FOR  │ →  │ RESEARCHING  │    │
│  │         │    │  (DRAFT)     │    │  DESIGN     │    │              │    │
│  └─────────┘    └──────────────┘    └─────────────┘    └──────┬───────┘    │
│                                                                │           │
│       Product/Lead writes                            Designer  │           │
│       initial spec                                  researches │           │
│                                                                ▼           │
│  ┌───────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────┐    │
│  │ READY FOR DEV │ ←  │WRITING TEST │ ←  │WAITING FOR  │ ← s │DESIGNING│   │   │
│  │      ✅      │    │   CASES     │    │ TEST CASES  │    │         │    │
│  └───────┬───────┘    └─────────────┘    └─────────────┘    └─────────┘    │
│          │                   │                  │                │         │
│          │                  QA              Spec updated    Wireframe/UI   │
│          │            writes tests         after design     created        │
│          │                                                                 │
└──────────┼─────────────────────────────────────────────────────────────────┘
           │
           ▼
    ┌─────────────────┐
    │   BUILD PHASE   │
    │  (Development)  │
    └─────────────────┘
```

---

## 🔗 Relations

| This Board | Relation | Target Board |
| --- | --- | --- |
| Spec | ← Request | Intake |
| Spec | → Tasks | Build |

When a spec is **Ready for Dev**, it generates multiple **Epic** tickets in the Build folder.

---

## ✅ Checklist Before "Ready for Dev"

- [ ]  Problem statement is clear
- [ ]  User stories are complete
- [ ]  Scope is defined (in/out)
- [ ]  Acceptance criteria are testable
- [ ]  Design is complete and approved
- [ ]  Test cases are written
- [ ]  Dependencies are identified
- [ ]  UI Flow is documented (source of truth for test cases)
- [ ]  **Change Log is updated with all modifications**

---

## ❓ FAQ

**Q: Who writes the spec?**

A: Product Manager or Tech Lead writes the initial spec.

**Q: When does design start?**

A: After the initial spec draft is complete.

**Q: Can we skip the design phase?**
A: Only for backend-only features with no UI impact.
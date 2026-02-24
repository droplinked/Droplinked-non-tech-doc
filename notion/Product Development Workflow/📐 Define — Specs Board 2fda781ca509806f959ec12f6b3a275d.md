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

Every spec follows this standardized format:

### Feature Specification

- **Feature Title:** [Name]
- **Feature ID:** [IAA-STM-001]
- **Category:** [Module]
- **Actors:** [User Types]
- **Channel:** [Web/Mobile/API]
- **Status:** Defined
- **Owner:** [Name]

### Part 1: Human-Readable Spec

### Problem Statement

What problem are we solving? Why does it matter?

### User Stories

As a [user type], I want [action] so that [benefit].

### Key User Journeys

Step-by-step flow of how users interact with feature.

### Scope

- ✅ **In Scope:** What's included
- ❌ **Out of Scope:** What's NOT included

### Acceptance Criteria

- ☑ Criterion 1
- ☑ Criterion 2
- ☑ Criterion 3

### Technical Notes

High-level technical considerations (no implementation).

### Dependencies

Other features or systems this depends on.

---

## UI Flow (Source of Truth)

Document the user journey through the feature. Simple arrow notation showing screens and actions.

### Example

```
Dashboard → Click "Add Product" → Product Form → Fill Details →
Click "Save" → Success Message → Product List
```

---

### Attachments

- 📎 Linked Tickets: [List of related tickets]

### Change Log

- 2024-03-15 — Sarah — Initial draft
- 2024-03-17 — Mike — Added edge cases

---

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

---

## ❓ FAQ

**Q: Who writes the spec?**

A: Product Manager or Tech Lead writes the initial spec.

**Q: When does design start?**

A: After the initial spec draft is complete.

**Q: Can we skip the design phase?**
A: Only for backend-only features with no UI impact.
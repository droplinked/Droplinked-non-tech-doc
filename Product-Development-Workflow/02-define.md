# 📐 Define — Specs Board

> Where requirements are documented, designs are created, and test cases are written.

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
|--------|-------------|-------|
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

### Ticket Fields

| Field | Type | Description |
|-------|------|-------------|
| **Title** | Short Text | Feature name |
| **Linked Request** | Relation | Link to original Request |
| **Status** | Status | Current phase |
| **Spec Link** | URL | Link to full spec document |
| **Design Link** | URL | Link to Figma/design files |
| **Test Case Link** | URL | Link to test case document |

---

## 🎨 Designer Responsibilities (Must be in ticket)

Designer must add these items inside the spec ticket (or linked doc):

1) **Competitor Review**
- List all competitors reviewed (names + links)
- Quick notes per competitor (what works / what doesn’t)

2) **Conclusion Based on Competitors**
- Final decision and why it’s best for us

3) **Wireframes**
- Low/medium fidelity wireframes linked in ticket

4) **Decision Rationale**
- Explain why key UI/UX decisions were made
- Must be clear enough to revisit later

---

## 📄 Spec Document Structure

Every spec follows this standardized format:

```
═══════════════════════════════════════════════════════════
FEATURE SPECIFICATION
═══════════════════════════════════════════════════════════

Feature Title: [Name]
Feature ID: [IAA-STM-001]
Category: [Module] | Actors: [User Types] | Channel: [Web/Mobile/API]
Status: Defined
Owner: [Name]

═══════════════════════════════════════════════════════════
PART 1: HUMAN-READABLE SPEC
═══════════════════════════════════════════════════════════

┌─────────────────────────────────────────────────────────┐
│ PROBLEM STATEMENT                                        │
├─────────────────────────────────────────────────────────┤
│ What problem are we solving? Why does it matter?        │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ USER STORIES                                             │
├─────────────────────────────────────────────────────────┤
│ As a [user type], I want [action] so that [benefit]     │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ KEY USER JOURNEYS                                        │
├─────────────────────────────────────────────────────────┤
│ Step-by-step flow of how users interact with feature    │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ SCOPE                                                    │
├─────────────────────────────────────────────────────────┤
│ ✅ In Scope: What's included                            │
│ ❌ Out of Scope: What's NOT included                    │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ ACCEPTANCE CRITERIA                                      │
├─────────────────────────────────────────────────────────┤
│ ☑ Criterion 1                                           │
│ ☑ Criterion 2                                           │
│ ☑ Criterion 3                                           │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ TECHNICAL NOTES                                          │
├─────────────────────────────────────────────────────────┤
│ High-level technical considerations (no implementation) │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ DEPENDENCIES                                             │
├─────────────────────────────────────────────────────────┤
│ Other features or systems this depends on               │
└─────────────────────────────────────────────────────────┘

═══════════════════════════════════════════════════════════
PART 2: AI-CENTRIC LAYER (Functional Logic Depth)
═══════════════════════════════════════════════════════════

This section provides detailed explanations for AI-assisted
development. NO technical implementation details.

┌─────────────────────────────────────────────────────────┐
│ DEFINITIONS & GLOSSARY                                   │
├─────────────────────────────────────────────────────────┤
│ Term definitions used in this feature                   │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ EXHAUSTIVE FUNCTIONAL LOGIC                              │
├─────────────────────────────────────────────────────────┤
│ Detailed step-by-step logic explanations                │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ EDGE CASES & ERROR HANDLING                              │
├─────────────────────────────────────────────────────────┤
│ All possible edge cases and how to handle them          │
└─────────────────────────────────────────────────────────┘

⚠️ NOTE: Do NOT include:
- Database schemas
- API routes
- Component architecture
- Code snippets

═══════════════════════════════════════════════════════════
ATTACHMENTS
═══════════════════════════════════════════════════════════

📎 Linked Tickets: [List of related tickets]

═══════════════════════════════════════════════════════════
CHANGE LOG
═══════════════════════════════════════════════════════════

| Date | Author | Change |
|------|--------|--------|
| 2024-03-15 | Sarah | Initial draft |
| 2024-03-17 | Mike | Added edge cases |

═══════════════════════════════════════════════════════════
```

---

## 🧪 Test Case Format (with example)

Keep test cases short, clear, and testable. Always break the feature into **small, independent tasks** before writing test cases.

### ✅ How to break tasks
1) **Feature → Sub-features** (e.g., Product Type Toggle)
2) **Sub-feature → Scenarios** (default state, change state, error state)
3) **Scenario → Test Cases** (one action, one expected result)

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
|------|----------------|------|--------------|-------|------|------|-----------------|-----------|--------|-----|
| TC-PRODUCT-1.3-001 | Verify Default Physical | UI | User logged in | Opens create page | Page loads | Physical selected | Toggle shows Physical | - | ✅ Pass | - |
| TC-PRODUCT-1.3-002 | Verify Toggle to Digital | Functional | On create page, Physical selected | Clicks Digital | Form switches | Digital form shows | Digital form shows | - | ✅ Pass | - |
| TC-PRODUCT-1.3-003 | Verify Toggle to Physical | Functional | On create page, Digital selected | Clicks Physical | Form switches | Physical form shows | Physical form shows | - | ✅ Pass | - |

**Rules:**
- One test case = one action + one expected result
- Use clear Given/When/Then for every row
- Status must be filled (✅ Pass / ❌ Fail / ⏳ Blocked)

## 📝 Example Spec Ticket

### SPEC: Product Image Upload System

- **Feature ID:** IAA-STM-401
- **Status:** Designing
- **Owner:** Behdad
- **Category:** Shop Settings
- **Actors:** Merchant (Admin), Merchant (Member)
- **Channel:** Web

#### Links

- 🔗 **Linked Request:** REQ-089
- 📄 **Spec Document:** [Link to Notion/Docs]
- 🎨 **Design:** [Link to Figma]
- 🧪 **Test Cases:** [Link to Test Sheet]

#### Problem Statement

Merchants need an intuitive way to upload multiple product images with drag-and-drop support, reordering capability, and clear feedback during upload.

#### User Stories

- As a merchant, I want to upload multiple images at once so that I can save time.
- As a merchant, I want to reorder images by dragging so that I can set the main product image easily.
- As a merchant, I want to see upload progress so that I know when uploads are complete.

#### Acceptance Criteria

- ☑ Drag & drop from desktop works
- ☑ Click to open file picker works
- ☐ Preview shows before upload completes
- ☐ Progress bar during upload
- ☐ Delete button on each image
- ☐ Drag to reorder images
- ☐ First image marked as "Main"
- ☐ Error for invalid format
- ☐ Error for files > 5MB
- ☐ Error for more than 10 images

**Progress:** 2/10

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
|------------|----------|--------------|
| Spec | ← Request | Intake |
| Spec | → Tasks | Build |

When a spec is **Ready for Dev**, it generates multiple **Epic** tickets in the Build folder.

---

## ✅ Checklist Before "Ready for Dev"

- [ ] Problem statement is clear
- [ ] User stories are complete
- [ ] Scope is defined (in/out)
- [ ] Acceptance criteria are testable
- [ ] Design is complete and approved
- [ ] Test cases are written
- [ ] Dependencies are identified
- [ ] AI-centric layer is documented

---

## ❓ FAQ

**Q: Who writes the spec?**  
A: Product Manager or Tech Lead writes the initial spec.

**Q: When does design start?**  
A: After the initial spec draft is complete.

**Q: What goes in the AI-centric layer?**  
A: Detailed functional logic, edge cases, and definitions — but NO technical implementation details (no database schemas, routes, or code).

**Q: Can we skip the design phase?**  
A: Only for backend-only features with no UI impact.

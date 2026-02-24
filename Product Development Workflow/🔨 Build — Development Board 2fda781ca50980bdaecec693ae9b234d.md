# 🔨 Build — Development Board

> Where specs become code through organized epics, tasks, and code reviews.
> 

---

## 🎯 Purpose

The **Build** folder is where development happens. Specs from the Define phase are broken down into **Epics** and **Subtasks** that developers work on through a structured workflow.

---

## 📊 Statuses

```
┌──────────┐   ┌─────────┐   ┌─────────────┐   ┌─────────────┐   ┌──────────────┐
│ BACKLOG  │ → │  TO DO  │ → │ IN PROGRESS │ → │ CODE REVIEW │ → │READY FOR TEST│
└──────────┘   └─────────┘   └─────────────┘   └──────┬──────┘   └──────────────┘
     ⚪             🔵             🟡               🟣                  🟠
                                                    │
                                                    ▼
                                            ┌──────────────┐
                                            │REVIEW FAILED │
                                            └──────────────┘
                                                   🔴

┌──────────────┐   ┌─────────┐   ┌────────────┐   ┌────────┐
│READY FOR TEST│ → │ TESTING │ → │TEST FAILED │   │  DONE  │
└──────────────┘   └─────────┘   └────────────┘   └────────┘
       🟠              🟣              🔴             🟢
                        │                             ▲
                        └─────────────────────────────┘
                              (if tests pass)
```

| Status | Description | Color |
| --- | --- | --- |
| **Backlog** | Ready for next sprint | ⚪ Gray |
| **To Do** | In current sprint | 🔵 Blue |
| **In Progress** | Being worked on | 🟡 Yellow |
| **Code Review** | Waiting for review | 🟣 Purple |
| **Review Failed** | Needs fixes | 🔴 Red |
| **Ready for Test** | Code done, awaiting test | 🟠 Orange |
| **Testing** | Being tested | 🟣 Purple |
| **Test Failed** | Test didn't pass | 🔴 Red |
| **Done** | Approved and complete | 🟢 Green |

---

## 🏗️ Structure: Specs → Epics → Subtasks

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        ONE SPEC (from Define)                           │
└─────────────────────────────────────┬───────────────────────────────────┘
                                      │
                                      │ generates
                                      ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         MULTIPLE EPICS                                  │
│                                                                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐          │
│  │ Epic: Frontend  │  │ Epic: Backend   │  │ Epic: AI/ML     │          │
│  │ Product Page    │  │ Product Page    │  │ Product Page    │          │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘          │
│           │                    │                    │                   │
│           ▼                    ▼                    ▼                   │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐          │
│  │   Subtasks:     │  │   Subtasks:     │  │   Subtasks:     │          │
│  │ • Layout        │  │ • Create API    │  │ • ML Model      │          │
│  │ • Form          │  │ • Upload API    │  │ • Integration   │          │
│  │ • Validation    │  │ • Validation    │  │ • Testing       │          │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘          │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Conversion Rules: Spec → Epics

| Spec Contains | Creates Epic For |
| --- | --- |
| UI Components | Frontend |
| API/Database | Backend |
| ML/AI Features | AI Team |
| Smart Contracts | Blockchain |

---

## 📦 Epic Types

| Type | Description | Example |
| --- | --- | --- |
| **Feature** | New capability | Product creation page |
| **Enhancement** | Improve existing | Speed up product list |
| **Refactor** | Code rewrite | Refactor payment module |
| **Bugfix** | Fix related bugs | Fix order page bugs |

---

## 📋 Epic Ticket Format

### EPIC: Frontend - Product Creation Page

- **ID:** EPIC-012
- **Type:** Feature
- **Labels:** Frontend
- **Linked Spec:** SPEC-018
- **Estimated:** 48h
- **Reviewer:** [Name]
- **Assignee:** [Team/Person]

### Description

Complete frontend implementation for product creation page:

- Physical and digital product forms
- Image and file upload
- Preview and submit functionality

### Subtasks

1. Page Structure & Layout — Assignee: Alex | Est: 4h | Status: ✅ Done
2. Basic Info Form — Assignee: Alex | Est: 4h | Status: ✅ Done
3. Image Upload with Drag — Assignee: Sara | Est: 6h | Status: 🔄 In Progress
4. Product Type Toggle — Assignee: Alex | Est: 2h | Status: 🔄 In Progress
5. Physical Product Form — Assignee: Mike | Est: 4h | Status: 📋 To Do
6. Digital Product Form — Assignee: Sara | Est: 4h | Status: 📋 To Do
7. Pricing Section — Assignee: Mike | Est: 3h | Status: 📋 To Do
8. Categories & Tags — Assignee: Mike | Est: 3h | Status: 📋 To Do
9. SEO Section — Assignee: Sara | Est: 3h | Status: 📋 To Do
10. Product Preview — Assignee: Alex | Est: 4h | Status: 📋 To Do
11. Submit Buttons — Assignee: Alex | Est: 2h | Status: 📋 To Do
12. Validation & Errors — Assignee: Sara | Est: 4h | Status: 📋 To Do
13. Mobile Responsive — Assignee: Mike | Est: 4h | Status: 📋 To Do

### Attachments

- 📎 Test Coverage Image
- 📎 Design Reference

---

## 📋 Subtask Ticket Format

### SUBTASK: Image Upload with Preview & Drag-Drop

- **Parent Epic:** EPIC-012 (Frontend - Product Creation Page)
- **Status:** In Progress
- **Estimated:** 6h
- **Assignee:** Sara

### Description

Image upload component for products:

- Multi-image upload
- Drag & drop from desktop
- Preview before upload
- Delete images
- Reorder by dragging
- First image = main image

### Technical Notes

[Implementation details, libraries, approach]

### Acceptance Criteria

- ☑ Drag & drop files from desktop works
- ☑ Click on box opens file picker
- ☐ Preview shows before upload completes
- ☐ Progress bar during upload
- ☐ × button to delete each image
- ☐ Drag to reorder images
- ☐ First image shows "Main" badge
- ☐ Error for invalid format
- ☐ Error for files > 5MB
- ☐ Error for more than 10 images

**Progress:** 2/10

### Design Reference

🎨 [Link to Figma]

### Test Results

- **Test Result:** -
- **Test Notes:** -
- **Failed Test Cases:** -

---

## 📏 Work Breakdown Rules

| If Subtask... | Then... |
| --- | --- |
| More than 8 hours | Break into smaller subtasks |
| Requires multiple skills | Create separate Epic per skill |
| Is a one-line change | Don't create subtask, include in existing work |

---

## ✅ Code Review Checklist

| Check | Description |
| --- | --- |
| 📖 Clean Code? | Readable and organized |
| ⚡ Works? | Tested and functional |
| 🎨 Matches Design? | Figma checked |
| 🔀 Edge Cases? | Special scenarios handled |
| 🚀 Performance? | Not slow or inefficient |

---

## 🧪 Testing Flow

```
Before marking as "Done":

┌─────────────────────────────────────────────────────────┐
│                    TESTING FLOW                         │
│                                                         │
│   ┌──────────────┐                                      │
│   │  Assignee    │ ─── Self-tests the code              │
│   │  tests       │                                      │
│   └──────┬───────┘                                      │
│          │                                              │
│          ▼                                              │
│   ┌──────────────┐                                      │
│   │  Reviewer    │ ─── Code review + test               │
│   │  tests       │                                      │
│   └──────┬───────┘                                      │
│          │                                              │
│          ▼                                              │
│   ┌──────────────┐                                      │
│   │  QA tests    │ ─── Tests on Staging                 │
│   │  on Staging  │                                      │
│   └──────────────┘                                      │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 🌿 Branch Naming Convention

```
feature/short-description     ← New feature
fix/short-description         ← Bug fix
refactor/short-description    ← Code refactor
hotfix/short-description      ← Urgent fix
```

### Examples

```
feature/product-image-upload
feature/checkout-flow
fix/cart-total-calculation
refactor/payment-module
hotfix/login-crash
```

---

## 🔗 Relations

| This Board | Relation | Target Board |
| --- | --- | --- |
| Task | ← Spec | Define |
| Task | → Release | Deliver |

Multiple tasks from the Build phase go into a single **Release** in the Deliver phase.

---

## 📈 Complete Build Flow

```
┌────────────────────────────────────────────────────────────────────────────┐
│                              BUILD PHASE                                   │
│                                                                            │
│     Spec Ready                                                             │
│   (from Define)                                                            │
│         │                                                                  │
│         ▼                                                                  │
│   ┌───────────┐    Create Epics for each team                              │
│   │   SPEC    │ ──────────────────────────────────────────┐                │
│   └───────────┘                                           │                │
│                                                           ▼                │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │                          EPICS                                      │  │
│   │                                                                     │  │
│   │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────┐         │  │
│   │  │ Frontend │  │ Backend  │  │   AI     │  │ Blockchain   │         │  │
│   │  │  Epic    │  │  Epic    │  │  Epic    │  │    Epic      │         │  │
│   │  └────┬─────┘  └────┬─────┘  └────┬─────┘  └──────┬───────┘         │  │
│   │       │             │             │               │                 │  │
│   │       ▼             ▼             ▼               ▼                 │  │
│   │   Subtasks      Subtasks      Subtasks        Subtasks              │  │
│   │                                                                     │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                           │                                │
│                                           ▼                                │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │                        SUBTASK WORKFLOW                             │  │
│   │                                                                     │  │
│   │  BACKLOG → TO DO → IN PROGRESS → CODE REVIEW → READY FOR TEST       │  │
│   │                                       │                             │  │
│   │                                       ▼                             │  │
│   │                              ┌──────────────┐                       │  │
│   │                              │REVIEW FAILED │ → back to IN PROGRESS │  │
│   │                              └──────────────┘                       │  │
│   │                                                                     │  │
│   │  READY FOR TEST → TESTING → DONE                                    │  │
│   │                      │                                              │  │
│   │                      ▼                                              │  │
│   │              ┌──────────────┐                                       │  │
│   │              │ TEST FAILED  │ → back to IN PROGRESS                 │  │
│   │              └──────────────┘                                       │  │
│   │                                                                     │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                           │                                │
│                                           ▼                                │
│                                    ┌─────────────┐                         │
│                                    │   RELEASE   │                         │
│                                    │  (Deliver)  │                         │
│                                    └─────────────┘                         │
│                                                                            │
└────────────────────────────────────────────────────────────────────────────┘
```

---

## ❓ FAQ

**Q: How do we estimate subtasks?**

A: Use hours. Anything over 8 hours should be broken down.

**Q: Who assigns the reviewer?**

A: The Epic defines the reviewer, usually the tech lead.

**Q: What if code review fails?**

A: Status goes to "Review Failed", fix issues, then back to "Code Review".

**Q: When does a task move to Done?**

A: After self-test, code review, and QA approval on staging.
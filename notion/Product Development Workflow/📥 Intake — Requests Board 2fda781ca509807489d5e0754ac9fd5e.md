# 📥 Intake — Requests Board

> The entry point for all new feature requests, ideas, and improvements.
> 

---

## 🎯 Purpose

The **Intake** folder is where all product requests begin their journey. Anyone in the organization can submit a request, which then goes through evaluation before moving to the Define phase.

---

## 📊 Statuses

```
┌─────────┐    ┌────────────┐    ┌──────────┐
│   NEW   │ →  │ EVALUATING │ → v│ ACCEPTED │ → To Define
└─────────┘    └─────┬──────┘    └──────────┘
                     │
         ┌───────────┴───────────┐
         │                       │
    ┌────▼────┐            ┌─────▼────┐
    │ REJECTED│            │  LATER   │
    └─────────┘            └──────────┘
```

| Status | Description | Next Action |
| --- | --- | --- |
| **New** | Request just submitted | Needs evaluation |
| **Evaluating** | Under review | Waiting for decision |
| **Accepted** | Approved | Moves to Define phase |
| **Rejected** | Not approved | Closed with feedback |
| **Later** | Postponed | Will be reviewed later |

---

## 📋 Request Ticket Format

### Ticket Title

```
[Requester Name] - [Short Title]
```

**Example:** `Sarah - Add Export to CSV Feature`

---

### Required Fields

| Field | Type | Description |
| --- | --- | --- |
| **Request Date** | Date | When the request was submitted |
| **Description** | Long Text | What is being requested? Clear and concise |
| **Why / Problem** | Long Text | Why is this needed? What problem does it solve? |
| **Attachments** | Files | Supporting files, screenshots, voice notes, etc. |

---

## 📝 Example Request Ticket

### REQUEST: Sarah - Add Export to CSV Feature

- **Status:** Evaluating
- **Request Date:** 2024-03-15

### Description

Need the ability to export order reports to CSV format for accounting purposes.

### Why / Problem

Currently, our accounting team manually copies data from the dashboard. This takes ~2 hours weekly and is error-prone. CSV export would save time and reduce errors.

### Attachments

- 📎 current-workflow-screenshot.png
- 📎 sample-csv-format.xlsx

---

## ✅ Best Practices

### For Requesters

- ✅ Keep descriptions **short and to the point**
- ✅ Clearly explain the **problem**, not just the solution
- ✅ Attach **examples** or **screenshots** when possible
- ✅ Be specific about **who** needs this and **why**

### For Evaluators

- ✅ Respond within maximum **3 business days**
- ✅ If rejecting, provide **clear reasoning**
- ✅ If marking as "Later", set a **review date**
- ✅ When accepting, create linked **Spec ticket**

---

## 🔗 Relations

| This Board | Relation | Target Board |
| --- | --- | --- |
| Request | → Spec | Define |

When a request is **Accepted**, a corresponding **Spec** ticket is created in the Define folder and linked back to this request.

---

## 📈 Flow Diagram

```
   ┌──────────────────────────────────────────────────────────────┐
   │                         INTAKE                               │
   │                                                              │
   │   User submits         Product team        Decision made     │
   │   request              evaluates                             │
   │       │                    │                    │            │
   │       ▼                    ▼                    ▼            │
   │   ┌───────┐          ┌──────────┐         ┌─────────┐        │
   │   │  NEW  │ ───────► │EVALUATING│ ───────►│ACCEPTED │────────┼──► Define
   │   └───────┘          └────┬─────┘         └─────────┘        │
   │                           │                                  │
   │               ┌───────────┴───────────┐                      │
   │               ▼                       ▼                      │
   │          ┌─────────┐            ┌─────────┐                  │
   │          │REJECTED │            │  LATER  │                  │
   │          └─────────┘            └─────────┘                  │
   │              │                       │                       │
   │              ▼                       ▼                       │
   │           Closed              Review Later                   │
   │                                                              │
   └──────────────────────────────────────────────────────────────┘
```

---

## ❓ FAQ

**Q: Who can submit a request?**

A: Anyone in the organization can submit requests.

**Q: How long does evaluation take?**

A: Typically 1-3 business days.

**Q: What happens after acceptance?**

A: A Spec is created in the Define folder, and the requester is notified.

**Q: Can I resubmit a rejected request?**

A: Yes, with significant changes or new information.
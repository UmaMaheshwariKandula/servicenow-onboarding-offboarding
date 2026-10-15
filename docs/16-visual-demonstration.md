# Visual Demonstration

This document provides annotated screenshots that visually demonstrate the working system across the full request lifecycle.

---

## 1. Objective

Provide visual proof that the Automated Employee Onboarding & Offboarding system works end-to-end — from portal submission to lifecycle closure.

---

## 2. Screenshots Overview

All screenshots are stored in `Screenshots/phase5/visual-demo/`.

| # | Screenshot | What It Shows |
|---|-----------|---------------|
| 1 | `01-portal-form.png` | Service Portal form with 13 dynamic variables |
| 2 | `02-form-filled.png` | Form filled with onboarding request data |
| 3 | `03-request-submitted.png` | Request Summary page with REQ number |
| 4 | `04-approval-record.png` | RITM Approvers related list — pending approval |
| 5 | `05-approval-approved.png` | Approval approved (banner confirmation) |
| 6 | `06-tasks-created.png` | 3 catalog tasks auto-created (IT, Facilities, Security) |
| 7 | `07-execution-log.png` | Flow Execution Details — all actions completed |
| 8 | `08-lifecycle-completed.png` | Lifecycle record with Status = Completed |
| 9 | `09-ritm-complete.png` | RITM with 3 green checkmarks (stage complete) |
| 10 | `10-email-action.png` | Flow's Send Email action configuration |

---

## 3. Detailed Walkthrough

### Screenshot 1 — Portal Form with Dynamic Variables

The Service Portal form displays all 13 variables from the "Employee Onboarding" catalog item:
- Request Type, Employee ID, Requested For
- Department, Manager
- Joining Date / Exit Date (conditionally shown)
- Assets needed, Assets to Collect
- Special Access, Access Revocations
- Special Instructions, Access Revocation Required

Mandatory fields are marked with an asterisk and enforced by ServiceNow.

---

### Screenshot 2 — Form Filled

The form filled with a real onboarding request:
- Request Type = Onboarding
- Employee ID = EMP002
- Requested For = System Administrator
- Department = IT
- Manager = Aileen Mottern
- Joining Date = 2026-10-30
- Assets needed = Laptop

---

### Screenshot 3 — Request Submitted

After clicking **Order Now**, the Request Summary page shows:
- Request Number: REQ0010002
- Item: Employee Onboarding
- Stage: Request Approval
- Estimated Delivery date

---

### Screenshot 4 — Approval Record

Opening the RITM shows the **Approvers** related list:
- State: Requested
- Approver: Aileen Mottern (the manager variable)
- Created: 2026-10-15 02:47:19

Proves the approval was routed dynamically to the correct manager.

---

### Screenshot 5 — Approval Approved

After clicking Approve, the RITM banner confirms:
> **Approved Requested Item: RITM0010002**

The flow's "If Approval State is Approved" branch triggers → tasks created.

---

### Screenshot 6 — Tasks Created

The RITM's **Catalog Tasks** related list shows 3 tasks auto-created:

| Number | Assignment Group | Short Description |
|--------|------------------|-------------------|
| SCTASK0010005 | IT – Account Provisioning | Create accounts & assign hardware |
| SCTASK0010006 | Facilities – Workspace | Prepare workspace |
| SCTASK0010007 | Security – Access | Issue ID badge & building access |

Each task is linked to the RITM and has its own assignment group.

---

### Screenshot 7 — Flow Execution Log

Flow Designer's **Execution Details** page shows each action's state:

| # | Action | State |
|---|--------|-------|
| 1 | Get Catalog Variables | Completed |
| 2 | Create Record | Completed |
| 3 | Ask For Approval | Completed |
| 4 | If (Approved) | Evaluated |
| 5-7 | Create 3 tasks | Completed |
| 8 | Wait For Condition | Completed |
| 9 | Update Lifecycle | Completed |
| 10 | Update RITM | Completed |
| 11 | Send Email | Completed |

Confirms the flow executed end-to-end without errors.

---

### Screenshot 8 — Lifecycle Record Completed

The `u_employee_lifecycle` list shows the record:

| Field | Value |
|-------|-------|
| Employee ID | EMP002 |
| Department | IT |
| Manager | Aileen Mottern |
| Joining Date | 2026-10-30 |
| Request Type | Onboarding |
| Status | **Completed** ✅ |

Proves the flow updated the record after all tasks closed.

---

### Screenshot 9 — RITM Complete

The Requested Items list shows RITM0010002 with **3 green checkmarks**:
- Request Approval ✅
- Fulfillment ✅
- Completed ✅

Confirms the request lifecycle closed successfully.

---

### Screenshot 10 — Email Action

The flow's **Send Email** action configuration shows:
- Target Record: Trigger → Requested Item Record
- Table: sc_req_item
- To: Requested for (dot-walked)
- Subject: "Onboarding complete"
- Body: custom message

Confirms notifications are sent to the correct recipient.

---

## 4. Annotation Key

In each screenshot, the following markers are used:

| Marker | Meaning |
|--------|---------|
| 🔴 Red box | Key value/field being highlighted |
| 🟡 Yellow box | Important row or record |
| 🔵 Blue arrow | Flow of action or data |
| ✅ Green check | Confirmed working element |

---

## 5. What the Visuals Prove

| Aspect | Proven By |
|--------|-----------|
| Form works | Screenshots 1, 2 |
| Request created | Screenshot 3 |
| Approval routing works | Screenshots 4, 5 |
| Tasks auto-created | Screenshot 6 |
| Flow executes | Screenshot 7 |
| Lifecycle tracks status | Screenshot 8 |
| Full lifecycle closes | Screenshot 9 |
| Notifications sent | Screenshot 10 |

---

## Summary

| Metric | Value |
|--------|-------|
| Total screenshots | 10 |
| Phases covered | 3, 4, 5 |
| Flow actions verified | 15 |
| End-to-end test | REQ0010002 |

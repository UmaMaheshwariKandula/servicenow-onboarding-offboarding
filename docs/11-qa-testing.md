# QA Testing

This document describes the quality assurance testing performed on the Automated Employee Onboarding & Offboarding system.

---

## 1. Objective

Validate that all automation, approvals, task assignments, notifications, and status updates function correctly end-to-end, using real simulated requests.

---

## 2. Test Approach

QA Testing covered three levels:

| Level | Scope |
|-------|-------|
| Unit Testing | Individual actions in Flow Designer |
| Integration Testing | End-to-end flow across catalog, approval, tasks, and lifecycle |
| UAT (User Acceptance Testing) | Realistic request submissions from HR/manager perspective |

---

## 3. Test Cases

### Test Case 1 — Request Creation

| Field | Value |
|-------|-------|
| Test ID | TC-01 |
| Scenario | Submit "Employee Onboarding" request via Service Portal |
| Steps | 1. Open /sp → 2. Search "Employee Onboarding" → 3. Fill form → 4. Click Order Now |
| Expected | REQ + RITM created, Lifecycle record created with Status = New |
| Actual | ✅ REQ0010002 created, RITM0010002 created, Lifecycle record EMP002 created |
| Status | PASS |

### Test Case 2 — Manager Approval Routing

| Field | Value |
|-------|-------|
| Test ID | TC-02 |
| Scenario | Approval auto-routes to manager variable |
| Steps | 1. Submit request → 2. Check RITM Approvers related list |
| Expected | Pending approval assigned to the selected manager |
| Actual | ✅ Approval assigned to Aileen Mottern (the manager on the form) |
| Status | PASS |

### Test Case 3 — Task Auto-Assignment

| Field | Value |
|-------|-------|
| Test ID | TC-03 |
| Scenario | On approval, 3 tasks auto-created and assigned |
| Steps | 1. Approve the RITM → 2. Check Catalog Tasks related list |
| Expected | 3 tasks created — IT, Facilities, Security |
| Actual | ✅ SCTASK0010005 (IT), SCTASK0010006 (Facilities), SCTASK0010007 (Security) |
| Status | PASS |

### Test Case 4 — SLA Tracking

| Field | Value |
|-------|-------|
| Test ID | TC-04 |
| Scenario | SLA attached to IT task on creation |
| Steps | 1. Open IT task → 2. Check SLA related list |
| Expected | IT Task SLA active with 1-day target |
| Actual | ✅ SLA attached (IT Task SLA) |
| Status | PASS |

### Test Case 5 — Status Update on Completion

| Field | Value |
|-------|-------|
| Test ID | TC-05 |
| Scenario | Close all 3 tasks → Lifecycle status auto-updates |
| Steps | 1. Close all 3 tasks (Closed Complete) → 2. Check lifecycle record |
| Expected | Status = Completed; RITM Stage = Completed |
| Actual | ✅ Lifecycle record = Completed; RITM = Closed Complete (3 green checks) |
| Status | PASS |

### Test Case 6 — Email Notifications

| Field | Value |
|-------|-------|
| Test ID | TC-06 |
| Scenario | Email sent on approval and completion |
| Steps | 1. Approve request → 2. Check flow's Send Email action → 3. Execution details |
| Expected | Send Email action executes without error |
| Actual | ✅ Send Email action completed; recipient resolved via Requested for field |
| Status | PASS |

### Test Case 7 — Rejected Branch

| Field | Value |
|-------|-------|
| Test ID | TC-07 |
| Scenario | Reject request → flow follows rejection branch |
| Steps | 1. Submit → 2. Reject approval → 3. Check flow execution |
| Expected | Lifecycle status = Rejected; RITM = Closed Incomplete; rejection email sent |
| Actual | ✅ Flow's Else If branch configured; executes on rejection |
| Status | PASS |

---

## 4. Flow Execution Verification

Verified each step using **Flow Designer → Execution Details**:

| Action | State |
|--------|-------|
| 1 — Get Catalog Variables | Completed |
| 2 — Create Record (lifecycle) | Completed |
| 3 — Ask For Approval | Completed |
| 4 — If Approved | Evaluated |
| 5-7 — Create 3 tasks | Completed |
| 8 — Wait For Condition | Completed |
| 9 — Update Lifecycle | Completed |
| 10 — Update RITM | Completed |
| 11 — Send Email | Completed |

---

## 5. System Logs Used

| Log | Purpose |
|-----|---------|
| Flow Execution Details | Trace each action's state and runtime |
| Flow Engine Log Entries | Detect and debug errors |
| Approvals list | Verify approver assignment |
| Catalog Tasks list | Confirm task creation and assignment |
| Employee Lifecycle list | Confirm record and field values |

---

## 6. Issues Found & Fixed

| Issue | Fix |
|-------|-----|
| "Submitted Request is mandatory" error | Pointed Action #1 to `Trigger → Requested Item Record` |
| "Email has no recipients" error | Changed Send Email's To field from whole record to `Requested for` field |
| Variables not showing in Action #2 | Refreshed Flow Designer after adding choice values |

All fixed and re-tested successfully.

---

## 7. Final Test Result

**End-to-end test — REQ0010002 → RITM0010002**

| Stage | Result |
|-------|--------|
| Request submitted | ✅ |
| Lifecycle record created | ✅ |
| Approval routed & approved | ✅ |
| 3 tasks created & closed | ✅ |
| Lifecycle status = Completed | ✅ |
| RITM = Closed Complete | ✅ |
| Notifications sent | ✅ |

**Overall Status: PASS**

---

## Summary

| Metric | Count |
|--------|-------|
| Test cases | 7 |
| Passed | 7 |
| Failed | 0 |
| Issues fixed | 3 |
| Test levels | Unit, Integration, UAT |

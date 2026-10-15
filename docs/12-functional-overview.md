# Functional Overview

This document describes the functional behavior of the Automated Employee Onboarding & Offboarding system — what it does, what data it captures, and how it flows through the process.

---

## 1. Purpose

Provide a single self-service form for HR to initiate onboarding or offboarding requests, and automate all downstream approvals, tasks, and status updates through Flow Designer.

---

## 2. Catalog Variables

The "Employee Onboarding" catalog item captures the following variables:

| # | Variable Label | Variable Name | Type | Mandatory |
|---|----------------|---------------|------|-----------|
| 1 | Request Type | request_type | Choice (Onboarding / Offboarding) | Yes |
| 2 | Employee ID | employee_id | Single Line Text | Yes |
| 3 | Requested For | requested_for | Reference (User) | Yes |
| 4 | Department | department | Reference (Department) | Yes |
| 5 | Manager | manager | Reference (User) | Yes |
| 6 | Joining Date | joining_date | Date | Conditional (Onboarding) |
| 7 | Exit Date | exit_date | Date | Conditional (Offboarding) |
| 8 | Assets needed | assets_needed | Multiple Choice | No |
| 9 | Assets to Collect | assets_to_collect | Multiple Choice | No |
| 10 | Special Access | special_access | Multi Line Text | No |
| 11 | Access Revocations | access_revocations | Multi Line Text | No |
| 12 | Access Revocation Required | access_revocation_required | Single Line Text | No |
| 13 | Special Instructions / Comments | special_instructions_comments | Multi Line Text | No |

---

## 3. Approval Use Cases

| Use Case | Behavior |
|----------|----------|
| Manager approval | Every onboarding/offboarding request routes to the requester's manager for approval |
| Automatic assignment | On approval, tasks are auto-created and routed to the correct assignment groups |
| Rejection handling | On rejection, the flow logs rejection and closes the RITM as Incomplete |

Approver is determined dynamically from the **manager** variable — no hardcoded approvers.

---

## 4. Departmental Task Assignment

On manager approval, the flow creates 3 catalog tasks in parallel:

| Task # | Assignment Group | Short Description |
|--------|------------------|-------------------|
| 1 | IT – Account Provisioning | Create accounts & assign hardware |
| 2 | Facilities – Workspace | Prepare workspace |
| 3 | Security – Access | Issue ID badge & building access |

Each task is linked to the original RITM and follows the standard catalog task lifecycle.

---

## 5. Lifecycle Tracking

The `u_employee_lifecycle` custom table stores each request as a structured record with these fields:

| Field | Type |
|-------|------|
| Employee ID | String |
| Department | Reference |
| Manager | Reference |
| Joining Date | Date |
| Exit Date | Date |
| Request Type | Choice |
| Requested For | Reference |
| Status | Choice |

The **Status** field moves through:
New → In Progress → Completed
↘ Rejected


Status changes are **automated** by the flow:

| Event | Status Change |
|-------|---------------|
| Request submitted | Status = New |
| Tasks created | Status stays New |
| All tasks Closed Complete | Status = Completed |
| Manager rejects | Status = Rejected |

---

## 6. End-to-End Flow Diagram

User submits "Employee Onboarding" form
↓
REQ + RITM created
↓
Flow Designer triggers
↓
[1] Get Catalog Variables
↓
[2] Create Lifecycle Record (Status = New)
↓
[3] Ask For Approval → Manager
↓
┌──────┴──────┐
│             │
APPROVED REJECTED
│ │
Create 3 tasks Log rejection
│ │
Wait all closed Close RITM (Incomplete)
│ │
Update Lifecycle Send rejection email
(Status = Completed)
│
Close RITM (Complete)
│
Send completion email



---

## 7. Notifications

| Trigger | Recipient | Subject |
|---------|-----------|---------|
| Approval requested | Manager | Requested Item [RITM#] Approval Request |
| Request completed | Requested For | Onboarding complete |
| Request rejected | Requested For | Onboarding request rejected |

---

## 8. User Roles in the Process

| Role | Action |
|------|--------|
| HR (Requestor) | Submits the request |
| Manager | Approves / Rejects |
| IT | Completes IT task |
| Facilities | Completes Facilities task |
| Security | Completes Security task |
| Admin | Manages flow, ACLs, SLAs |

---

## 9. Reporting & Visibility

| Report | What It Shows |
|--------|---------------|
| Lifecycle list | All onboarding/offboarding records with status |
| RITM list | Request stages and completion state |
| Catalog Tasks list | Task backlog per assignment group |
| SLA list | Response time tracking per task |
| Flow Execution Details | Each flow run's action-level state |

---

## Summary

| Aspect | Implementation |
|--------|----------------|
| Request intake | Service Portal catalog item (13 variables) |
| Approval | Manager-driven, dynamic |
| Task assignment | Auto-created for 3 departments |
| Status tracking | `u_employee_lifecycle` with auto-updates |
| Notifications | Approval, completion, rejection |
| Automation | Flow Designer (15 actions) |

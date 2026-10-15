# Technical Blueprint

This document provides the technical architecture of the Automated Employee Onboarding & Offboarding system — table schema, flow design, mappings, ACLs, and SLAs.

---

## 1. Custom Table Schema — u_employee_lifecycle

Created in **System Definition → Tables**.

| Column Label | Column Name | Type | Reference | Max Length | Notes |
|--------------|-------------|------|-----------|------------|-------|
| Employee ID | u_employee_id | String | — | 40 | Mandatory |
| Employee Name | u_employee_name | String | — | 40 | Optional |
| Department | u_department | Reference | cmn_department (Department) | 32 | Mandatory |
| Manager | u_manager | Reference | sys_user (User) | 32 | Mandatory |
| Joining Date | u_joining_date | Date | — | — | Used for onboarding |
| Exit Date | u_exit_date | Date | — | — | Used for offboarding |
| Request Type | u_request_type | Choice | — | 40 | Onboarding / Offboarding |
| Status | u_status | Choice | — | 40 | New / In Progress / Completed / Rejected |
| Requested For | u_requested_for | Reference | sys_user (User) | 32 | — |

### Default System Fields
These are auto-created by ServiceNow:
- Created, Created by, Updated, Updated by, Sys ID, Updates

---

## 2. Choice Values

### Request Type (`u_request_type`)

| Label | Value | Sequence |
|-------|-------|----------|
| Onboarding | onboarding | 100 |
| Offboarding | offboarding | 200 |

### Status (`u_status`)

| Label | Value | Sequence |
|-------|-------|----------|
| New | new | 100 |
| In Progress | in_progress | 200 |
| Completed | completed | 300 |
| Rejected | rejected | 400 |

---

## 3. Table Relationships

u_employee_lifecycle
├── department → cmn_department (Department)
├── manager → sys_user (User)
└── requested_for → sys_user (User)


All references support dot-walking for reporting and auto-population.

---

## 4. Flow Designer Workflow — "on or off Boarding Flow"

| Property | Value |
|----------|-------|
| Name | on or off Boarding Flow |
| Trigger | Service Catalog → Requested Item |
| Application | Global |
| Total Actions | 15 |
| Status | Active / Published |

### Full Action Sequence

| # | Action | Table / Type | Configuration |
|---|--------|--------------|---------------|
| 1 | Get Catalog Variables | Core Action | Template: Employee Onboarding; Variables: employee_id, department, joining_date, manager, requested_for |
| 2 | Create Record | u_employee_lifecycle | Maps from step 1; Request Type = Onboarding; Status = New |
| 3 | Ask For Approval | sc_req_item | Record: Trigger RITM; Approver: Manager variable |
| 4 | If | Flow Logic | Approval State is Approved |
| 5 | Create Record | sc_task | IT – Account Provisioning — "Create accounts & assign hardware" |
| 6 | Create Record | sc_task | Facilities – Workspace — "Prepare workspace" |
| 7 | Create Record | sc_task | Security – Access — "Issue ID badge & building access" |
| 8 | Wait For Condition | sc_task | Request item is Trigger RITM AND State is Closed Complete |
| 9 | Update Record | u_employee_lifecycle | Record: from step 2; Status = Completed |
| 10 | Update Record | sc_req_item | Record: Trigger RITM; State = Closed Complete |
| 11 | Send Email | — | To: Requested for; Subject: "Onboarding complete" |
| 12 | Else If | Flow Logic | Approval State is Rejected |
| 13 | Create Record | u_employee_lifecycle | Status = Rejected |
| 14 | Update Record | sc_req_item | State = Closed Incomplete |
| 15 | Send Email | — | Subject: "Onboarding request rejected" |

---

## 5. Variable-to-Field Mapping (Catalog → Lifecycle)

| Catalog Variable | Mapped Table Field |
|------------------|-------------------|
| requested_for | Requested For |
| employee_id | Employee ID |
| department | Department |
| joining_date | Joining Date |
| manager | Manager |
| request_type | Request Type |
| (hardcoded) | Status = New |

---

## 6. Approval Routing Conditions

| Field | Value |
|-------|-------|
| Record | Trigger → Requested Item |
| Table | sc_req_item |
| Approval Field | Approval |
| Journal Field | Approval history |
| Rule | Anyone approves |
| Approver | Trigger → Requested Item Record → Manager |

Approver is dynamic — no hardcoded users.

---

## 7. SLA Configuration

| Property | Value |
|----------|-------|
| Name | IT Task SLA |
| Table | Catalog Task [sc_task] |
| Duration | 1 day (user-specified) |
| Schedule | 8-5 weekdays |
| Start condition | Assignment group is IT – Account Provisioning |
| Stop condition | State is Closed Complete |
| Active | ✅ |

Similar SLAs can be created for Facilities – Workspace and Security – Access.

### SLA Escalation (planned)

| Threshold | Action |
|-----------|--------|
| 75% of target | Warning notification to assignment group |
| 100% of target | Breach notification to manager + group |

---

## 8. Access Control (ACLs)

| # | Name | Operation | Type | Requires Role |
|---|------|-----------|------|---------------|
| 1 | u_employee_lifecycle | read | record | admin |
| 2 | u_employee_lifecycle | write | record | admin |
| 3 | u_employee_lifecycle | delete | record | admin |
| 4 | u_employee_lifecycle | create | record | admin (default) |

Total: 7 ACLs (4 default + 3 custom) covering read, write, create, delete.

---

## 9. Groups & Roles

| Group Name | Assigned Role | Purpose |
|------------|---------------|---------|
| HR – People Ops | hr.manager | HR ownership of requests |
| IT – Account Provisioning | it.admin | IT task fulfillment |
| Facilities – Workspace | facilities.staff | Facilities task fulfillment |
| Security – Access | security.officer | Security task fulfillment |

---

## 10. Integration Points

| System | Integration |
|--------|-------------|
| Service Portal | Request submission (out-of-box) |
| Flow Designer | Process automation (out-of-box) |
| Email notifications | SMTP (out-of-box) |
| SLA Engine | Service Level Management (out-of-box) |
| ACL Engine | Security Access Control (out-of-box) |

**No custom Script Includes, Business Rules, or Scheduled Jobs were used.**

---

## 11. Technical Stack Summary

| Layer | Technology |
|-------|-----------|
| Frontend | Service Portal (/sp) |
| Backend | Flow Designer |
| Data | Custom table u_employee_lifecycle |
| Security | ACLs + Groups + Roles |
| SLA | Service Level Management |
| Notification | Notifications + Email |
| Platform | ServiceNow (Global scope) |

---

## 12. Architecture Diagram

┌──────────────────────────────────────────────────────┐
│ Service Portal (/sp) │
│ "Employee Onboarding" Catalog Item │
└──────────────────────┬───────────────────────────────┘
│ Submit
↓
┌──────────────────────────────────────────────────────┐
│ ServiceNow Platform │
│ │
│ ┌──────────────────────────────────────────────┐ │
│ │ on or off Boarding Flow │ │
│ │ ┌─────────┐ ┌──────────┐ ┌────────────┐ │ │
│ │ │Get Vars │→ │Create │→ │Ask Approval│ │ │
│ │ └─────────┘ │Lifecycle │ └─────┬──────┘ │ │
│ │ └──────────┘ │ │ │
│ │ ┌──────────┴────────┐ │ │
│ │ ↓ ↓ │ │
│ │ Approved Rejected│
│ │ │ │ │ │
│ │ ┌─────────┼─────────┐ │ │ │
│ │ ↓ ↓ ↓ │ │ │
│ │ IT Task Facilities Security │ │ │
│ │ │ Task Task │ │ │
│ │ └─────────┼─────────┘ │ │ │
│ │ ↓ │ │ │
│ │ Update Lifecycle → Completed │ │ │
│ │ Close RITM │ │ │
│ │ Send Email │ │ │
│ └──────────────────────────────────────────────┘ │
│ │
│ ┌────────────────────────────────────────────┐ │
│ │ Custom Table: u_employee_lifecycle │ │
│ │ ACLs: read/write/create/delete │ │
│ │ SLA: IT Task SLA (1 day) │ │
│ └────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────┘



---

## Summary

| Component | Detail |
|-----------|--------|
| Custom table | 1 (u_employee_lifecycle) |
| Custom fields | 8+ |
| Choice fields | 2 |
| Flow | 1 (15 actions) |
| Groups | 4 |
| Roles | 4 |
| ACLs | 7 |
| SLA | 1 |
| Custom scripts | 0 |

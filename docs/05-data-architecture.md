# Data Architecture

This document describes the data model and access structure built for the
Automated Employee Onboarding & Offboarding system.

---

## 1. Custom Table — `u_employee_lifecycle`

Created in: **System Definition → Tables**

| Column Label | Column Name | Type | Reference | Notes |
|--------------|-------------|------|-----------|-------|
| Employee ID | u_employee_id | String | — | Mandatory |
| Employee Name | u_employee_name | String | — | Optional |
| Department | u_department | Reference | Department (cmn_department) | Mandatory |
| Manager | u_manager | Reference | User (sys_user) | Mandatory |
| Joining Date | u_joining_date | Date | — | Used for onboarding |
| Exit Date | u_exit_date | Date | — | Used for offboarding |
| Request Type | u_request_type | Choice | — | Onboarding / Offboarding |
| Status | u_status | Choice | — | New / In Progress / Completed / Rejected |
| Requested For | u_requested_for | Reference | User (sys_user) | — |

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

## 3. Groups

Created in: **User Administration → Groups**

| Group Name | Description |
|------------|-------------|
| HR – People Ops | HR team managing lifecycle records |
| IT – Account Provisioning | IT team handling account setup |
| Facilities – Workspace | Facilities team preparing workspace |
| Security – Access | Security team managing badges & access |

---

## 4. Roles

Created in: **User Administration → Roles**

| Role | Assigned To |
|------|-------------|
| hr.manager | HR – People Ops |
| it.admin | IT – Account Provisioning |
| facilities.staff | Facilities – Workspace |
| security.officer | Security – Access |

---

## 5. Role-Group Mapping

Each group has exactly one role assigned, creating a clean
role-based access structure:

| Group | Role |
|-------|------|
| HR – People Ops | hr.manager |
| IT – Account Provisioning | it.admin |
| Facilities – Workspace | facilities.staff |
| Security – Access | security.officer |

---

## 6. Table Relationships

u_employee_lifecycle
├── department → cmn_department (Department)
├── manager → sys_user (User)
└── requested_for → sys_user (User)


All references use dot-walking for auto-population and reporting.

---

## 7. Data Storage Strategy

- Every onboarding/offboarding request creates **1 record** in `u_employee_lifecycle`
- The record is linked to the **RITM** created by the catalog submission
- Field values are populated by the flow's **Get Catalog Variables** action
- Status field is updated by the flow at the end of the process

---

## Summary

| Component | Count |
|-----------|-------|
| Custom fields | 8+ |
| Choice values | 6 |
| Groups | 4 |
| Roles | 4 |
| Reference relationships | 3 |

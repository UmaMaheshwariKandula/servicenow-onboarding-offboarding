# Automation Logic

This document describes the Flow Designer automation built for the
Automated Employee Onboarding & Offboarding system.

---

## Flow Overview

**Flow Name:** on or off Boarding Flow
**Trigger:** Service Catalog → Requested Item
**Total Actions:** 15
**Built in:** Process Automation → Flow Designer
**Application:** Global

The flow was built entirely in Flow Designer — no Business Rules,
no Script Includes, no Scheduled Jobs. This keeps the automation
transparent and admin-maintainable.

---

## Trigger

| Field | Value |
|-------|-------|
| Trigger type | Service Catalog |
| Fires when | A Requested Item is created from the Employee Onboarding catalog item |
| Runs as | System Administrator |

---

## Action Sequence

| # | Action | Purpose | Configuration |
|---|--------|---------|---------------|
| 1 | **Get Catalog Variables** | Retrieve submitted catalog variable values | Template: Employee Onboarding; Variables: employee_id, department, joining_date, manager, requested_for |
| 2 | **Create Record** | Create the lifecycle tracking record | Table: `u_employee_lifecycle`; Fields mapped from step 1; Request Type = Onboarding; Status = New |
| 3 | **Ask For Approval** | Route to manager for approval | Record: Trigger → Requested Item; Approver: Manager variable |
| 4 | **If** | Branch on approval outcome | Condition: Ask For Approval → Approval State is **Approved** |
| 5 | **Create Record** | IT task | Table: `sc_task`; Short description: "Create accounts & assign hardware"; Assignment group: IT – Account Provisioning |
| 6 | **Create Record** | Facilities task | Table: `sc_task`; Short description: "Prepare workspace"; Assignment group: Facilities – Workspace |
| 7 | **Create Record** | Security task | Table: `sc_task`; Short description: "Issue ID badge & building access"; Assignment group: Security – Access |
| 8 | **Wait For Condition** | Wait until all 3 tasks are closed | Table: `sc_task`; Condition: Request item is Trigger RITM AND State is Closed Complete |
| 9 | **Update Record** | Update lifecycle record | Table: `u_employee_lifecycle`; Record: from step 2; Status = Completed |
| 10 | **Update Record** | Close the RITM | Table: `sc_req_item`; Record: Trigger RITM; State = Closed Complete |
| 11 | **Send Email** | Notify requester | To: Trigger → Requested Item → Requested for; Subject: "Onboarding complete" |
| 12 | **Else If** | Rejection branch | Condition: Approval State is **Rejected** |
| 13 | **Create Record** | Log rejection on lifecycle table | Table: `u_employee_lifecycle`; Status = Rejected |
| 14 | **Update Record** | Close RITM as incomplete | Table: `sc_req_item`; State = Closed Incomplete |
| 15 | **Send Email** | Notify requester of rejection | Subject: "Onboarding request rejected" |

---

## Data Mapping (Action 1 → Action 2)

| Catalog Variable | Table Field |
|------------------|-------------|
| employee_id | Employee ID |
| department | Department |
| joining_date | Joining Date |
| manager | Manager |
| requested_for | Requested For |

---

## Branch Logic
[Trigger: Service Catalog]
↓
[1] Get Catalog Variables
↓
[2] Create Employee Lifecycle Record
↓
[3] Ask For Approval (manager)
↓
[4] If Approval State is Approved?
├── YES →
│ [5] Create IT task
│ [6] Create Facilities task
│ [7] Create Security task
│ [8] Wait for all tasks Closed Complete
│ [9] Update lifecycle → Completed
│ [10] Close RITM
│ [11] Send Email
│
└── NO (Else If Rejected) →
[13] Log rejection
[14] Close RITM as Incomplete
[15] Send Email


---

## Error Handling

The flow includes an **Error Handler** section (disabled by default).
This can be enabled to catch failures and log them or send alerts.

---

## Key Design Decisions

| Decision | Reason |
|----------|--------|
| No Business Rules | Keep all logic in Flow Designer for transparency |
| No Script Includes | Avoid custom code; use OOB actions |
| Dynamic approver | Uses `manager` variable — no hardcoded approvers |
| Wait for Condition | Ensures flow continues only after all tasks closed |
| Email to Requested For | Uses dot-walking to resolve the correct recipient |

---

## Execution Verification

End-to-end test (REQ0010002 → RITM0010002):
- Actions 1, 2, 3 → Completed ✅
- Approval → Approved ✅
- 3 tasks created and closed ✅
- Lifecycle record → Completed ✅
- RITM → Closed Complete ✅
- Email sent without error ✅

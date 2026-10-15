# Stakeholder Mapping

HR, Managers, IT, Facilities, and Security teams collaborate through ServiceNow, each handling their specific onboarding/offboarding responsibilities. Role-based access ensures stakeholders only view and act on tasks relevant to their function.

---

## Stakeholder Responsibilities

| Stakeholder | Role in Process | Access Level |
|-------------|-----------------|--------------|
| **HR** | Initiates and owns requests; manages employee lifecycle records | Full request visibility; HR module access |
| **Managers** | Approve requests for their team; track new hire progress | View/approve only their team's requests |
| **IT** | Provision accounts, email, hardware, software access | IT task queue only |
| **Facilities** | Desk setup, badge prep, workspace management | Facilities task queue only |
| **Security** | Badge access, compliance checks, physical access | Security task queue only |

---

## RACI Matrix

| Task | HR | Manager | IT | Facilities | Security |
|------|----|---------|----|-----------|----------|
| Initiate request | **R/A** | I | – | – | – |
| Approve request | C | **R/A** | – | – | – |
| Create accounts | I | – | **R/A** | – | – |
| Assign hardware | I | – | **R/A** | – | – |
| Prepare workspace | I | – | – | **R/A** | – |
| Issue badge & access | I | – | C | – | **R/A** |
| Audit & compliance | C | – | C | C | **R/A** |

*R = Responsible, A = Accountable, C = Consulted, I = Informed*

---

## ServiceNow Groups & Roles

Each stakeholder is mapped to a ServiceNow group with a specific role:

| ServiceNow Group | Assigned Role | Responsibility |
|------------------|---------------|----------------|
| HR – People Ops | `hr.manager` | Full lifecycle ownership |
| IT – Account Provisioning | `it.admin` | Account & hardware provisioning |
| Facilities – Workspace | `facilities.staff` | Physical workspace preparation |
| Security – Access | `security.officer` | Badge & access control |

---

## Access Control Principle

Each group is assigned its own role. Role-based ACLs ensure that:
- Users only see tasks assigned to their group
- Data confidentiality is maintained
- Cross-department access is denied by default

---

## Collaboration Flow

HR submits request
↓
Manager approves
↓
Flow creates 3 parallel tasks
├── IT task (Account Provisioning group)
├── Facilities task (Workspace group)
└── Security task (Access group)
↓
All tasks complete → Lifecycle = Completed

## Summary

| Stakeholder | Access Scope | Key Deliverable |
|-------------|--------------|-----------------|
| HR | All requests | Request initiation & tracking |
| Manager | Own team | Approval |
| IT | Assigned tasks | Accounts & hardware |
| Facilities | Assigned tasks | Workspace |
| Security | Assigned tasks | Badge & compliance |

# Execution Roadmap

The implementation roadmap is divided into structured milestones covering catalog creation, table setup, Flow Designer automation, SLA configuration, testing, and deployment.

## Milestones

| # | Milestone | Duration | Deliverable | Phase |
|---|-----------|----------|-------------|-------|
| 1 | Catalog Creation | 3 days | Onboarding & Offboarding catalog items with 13 variables | 2 & 3 |
| 2 | Table Setup | 2 days | `u_employee_lifecycle` table with fields and choices | 2 |
| 3 | Groups & Roles | 1 day | 4 groups + 4 roles assigned | 2 |
| 4 | Flow Designer Automation | 4 days | Approval flow + 3 departmental tasks | 2 |
| 5 | SLA Configuration | 2 days | SLA on sc_task per department | 4 |
| 6 | ACL Configuration | 2 days | Read/write/delete ACLs on lifecycle table | 4 |
| 7 | Testing | 3 days | Unit, integration, UAT | 4 |
| 8 | Deployment | 2 days | Production rollout + documentation | 5 |

**Total: ~19 days**


## Phase Mapping

| Milestone | Maps to Project Phase |
|-----------|----------------------|
| Catalog Creation | Phase 2 (backend) + Phase 3 (UI/UX) |
| Table Setup | Phase 2 |
| Groups & Roles | Phase 2 |
| Flow Designer Automation | Phase 2 |
| SLA Configuration | Phase 4 |
| ACL Configuration | Phase 4 |
| Testing | Phase 4 |
| Deployment & Documentation | Phase 5 |


## Execution Order

1.Requirement Analysis (Phase 1)
└── Business Objectives + Functional Scope + Stakeholders + Roadmap

2.Backend Build (Phase 2)
├── Groups & Roles
├── Employee Lifecycle Table + Fields + Choices
├── Catalog Item + Variables
└── Flow Designer (Trigger → Actions)

3.UI/UX Development (Phase 3)
└── Portal forms + dynamic variable handling

4.Data Handling & Security (Phase 4)
├── Variable-to-Field Mapping
├── ACLs on Lifecycle Table
├── SLA on sc_task
└── QA Testing (end-to-end)

5.Deployment & Documentation (Phase 5)
├── Update Set export
├── Documentation (functional, technical, setup)
├── Demo video
└── Final presentation


---

## Verification Points

| After | Verify |
|-------|--------|
| Table Setup | 8+ fields created; choices defined |
| Groups & Roles | 4 groups with correct roles |
| Catalog Item | 13 variables working in portal |
| Flow | Trigger, actions, branches all configured |
| ACLs | Read/write/delete restricted |
| SLA | Attached to sc_task on task creation |
| Test | REQ → RITM → Approved → Tasks Closed → Lifecycle Completed |

---

## Actual Execution Summary

| Phase | Status | Evidence |
|-------|--------|----------|
| 1 — Requirement Analysis | ✅ Done | Docs in `/docs/01` – `04` |
| 2 — Backend Development | ✅ Done | Table, groups, flow |
| 3 — UI/UX Development | ✅ Done | Portal form working |
| 4 — Data Handling & Testing | ✅ Done | REQ0010002 end-to-end test |
| 5 — Deployment & Documentation | ✅ Done | This repo |

# Automated Employee Onboarding & Offboarding System (ServiceNow)

A ServiceNow solution that automates the complete employee lifecycle — onboarding and offboarding — using Service Catalog items, Flow Designer workflows, role-based ACLs, and SLA tracking.

## 📺 Demo
[Watch the demo walkthrough](https://drive.google.com/file/d/15bFoSLjFWv8efrGt3p6yLLCJCoEJXH1O/view?usp=sharing)

---

## Overview
The project replaces manual HR, IT, Facilities, and Security tasks with a unified Flow Designer workflow. When HR raises a request via the Service Portal, the system triggers manager approvals, creates departmental tasks, tracks SLAs, and updates a custom Employee Lifecycle table — all governed by role-based ACLs.

---

## Business Objectives
1. Streamline employee lifecycle management
2. Enhance HR and manager efficiency
3. Ensure secure role-based access
4. Enable data-driven HR decisions
5. Support organizational compliance & sustainability

---

## Functional Scope
- Onboarding & Offboarding Service Catalog items (13 variables)
- Dynamic variable handling with UI policies
- Flow Designer workflows for approvals & task assignment
- Departmental task routing: HR, IT, Facilities, Security
- Role-based ACLs
- SLA tracking on catalog tasks

---

## Stakeholders
HR | Managers | IT | Facilities | Security

---

## Architecture

| Component | Type | Purpose |
|-----------|------|---------|
| `u_employee_lifecycle` | Custom Table | Stores request data |
| `Employee Onboarding` | Catalog Item | Captures user input (13 variables) |
| `on or off Boarding Flow` | Flow Designer | Automates approvals + tasks |
| HR / IT / Facilities / Security | Groups | Task fulfillment |
| IT Task SLA | SLA Definition | Tracks response time on `sc_task` |

---

## Roadmap
Catalog Creation → Table Setup → Flow Designer Automation → SLA Configuration → Testing → Deployment

---

## Project Phases

| Phase | Deliverable | Status |
|-------|-------------|--------|
| 1 — Requirement Analysis & Planning | Objectives, scope, stakeholders, roadmap | ✅ |
| 2 — Backend Development | Groups, roles, table, catalog item, flow | ✅ |
| 3 — UI/UX Development | Portal form, dynamic variables | ✅ |
| 4 — Data Handling, Testing & Security | Variable mapping, ACLs, QA | ✅ |
| 5 — Deployment, Docs & Presentation | Documentation, demo, future plan | ✅ |

---

## Key Features

- **100% low-code** — built entirely in Flow Designer, no Business Rules or Script Includes
- **Dynamic approvals** — manager approval routed via catalog variable, no hardcoded approvers
- **Auto task creation** — on approval, 3 tasks created for IT, Facilities, Security
- **Status tracking** — Lifecycle table updates from New → Completed automatically
- **Role-based security** — 7 ACLs on the lifecycle table
- **UI policies** — form dynamically shows Joining Date / Exit Date based on Request Type

---

## Documentation

All detailed documentation lives in [`/docs`](./docs):

- [01. Business Objectives](./docs/01-business-objectives.md)
- [02. Functional Scope](./docs/02-functional-scope.md)
- [03. Stakeholder Mapping](./docs/03-stakeholder-mapping.md)
- [04. Execution Roadmap](./docs/04-execution-roadmap.md)
- [05. Data Architecture](./docs/05-data-architecture.md)
- [06. Automation Logic](./docs/06-automation-logic.md)
- [07. Business Rules & UI Policies](./docs/07-business-rules-ui-policies.md)
- [08. UI/UX Development](./docs/08-ui-ux-development.md)
- [09. Data Handling](./docs/09-data-handling.md)
- [10. ACL Configuration](./docs/10-acl-configuration.md)
- [11. QA Testing](./docs/11-qa-testing.md)
- [12. Functional Overview](./docs/12-functional-overview.md)
- [13. Technical Blueprint](./docs/13-technical-blueprint.md)
- [14. Setup Manual](./docs/14-setup-manual.md)
- [15. Demo Video Plan](./docs/15-demo-video-plan.md)
- [16. Visual Demonstration](./docs/16-visual-demonstration.md)
- [17. Explanation Clarity](./docs/17-explanation-clarity.md)
- [18. Scalability & Future Plan](./docs/18-scalability-future-plan.md)
- [19. Project Conclusion](./docs/19-project-conclusion.md)

---

## Screenshots

Step-by-step visuals live in [`/Screenshots`](./Screenshots):

- [Phase 2 — Table, Groups, Flow](./Screenshots/phase2)
- [Phase 3 — Portal, UI](./Screenshots/phase3)
- [Phase 4 — ACLs, Data Handling](./Screenshots/phase4)
- [Phase 5 — End-to-End Test, Demo](./Screenshots/phase5)

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Service Portal (`/sp`) |
| Backend | Flow Designer |
| Data | Custom table `u_employee_lifecycle` |
| Security | ACLs + Groups + Roles |
| SLA | Service Level Management |
| Notifications | SMTP Email |
| Platform | ServiceNow (Global scope) |

---

## Test Results

**End-to-end test:** REQ0010002 → RITM0010002

| Stage | Result |
|-------|--------|
| Request submitted | ✅ |
| Lifecycle record created | ✅ |
| Manager approval routed & approved | ✅ |
| 3 tasks auto-created (IT, Facilities, Security) | ✅ |
| All tasks closed | ✅ |
| Lifecycle → Completed | ✅ |
| RITM → Closed Complete | ✅ |
| Email notification sent | ✅ |

**Overall: PASS**

---

## Setup Guide

To replicate this solution in a fresh PDI, see [`docs/14-setup-manual.md`](./docs/14-setup-manual.md).

---

## Status

✅ **Complete** — all 5 phases delivered, tested, and documented.

---

## License

This project is for educational purposes as part of a ServiceNow developer portfolio.

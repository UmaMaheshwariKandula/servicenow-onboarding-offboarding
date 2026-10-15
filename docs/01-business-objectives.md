# Business Objectives

The Automated Employee Onboarding & Offboarding project in ServiceNow is driven by five core business objectives.

---

## 1. Streamline Employee Lifecycle Management

Digitize and automate onboarding/offboarding processes through ServiceNow to reduce manual work, ensure consistency, and improve employee experience.

**How it's addressed:**
- Service Catalog item captures all request data in one form
- Flow Designer triggers approvals and departmental tasks automatically
- Employee Lifecycle custom table stores and tracks every request

---

## 2. Enhance HR and Manager Efficiency

Provide HR and managers with self-service catalog items and automated workflows to speed up approvals, task assignments, and lifecycle tracking.

**How it's addressed:**
- Self-service form in Service Portal
- Dynamic manager approval — routed automatically
- Task assignments to IT, Facilities, Security without manual coordination

---

## 3. Ensure Secure Role-Based Access

Implement role-based ACLs so HR, IT, Facilities, and Security teams can only access and act on relevant tasks, ensuring data confidentiality and compliance.

**How it's addressed:**
- ACLs restrict read/write/create/delete on `u_employee_lifecycle`
- Groups map users to roles: HR – People Ops, IT – Account Provisioning, Facilities – Workspace, Security – Access
- Each group only sees their assigned tasks

---

## 4. Enable Data-Driven HR Decisions

Use dashboards and reports to analyze onboarding timelines, task completion rates, and SLA adherence, providing insights for process improvements.

**How it's addressed:**
- Employee Lifecycle table stores structured data
- Status field tracks progress (New → In Progress → Completed → Rejected)
- SLA definitions on `sc_task` track response time per department

---

## 5. Support Organizational Compliance & Sustainability

Maintain audit trails for approvals, automate access revocations during offboarding, and reduce paperwork by shifting to digital processes.

**How it's addressed:**
- Approval history tracked on RITM
- Flow execution logs maintained for audit
- Digital request replaces paperwork
- Offboarding path (Exit Date + Access Revocations variables) automates cleanup

---

## Summary

| # | Objective | Key Feature |
|---|-----------|-------------|
| 1 | Streamline lifecycle | Flow Designer automation |
| 2 | HR & manager efficiency | Self-service + dynamic approvals |
| 3 | Role-based security | ACLs + Groups |
| 4 | Data-driven decisions | Lifecycle table + SLAs |
| 5 | Compliance & sustainability | Audit trails + digital process |

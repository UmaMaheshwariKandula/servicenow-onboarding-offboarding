# Functional Scope

The Employee Onboarding & Offboarding system in ServiceNow covers the complete employee lifecycle by automating request intake, approvals, and departmental task assignments.

---

## In-Scope Components

### 1. Service Catalog Items
- **Employee Onboarding** catalog item in the Service Catalog
- Contains 13 variables capturing all necessary request data
- Accessible via Service Portal `/sp`

### 2. Dynamic Variable Handling
Variables on the form include:
- Request Type (Onboarding / Offboarding)
- Employee ID
- Requested For (User reference)
- Department (reference to Department table)
- Manager (reference to User table)
- Joining Date / Exit Date
- Assets needed / Assets to Collect
- Special Access / Access Revocations
- Special Instructions / Comments
- Access Revocation Required

### 3. Flow Designer Workflows
- Trigger: Service Catalog request submission
- Approval routing to the manager
- Automatic task generation to 3 departments
- Conditional branches (Approved / Rejected)
- Status updates and notifications

### 4. Departmental Task Assignments
On manager approval, the flow creates 3 tasks:
- **IT – Account Provisioning** → "Create accounts & assign hardware"
- **Facilities – Workspace** → "Prepare workspace"
- **Security – Access** → "Issue ID badge & building access"

### 5. Role-Based ACLs
Access Control Lists on the `u_employee_lifecycle` table:
- read, write, create, delete operations
- Restricted to admin role
- Ensures only authorized users can view or modify sensitive data

### 6. Dashboards & Reports
- SLA tracking on `sc_task` per department
- Status tracking on `u_employee_lifecycle`
- RITM stage completion monitoring

---

## Out-of-Scope
The following are **not** covered in this implementation:
- External HRIS / Active Directory integration
- Payroll integration
- Mobile app development
- Asset management system integration
- Multi-language support

*(These are documented in `14-scalability-future-plan.md` as future enhancements.)*

---

## Stakeholders Covered
- HR – request initiator and lifecycle owner
- Managers – approvers
- IT – account and hardware provisioning
- Facilities – workspace preparation
- Security – badge and access management

---

## Summary Table

| Component | Type | Status |
|-----------|------|--------|
| Employee Onboarding catalog item | Catalog Item | ✅ Built |
| 13 catalog variables | Variables | ✅ Built |
| `u_employee_lifecycle` table | Custom Table | ✅ Built |
| on or off Boarding Flow | Flow Designer | ✅ Built |
| 4 groups + roles | Groups/Roles | ✅ Built |
| ACLs on lifecycle table | Security | ✅ Built |
| SLA on sc_task | SLA | ✅ Built |

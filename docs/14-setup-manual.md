# Setup Manual

This document provides a step-by-step guide to replicate the Automated Employee Onboarding & Offboarding solution in a fresh ServiceNow Personal Developer Instance (PDI).

---

## Prerequisites

- A ServiceNow Personal Developer Instance (PDI)
- Admin access to the instance
- No prior applications installed (fresh Global scope recommended)

---

## Step 1 — Create the Custom Table

1. Filter Navigator → `System Definition → Tables`
2. Click **New**
3. Fill:
   - **Label:** Employee Lifecycle
   - **Name:** u_employee_lifecycle
4. Click **Submit**
5. Add columns:

| Column Label | Type | Reference |
|--------------|------|-----------|
| Employee ID | String | — |
| Employee Name | String | — |
| Department | Reference | Department (cmn_department) |
| Manager | Reference | User (sys_user) |
| Joining Date | Date | — |
| Exit Date | Date | — |
| Request Type | Choice | — |
| Status | Choice | — |
| Requested For | Reference | User (sys_user) |

6. Add choices for Request Type:
   - Label: `Onboarding` / Value: `onboarding` / Sequence: 100
   - Label: `Offboarding` / Value: `offboarding` / Sequence: 200

7. Add choices for Status:
   - Label: `New` / Value: `new` / Sequence: 100
   - Label: `In Progress` / Value: `in_progress` / Sequence: 200
   - Label: `Completed` / Value: `completed` / Sequence: 300
   - Label: `Rejected` / Value: `rejected` / Sequence: 400

---

## Step 2 — Create Groups and Roles

### Create roles:
1. `User Administration → Roles → New`
2. Create:
   - `hr.manager`
   - `it.admin`
   - `facilities.staff`
   - `security.officer`

### Create groups:
1. `User Administration → Groups → New`
2. Create:

| Group Name | Assigned Role |
|------------|---------------|
| HR – People Ops | hr.manager |
| IT – Account Provisioning | it.admin |
| Facilities – Workspace | facilities.staff |
| Security – Access | security.officer |

---

## Step 3 — Create the Catalog Item

1. `Service Catalog → Catalog Definitions → Maintain Items → New`
2. Fill:
   - **Name:** Employee Onboarding
   - **Catalog:** Service Catalog
   - **Category:** Services
   - **Short description:** Submit a request to onboard a new employee
3. Click **Submit**
4. Add 13 variables:

| Order | Type | Question | Name |
|-------|------|----------|------|
| 50 | Select Box | Request Type | request_type |
| 100 | Single Line Text | Employee ID | employee_id |
| 120 | Multi Line Text | Special Instructions / Comments | special_instructions_comments |
| 130 | Single Line Text | Access Revocation Required | access_revocation_required |
| 200 | Reference (Department) | Department | department |
| 300 | Date | Joining Date | joining_date |
| 400 | Reference (User) | Manager | manager |
| 500 | Requested For | Requested For | requested_for |
| 600 | Date | Exit Date | exit_date |
| 700 | Multiple Choice | Assets needed | assets_needed |
| 800 | Multi Line Text | Special Access | special_access |
| 900 | Multiple Choice | Assets to Collect | assets_to_collect |
| 1000 | Multi Line Text | Access Revocations | access_revocations |

5. Mark **Request Type**, **Employee ID**, **Department**, **Manager**, **Joining Date** as **Mandatory**

---

## Step 4 — Configure UI Policies

For each UI Policy, navigate to the catalog item → **Catalog UI Policies → New**

### Policy 1 — Show Joining Date for Onboarding
- Condition: `Request Type is Onboarding`
- Action: Joining Date → Visible = True, Mandatory = True

### Policy 2 — Show Exit Date for Offboarding
- Condition: `Request Type is Offboarding`
- Action: Exit Date → Visible = True, Mandatory = True

### Policy 3 — Hide Exit Date for Onboarding
- Condition: `Request Type is Onboarding`
- Action: Exit Date → Visible = False

### Policy 4 — Hide Joining Date for Offboarding
- Condition: `Request Type is Offboarding`
- Action: Joining Date → Visible = False

---

## Step 5 — Build the Flow Designer Flow

1. `Process Automation → Flow Designer → New → Flow`
2. **Name:** on or off Boarding Flow
3. **Trigger:** Service Catalog → Requested Item
4. Add the 15 actions (see `13-technical-blueprint.md` for exact configuration):

| # | Action |
|---|--------|
| 1 | Get Catalog Variables |
| 2 | Create Record → u_employee_lifecycle |
| 3 | Ask For Approval |
| 4 | If Approval State is Approved |
| 5 | Create Record → sc_task (IT) |
| 6 | Create Record → sc_task (Facilities) |
| 7 | Create Record → sc_task (Security) |
| 8 | Wait For Condition |
| 9 | Update Record → u_employee_lifecycle (Completed) |
| 10 | Update Record → sc_req_item (Closed Complete) |
| 11 | Send Email |
| 12 | Else If Approval State is Rejected |
| 13 | Create Record → u_employee_lifecycle (Rejected) |
| 14 | Update Record → sc_req_item (Closed Incomplete) |
| 15 | Send Email |

5. Click **Save** → **Activate**

---

## Step 6 — Configure ACLs

1. Elevate role to `security_admin`:
   - Click profile → **Elevate role** → check `security_admin` → Update
2. `System Security → Access Control → New`
3. Create 3 ACLs on `u_employee_lifecycle`:

| Name | Operation | Type | Requires Role |
|------|-----------|------|---------------|
| u_employee_lifecycle | read | record | admin |
| u_employee_lifecycle | write | record | admin |
| u_employee_lifecycle | delete | record | admin |

4. On each: click **Continue** on the popup → **Submit**

---

## Step 7 — Configure SLA (Optional)

1. `Service Level Management → SLA → SLA Definitions → New`
2. Fill:
   - **Name:** IT Task SLA
   - **Table:** Catalog Task [sc_task]
   - **Duration:** 1 day
   - **Schedule:** 8-5 weekdays
3. **Start condition tab:** Assignment group is IT – Account Provisioning
4. **Stop condition tab:** State is Closed Complete
5. **Submit**

---

## Step 8 — Test the Solution

### Test 1 — Submit Request
1. Open `https://<instance>.service-now.com/sp`
2. Search `Employee Onboarding`
3. Fill form:
   - Request Type = Onboarding
   - Employee ID = EMP001
   - Requested For = System Administrator
   - Department = IT
   - Manager = Aileen Mottern
   - Joining Date = tomorrow
   - Assets needed = Laptop
4. Click **Order Now**
5. Note the REQ number

### Test 2 — Approve
1. Open `Service-now → Requested Items`
2. Find the new RITM
3. Scroll to Approvers related list
4. Click the pending approval → **Approve**

### Test 3 — Complete Tasks
1. Scroll to Catalog Tasks related list
2. Open each of the 3 tasks
3. Set **State = Closed Complete**
4. Click **Update**

### Test 4 — Verify Completion
1. Open `u_employee_lifecycle_list.do`
2. Confirm new record with **Status = Completed**
3. Open RITM → check **Stage = Completed** (3 green checks)

---

## Step 9 — Package & Export (Optional)

1. `System Applications → My Company Applications` or use Global scope
2. Export update set:
   - `System Update Sets → Local Update Sets → New`
   - Name: `Employee Onboarding Solution`
   - Make all changes
   - Export the XML

Save the update set XML to GitHub under `update-sets/`.

---

## Sequence of Execution (Recommended Order)

1.Custom Table + Fields + Choices

2.Groups + Roles

3.Catalog Item + Variables

4.UI Policies

5.Flow Designer Flow

6.ACLs

7.SLA

8.End-to-end Testing

9.Update Set Export

10.Documentation


---

## Troubleshooting Common Issues

| Issue | Fix |
|-------|-----|
| "Submitted Request is mandatory" | Flow Action #1 — set Submitted Request to `Trigger → Requested Item Record` |
| "Email has no recipients" | Send Email To field — set to `Trigger → Requested Item → Requested for` |
| Choice values not showing in flow | Refresh Flow Designer after adding choices |
| ACL not enforcing | Confirm `security_admin` is elevated when editing |
| Flow doesn't trigger | Verify trigger's catalog item is set correctly |

---

## Summary

| Step | Time Estimate |
|------|---------------|
| Table + Fields + Choices | 15 min |
| Groups + Roles | 10 min |
| Catalog Item + Variables | 20 min |
| UI Policies | 10 min |
| Flow Designer Flow | 60–90 min |
| ACLs | 10 min |
| SLA | 5 min |
| Testing | 15 min |
| **Total** | **~2.5 hours** |

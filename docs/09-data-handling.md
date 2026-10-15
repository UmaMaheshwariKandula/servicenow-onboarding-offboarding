# Data Handling

This document describes how catalog variables from the "Employee Onboarding" catalog item are captured, mapped, and stored in the Employee Lifecycle custom table.

---

## 1. Objective

Store onboarding and offboarding request data in the `u_employee_lifecycle` table with references to user details and catalog variables. Ensure structured tracking for reporting and audit.

---

## 2. Variables to Custom Table Records

Process automation is used to capture catalog variables from the Employee Onboarding catalog item and automatically map them to fields in the `u_employee_lifecycle` table for structured tracking.

---

## 3. Process to Map Variables

1. Create a flow in Flow Designer with a trigger on catalog submission.
2. Use **Get Catalog Variables** action to retrieve submitted values (Employee ID, Manager, Department, Joining/Exit Date, Assets, Access Revocations).
3. Use **Create Record** action and select the Employee Lifecycle table.
4. Click **Add Fields (+)** to map each catalog variable to the corresponding table field.

---

## 4. Variable-to-Field Mapping

| Catalog Variable | Table Field | Type |
|------------------|-------------|------|
| `request_type` | Request Type | Choice |
| `employee_id` | Employee ID | String |
| `requested_for` | Requested For | Reference (User) |
| `department` | Department | Reference (Department) |
| `manager` | Manager | Reference (User) |
| `joining_date` | Joining Date | Date |
| `exit_date` | Exit Date | Date |
| `assets_needed` | Assets needed | Choice (multi) |
| `assets_to_collect` | Assets to Collect | Choice (multi) |
| `special_access` | Special Access | String |
| `access_revocations` | Access Revocations | String |
| `access_revocation_required` | Access Revocation Required | String |
| `special_instructions_comments` | Special Instructions / Comments | String |

---

## 5. Flow Actions Used

### Action #1 — Get Catalog Variables
- **Template Catalog Item:** Employee Onboarding
- **Submitted Request:** Trigger → Requested Item Record
- **Selected variables:** employee_id, department, joining_date, manager, requested_for

### Action #2 — Create Record
- **Table:** `u_employee_lifecycle`
- **Field mapping:**
  - Requested For ← `1 - Get Catalog Variables → requested_for`
  - Employee ID ← `1 - Get Catalog Variables → employee_id`
  - Department ← `1 - Get Catalog Variables → department`
  - Joining Date ← `1 - Get Catalog Variables → joining_date`
  - Manager ← `1 - Get Catalog Variables → manager`
  - Request type ← `Onboarding` (hardcoded)
  - Status ← `New` (hardcoded)

---

## 6. Data Persistence

Every time a user submits the "Employee Onboarding" catalog item:

1. A **Requested Item (RITM)** record is created in `sc_req_item`
2. The flow triggers automatically
3. A new record is created in `u_employee_lifecycle` with all mapped fields
4. The record is linked to the RITM via the Requested For field
5. Status begins at **New** and is updated to **Completed** when all tasks close

---

## 7. Verification

**Test case:** REQ0010002 → RITM0010002

| Step | Result |
|------|--------|
| Catalog item submitted | ✅ REQ0010002 created |
| Lifecycle record created | ✅ EMP002 record with department=IT, manager=Aileen Mottern, joining_date=2026-10-30 |
| Variables mapped correctly | ✅ All 5 selected variables populated |
| Status updates | ✅ New → Completed after tasks closed |

---

## 8. Benefits

| Benefit | Description |
|---------|-------------|
| Structured data | All requests stored in one table for reporting |
| Consistency | Same fields captured for every request |
| Auditability | Full history of each request |
| Reporting | Enables dashboards on onboarding volume and duration |
| Extensibility | Can add more fields or references later |

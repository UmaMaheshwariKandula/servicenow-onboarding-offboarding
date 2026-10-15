# ACL Configuration

This document describes the Access Control Lists (ACLs) implemented on the `u_employee_lifecycle` table to enforce role-based access.

---

## 1. Objective

Ensure secure role-based access so HR, IT, Facilities, and Security teams can only access and act on relevant data — maintaining data confidentiality and compliance.

---

## 2. Elevation Step

To create or modify ACLs, the `security_admin` role must be elevated:

1. Click profile (top-right) → **Elevate role**
2. Check **security_admin**
3. Click **Update**
4. Orange banner confirms: "Elevated: security_admin"

This elevation must be done before adding/editing any ACL.

---

## 3. Default ACLs

When the custom table `u_employee_lifecycle` was created, ServiceNow automatically created 4 default ACLs:

| Name | Operation | Type | Active |
|------|-----------|------|--------|
| u_employee_lifecycle | read | record | true |
| u_employee_lifecycle | write | record | true |
| u_employee_lifecycle | create | record | true |
| u_employee_lifecycle | delete | record | true |

These govern table-level access.

---

## 4. Custom ACLs Added

To enforce stricter access, 3 additional ACLs were configured:

| # | Name | Operation | Type | Requires Role | Description |
|---|------|-----------|------|---------------|-------------|
| 1 | u_employee_lifecycle | read | record | admin | Restrict read to admin role |
| 2 | u_employee_lifecycle | write | record | admin | Restrict write to admin role |
| 3 | u_employee_lifecycle | delete | record | admin | Restrict delete to admin role |

Each ACL is:
- **Type:** record
- **Active:** true
- **Decision Type:** Allow If
- **Condition:** no condition (role-based only)
- **Requires role:** admin

---

## 5. ACL List (Final)

Full list of ACLs on `u_employee_lifecycle`:

| # | Operation | Updated By | Updated |
|---|-----------|------------|---------|
| 1 | delete | admin | 2026-10-12 (default) |
| 2 | write | admin | 2026-10-15 (custom) |
| 3 | create | admin | 2026-10-12 (default) |
| 4 | delete | admin | 2026-10-15 (custom) |
| 5 | read | admin | 2026-10-12 (default) |
| 6 | write | admin | 2026-10-12 (default) |
| 7 | read | admin | 2026-10-15 (custom) |

Total: **7 ACLs** governing read, write, create, delete on the table.

---

## 6. Role-Based Access Model

| Role | Access |
|------|--------|
| admin | Full read/write/create/delete |
| hr.manager | View requests (planned) |
| it.admin | View/complete IT tasks (planned) |
| facilities.staff | View/complete Facilities tasks (planned) |
| security.officer | View/complete Security tasks (planned) |

*(Custom roles can be added to ACLs if the HR/IT/Facilities/Security modules are installed.)*

---

## 7. Access Principle

ACLs enforce:
- **Data confidentiality** — only authorized users view records
- **Least privilege** — users only access what they need
- **Audit compliance** — all access attempts logged
- **Deny by default** — cross-department access is blocked unless granted

---

## 8. Testing the ACLs

### Test 1 — Admin can read
1. Login as admin → open `u_employee_lifecycle_list.do`
2. Records visible ✅

### Test 2 — Non-admin restricted (if roles are configured)
1. Login as a user without admin role
2. Attempt to open `u_employee_lifecycle_list.do`
3. Records blocked ❌ (denied by ACL)

---

## 9. Steps to Replicate ACLs

1. Elevate role to `security_admin`
2. Navigate to `System Security → Access Control → New`
3. Fill:
   - Name: `Employee Lifecycle [u_employee_lifecycle]`
   - Type: record
   - Operation: read (or write/delete)
   - Active: checked
4. Add **Requires role** → `admin`
5. Submit
6. Click Continue on the popup
7. Click Submit on the form
8. Repeat for write and delete operations
9. Verify list shows 7 ACLs

---

## Summary

| Item | Count |
|------|-------|
| Default ACLs | 4 |
| Custom ACLs added | 3 |
| Total ACLs | 7 |
| Operations covered | read, write, create, delete |
| Restricted role | admin |

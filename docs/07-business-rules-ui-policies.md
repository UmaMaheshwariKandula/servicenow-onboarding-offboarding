# Business Rules & UI Policy Configuration

This document describes the UI Policies and business rule logic implemented on the "Employee Onboarding" catalog item to enforce data consistency, dynamic field visibility, and automatic status updates.

---

## 1. Catalog UI Policies

UI Policies dynamically show or hide fields in the onboarding/offboarding form based on the user's Request Type selection. This reduces errors and improves user experience.

### UI Policy 1 — Show Joining Date for Onboarding

| Field | Value |
|-------|-------|
| Short description | Show Joining Date for Onboarding |
| Applies to | Catalog item |
| Catalog Item | Employee Onboarding |
| Active | ✅ |
| Condition | Request Type **is** Onboarding |

**UI Policy Action:**

| Variable | Attribute | Value |
|----------|-----------|-------|
| Joining Date | Visible | True |
| Joining Date | Mandatory | True |

### UI Policy 2 — Show Exit Date for Offboarding

| Field | Value |
|-------|-------|
| Short description | Show Exit Date for Offboarding |
| Applies to | Catalog item |
| Catalog Item | Employee Onboarding |
| Active | ✅ |
| Condition | Request Type **is** Offboarding |

**UI Policy Action:**

| Variable | Attribute | Value |
|----------|-----------|-------|
| Exit Date | Visible | True |
| Exit Date | Mandatory | True |

### UI Policy 3 — Hide Exit Date for Onboarding

| Field | Value |
|-------|-------|
| Short description | Hide Exit Date for Onboarding |
| Applies to | Catalog item |
| Catalog Item | Employee Onboarding |
| Active | ✅ |
| Condition | Request Type **is** Onboarding |

**UI Policy Action:**

| Variable | Attribute | Value |
|----------|-----------|-------|
| Exit Date | Visible | False |

### UI Policy 4 — Hide Joining Date for Offboarding

| Field | Value |
|-------|-------|
| Short description | Hide Joining Date for Offboarding |
| Applies to | Catalog item |
| Catalog Item | Employee Onboarding |
| Active | ✅ |
| Condition | Request Type **is** Offboarding |

**UI Policy Action:**

| Variable | Attribute | Value |
|----------|-----------|-------|
| Joining Date | Visible | False |

---

## 2. Business Rule Logic (Handled via Flow Designer)

Rather than traditional Business Rules (server-side scripts), the equivalent logic was implemented in **Flow Designer** for maintainability and transparency.

| Business Rule Concern | Implemented As |
|-----------------------|----------------|
| Auto-update request status | Flow Action #9: Update Record → Status = Completed |
| Auto-close lifecycle record after tasks complete | Flow Action #8 (Wait for Condition) → Action #9 (Update Record) |
| Restrict invalid submissions | UI Policies + Mandatory field enforcement |
| Trigger background updates | Flow Designer trigger on catalog submission |

### Status Update Logic

| Trigger | Status Change |
|---------|---------------|
| Catalog item submitted | Status = New |
| Manager approves | Status stays New (tasks created) |
| All 3 tasks Closed Complete | Status = Completed |
| Manager rejects | Status = Rejected |

---

## 3. Testing & Verification

### Test 1 — Onboarding Path
1. Open Service Portal → Employee Onboarding
2. Select **Request Type = Onboarding**
3. Verify: **Joining Date appears, Exit Date hides**
4. Fill form, submit
5. Verify: Lifecycle record created with Status = New

### Test 2 — Offboarding Path
1. Open Service Portal → Employee Onboarding
2. Select **Request Type = Offboarding**
3. Verify: **Exit Date appears, Joining Date hides**
4. Fill form, submit
5. Verify: Lifecycle record created with Status = New

### Test 3 — Approval Flow
1. Submit request → approval routed to manager
2. Approve → 3 tasks created
3. Close all tasks → Lifecycle = Completed
4. RITM → Closed Complete

---

## 4. Design Decisions

| Decision | Reason |
|----------|--------|
| UI Policies over Client Scripts | No custom code; admin-editable |
| Flow Designer over Business Rules | Visual, traceable, easier to debug |
| Mandatory fields enforced on form | Prevents incomplete submissions |
| Dynamic field visibility | Better user experience; fewer errors |

---

## Summary

| Component | Count |
|-----------|-------|
| UI Policies | 4 |
| UI Policy Actions | 6 |
| Business Rule equivalents (in flow) | 4 |
| Test scenarios | 3 |

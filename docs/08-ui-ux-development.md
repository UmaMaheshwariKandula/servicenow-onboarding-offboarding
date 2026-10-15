# UI/UX Development & Customization

This document describes the interface design, navigation flow, and usability considerations for the Automated Employee Onboarding & Offboarding system.

---

## 1. Interface Design

### Service Portal Layout

The user-facing interface is the **Service Portal** (`/sp`), which provides a clean, self-service experience.

| Element | Design |
|---------|--------|
| Search bar | Prominent at top — users search "Employee Onboarding" |
| Category | Grouped under "Services" |
| Catalog item card | Icon + short description for quick recognition |
| Form layout | 2-column layout with required fields marked with ✱ |
| Action buttons | "Order Now" (primary), "Save as Draft", "Add to Cart" |
| Right sidebar | Quantity, Delivery Time, and action buttons |

### Form Field Design

Fields are grouped logically:

| Section | Fields |
|---------|--------|
| Request Info | Request Type, Employee ID, Requested For |
| Employee Details | Department, Manager, Joining Date, Exit Date |
| Access | Special Access, Access Revocations, Access Revocation Required |
| Assets | Assets needed, Assets to Collect |
| Notes | Special Instructions / Comments, Attachments |

### Visual Design Principles

- **Clean form layout** — labels left, inputs right
- **Mandatory fields** marked with asterisks
- **Dynamic field visibility** — only relevant fields show
- **Error messages** displayed inline for required fields
- **Consistent typography** — ServiceNow default (OpenSans)

---

## 2. Navigation Flow

### User Journey (Requestor)

1.User opens Service Portal (/sp)
                            ↓

2.Searches "Employee Onboarding"
                            ↓

3.Clicks the catalog item
                            ↓

3.Fills the form (UI policies adjust fields dynamically)
                            ↓

4.Clicks "Order Now"
                            ↓

5.Sees Request Summary with REQ number
                            ↓

6.Tracks request in "Requests" menu


### Fulfiller Journey (IT / Facilities / Security)

1.User logs into ServiceNow (agent UI)
                     ↓

2.Opens "My Tasks" or assignment group queue
                     ↓

3.Opens assigned sc_task
                     ↓

4.Performs action (create account, prepare workspace, issue badge)
                     ↓

5.Sets State = Closed Complete
                     ↓

6.Flow proceeds to next stage


### Manager Journey


1.Receives approval notification (email + ServiceNow notification)
                 ↓

2.Opens RITM or "My Approvals"
                 ↓

3.Reviews request
                 ↓

4.Clicks Approve / Reject
                 ↓

5.Flow continues to Approved / Rejected branch


### Admin Journey
1.Uses Flow Designer to inspect automation

                   ↓
2.Uses Execution Details to debug
                   ↓

3.Uses ACLs to manage access
                   ↓

4.Uses Reports for analytics

## 3. Usability Considerations

### For Requestors
- **Self-service** — no need to email HR; submit through portal
- **Dynamic form** — irrelevant fields hidden → less confusion
- **Clear required fields** — enforced by UI policies
- **Request tracking** — real-time status visibility

### For Approvers
- **One-click approval** from email or ServiceNow
- **Context visible** — all request details on the RITM
- **No back-and-forth** — request data already captured

### For Fulfillers
- **Auto-assigned tasks** — no manual routing
- **Clear task descriptions** — Short description explains exactly what to do
- **SLA tracking** — response time measured automatically

### For Admins
- **Low-code build** — no custom scripts to maintain
- **Flow Designer transparency** — visual flow, easy to update
- **ACLs at table level** — centralized access control

---

## 4. Responsive & Accessible

- Service Portal is **responsive** — works on desktop, tablet, mobile
- Forms use **standard ServiceNow components** — accessible by default
- **Color contrast** meets accessibility guidelines
- **Keyboard navigation** supported on all forms

---
## 5. Dynamic Form Behavior (UI Policies)

| Request Type | Joining Date | Exit Date |
|--------------|--------------|-----------|
| Onboarding | ✅ Visible | ❌ Hidden |
| Offboarding | ❌ Hidden | ✅ Visible |

The form **automatically adjusts** based on Request Type, keeping the UI clean and relevant.

---

## 6. Screenshots

Refer to `Screenshots/phase3/` for:
- Service Portal home
- Catalog item card
- Form filled (Onboarding view)
- Form filled (Offboarding view)
- Request Summary page
- RITM view with tasks

---

## Summary

| Aspect | Implementation |
|--------|----------------|
| Interface | Service Portal (`/sp`) |
| Navigation | Search → item → form → submit → track |
| Usability | Dynamic fields, self-service, one-click approvals |
| Responsiveness | Mobile-friendly via Service Portal |
| Dynamic Behavior | UI Policies show/hide date fields |

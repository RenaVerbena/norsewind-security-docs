# Privileged Access Request, Approval, and Review Procedure

| | |
|---|---|
| **Document ID** | PRO-IS-031 |
| **Version** | 1.3 |
| **Owner** | Manager, Identity and Access Management |
| **Approver** | Director, Identity and Access Management |
| **Effective date** | November 1, 2026 |
| **Review cycle** | Annual, or upon change to the ITSM or PAM platform |
| **Classification** | Internal |
| **Governing documents** | [POL-IS-004 Access Control Policy](POL-IS-004_Access-Control-Policy.md); [STD-IS-012 Privileged Access Management Standard](STD-IS-012_Privileged-Access-Management-Standard.md) |
| **Related documents** | RB-IR-007 Suspected Compromise of a Privileged Account; PRO-HR-014 Offboarding Procedure |
| **Questions** | iam@norsewind.example |

## Summary

This procedure describes how privileged access is requested, approved, provisioned, changed, removed, and recertified at Norsewind, and what records each step produces. It applies to IAM analysts, approvers, requesters, and account owners, and it covers accounts used by AI tools and agents. Every step that creates an audit record says so. Parameters such as timeframes and review intervals come from STD-IS-012 and are not restated here.

## AI-Era Assumptions

This procedure is written to hold under the following assumptions. They are Norsewind's own and go beyond the minimum requirements of the frameworks listed under Control Mapping.

- Voice, video, and email can be convincingly impersonated, so no access decision rests on a live call or a single message alone.
- An attacker who gains a foothold can move at machine speed, so controls favor automated enforcement over manual review.
- AI tools and agents that act on Norsewind systems are privileged identities.
- Operational technology cannot be patched on IT timelines and relies on documented compensating controls.

Steps that follow from these assumptions rather than from a mapped control are tagged `[Beyond framework minimum]`. Review the assumptions at each scheduled review of this document.

## I. Purpose

To provide a repeatable process that satisfies STD-IS-012 and produces the evidence internal and external auditors request during ISO 27001, SOC 2, and PCI DSS assessments.

## II. Roles

| Role | In this procedure |
|---|---|
| Requester | The person who needs access, or their manager. For an AI agent account, the human owner of the agent |
| Approver | The system or information owner listed in the PAM inventory for the target system. For broad administrative roles, the CISO is a second approver |
| IAM analyst | Validates, provisions, records, and removes access |
| Account owner | The person accountable for a privileged account; completes recertification |
| Security Operations | Receives notifications of provisioning events and reviews exceptions |

## III. Prerequisites

Before submitting a request, confirm that:

- A. The requester has completed the privileged user training module within the last 12 months.
- B. The requester has a registered phishing-resistant authenticator (see STD-IS-009).
- C. The target system and its owner appear in the PAM inventory. If not, stop and open an inventory request first.
- D. For an AI agent account, the agent is registered in the AI tool inventory with a named human owner and a written scope of action. `[Beyond framework minimum]`

## IV. Procedure

### 4.1 Request

1. The requester opens a **Privileged Access Request** in the ITSM system using the standard form. Requests made by phone, video call, chat, or email are not accepted as requests; the IAM analyst directs the requester to the form. `[Beyond framework minimum]`
2. Complete every required field: target system, role or permission set requested, business justification, expected duration (default 12 months maximum), and whether standing access is needed and why. For an AI agent account, also state the agent's scope of action and the systems it will act on. `[Beyond framework minimum]`
3. Attach the manager's confirmation if the requester is submitting on their own behalf.

*Record produced:* ITSM ticket with request details and timestamp.

### 4.2 Approval

4. The ITSM workflow routes the ticket to the approver named in the PAM inventory for that system.
5. The approver reviews the justification against least privilege. The approver may approve, approve with a narrower role, or reject with a reason.
6. If the request is for a broad administrative role (see STD-IS-012 Section IV.C), the workflow routes to the CISO for second approval.
7. Requests not actioned within 5 business days are escalated to the approver's manager. Requests not actioned within 10 business days are closed and must be resubmitted.

*Record produced:* Approval or rejection with approver identity, timestamp, and comments, retained on the ticket.

### 4.3 Provisioning

8. The IAM analyst verifies that the prerequisites in Section III are met and that the approved role matches the request. Any mismatch goes back to the approver.
9. Provision the access through the PAM platform. Configure just-in-time elevation unless standing access was approved.
10. Record the approval ticket number, account type, owner, and expiry date in the PAM inventory entry. For an AI agent account, record the scope of action. `[Beyond framework minimum]`
11. Notify the requester and Security Operations through the automated provisioning notice.

*Record produced:* PAM inventory entry linked to the ticket; provisioning log entry in the SIEM.

**Verification:** Before closing the ticket, confirm the account appears in the PAM inventory, the expiry date is set, and the SIEM shows the provisioning event. If any of the three is missing, do not close the ticket.

### 4.4 Modification

12. Changes to an existing privileged account (broader role, extended duration, different system, or, for an AI agent, a wider scope of action) follow steps 1 through 11 as a new request. Narrowing access or shortening duration may be actioned by the IAM analyst on the account owner's written request without approver review.

### 4.5 Removal

13. On notification of departure or role change from HR or the manager, the IAM analyst removes privileged access within the timeframe in STD-IS-012 Section IX.C. For involuntary terminations, HR notifies IAM before the termination meeting where possible so removal can coincide with it.
14. Rotate any vaulted credential the departing person could have known (STD-IS-012 Section VII).
15. Update the PAM inventory entry with the removal date and ticket reference.
16. When an AI tool or agent is retired or its hosting changes, the owner submits a removal request and the IAM analyst disables the agent account and rotates any credential it held. `[Beyond framework minimum]`

*Record produced:* Removal ticket with timestamps showing notification time and removal time. This pair of timestamps is the evidence auditors test against the timeframe in the standard.

### 4.6 Quarterly Recertification

17. On the first business day of each quarter, the PAM platform generates a recertification campaign listing every privileged account by owner.
18. Each account owner reviews their list and marks each account **Retain**, **Modify**, or **Remove**, with a comment for any Modify or Remove. For an AI agent account, the owner also confirms that the recorded scope of action is still accurate. `[Beyond framework minimum]`
19. The IAM analyst actions all Modify and Remove decisions within 5 business days of the owner's response.
20. Accounts not certified by the due date are disabled automatically per STD-IS-012 Section IX.B. The IAM analyst notifies the owner and their manager.
21. The IAM manager reviews the completed campaign, documents completion rate and exceptions, and files the campaign report.

*Record produced:* Campaign report with per-account decisions, owner identities, timestamps, and completion metrics.

## V. Records and Evidence

The following records are retained for a minimum of 3 years and are the standard evidence package for an access control audit request:

| Evidence | Where | Produced by step |
|---|---|---|
| Access request and approval | ITSM ticket | 4.1, 4.2 |
| Provisioning confirmation | PAM inventory and SIEM | 4.3 |
| Removal timing | ITSM removal ticket | 4.5 |
| Recertification results | PAM campaign report | 4.6 |
| Training completion | Learning management system | Section III |
| AI agent scope record | AI tool inventory and PAM inventory | Section III, 4.3, 4.6 |

## VI. Control Mapping

| Framework | Reference |
|---|---|
| ISO/IEC 27001:2022 Annex A | A.5.16, A.5.18, A.6.5 Responsibilities after termination, A.8.2 |
| SOC 2 (TSC 2017) | CC6.2, CC6.3 |
| NIST CSF 2.0 | PR.AA-01, PR.AA-05 |
| NIST SP 800-53 Rev 5 | AC-2, PS-4, PS-5 |
| PCI DSS 4.0 | Requirements 7.2, 8.2 |

Steps tagged `[Beyond framework minimum]` are not required by any framework above. They follow from the AI-Era Assumptions section.

## Revision History

| Version | Date | Author | Summary |
|---|---|---|---|
| 1.3 | 2026-11-01 | IAM | Added AI-Era Assumptions section; form-only intake rule (step 1); AI agent prerequisites, scope fields, retirement step, and recertification check; AI agent scope record added to evidence table |
| 1.2 | 2026-10-01 | IAM | Aligned to STD-IS-012 v2.0; added verification step at 4.3; added evidence table |
| 1.1 | 2025-04-10 | IAM | Added escalation timers for approvals |
| 1.0 | 2024-03-01 | IAM | Initial release |

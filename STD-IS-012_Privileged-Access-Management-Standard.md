# Privileged Access Management Standard

| | |
|---|---|
| **Document ID** | STD-IS-012 |
| **Version** | 2.1 |
| **Owner** | Director, Identity and Access Management |
| **Approver** | Chief Information Security Officer |
| **Effective date** | November 1, 2026 |
| **Review cycle** | Annual, or upon change to the PAM platform, authentication technology, or an audit finding |
| **Classification** | Internal |
| **Parent policy** | [POL-IS-004 Access Control Policy](POL-IS-004_Access-Control-Policy.md) |
| **Related documents** | STD-IS-009 Authentication Standard; STD-IS-020 Logging and Monitoring Standard; [PRO-IS-031 Privileged Access Request, Approval, and Review Procedure](PRO-IS-031_Privileged-Access-Request-and-Review-Procedure.md); RB-IR-007 Suspected Compromise of a Privileged Account |
| **Questions** | iam@norsewind.example |

## Summary

This standard sets the mandatory parameters for privileged accounts at Norsewind: how they are inventoried, provisioned, authenticated, used, monitored, and reviewed. It applies to every account with administrative or otherwise elevated rights on corporate IT, security tooling, operational technology, and customer-facing systems, including accounts used by AI tools and agents. The requirements that matter most: privileged access runs through the PAM platform, uses phishing-resistant multi-factor authentication, is time-bound, is fully logged, and is recertified quarterly.

## AI-Era Assumptions

This standard is written to hold under the following assumptions. They are Norsewind's own and go beyond the minimum requirements of the frameworks listed under Control Mapping.

- Voice, video, and email can be convincingly impersonated, so no access decision rests on a live call or a single message alone.
- An attacker who gains a foothold can move at machine speed, so controls favor automated enforcement over manual review.
- AI tools and agents that act on Norsewind systems are privileged identities.
- Operational technology cannot be patched on IT timelines and relies on documented compensating controls.

Requirements that follow from these assumptions rather than from a mapped control are tagged `[Beyond framework minimum]`. Review the assumptions at each scheduled review of this document.

## I. Purpose

POL-IS-004 requires that privileged access be separately controlled, individually approved, subject to stronger authentication, monitored, and reviewed more frequently than standard access. This standard defines what those requirements mean in measurable terms so that they can be implemented consistently and tested by internal and external auditors.

## II. Scope

- A. All accounts, human or non-human, that hold privileged access as defined in POL-IS-004, on any Norsewind system. Non-human accounts include service accounts and accounts used by AI tools and agents.
- B. Operational technology (OT) systems are in scope. Where an OT system cannot technically meet a requirement in this standard, Section X applies.
- C. Customer-managed accounts on customer-owned equipment located at Norsewind are out of scope unless Norsewind Personnel hold them.

## III. Account Types and Inventory

- A. Privileged accounts shall be classified as one of: **named administrative** (assigned to one person), **service** (used by an application or automated process), **AI agent** (used by an AI tool or autonomous agent that can act on systems or data) `[Beyond framework minimum]`, **shared functional** (used by a team where the platform cannot support named accounts), or **break-glass** (emergency use only).
- B. The IAM team shall maintain an inventory of all privileged accounts in the PAM platform, recording account type, system, owner, approval reference, and date of last review. For AI agent accounts, the inventory shall also record the agent's approved scope of action and the human owner accountable for its behavior. `[Beyond framework minimum]` The inventory shall be reconciled against system-generated account lists at least monthly.
- C. Any privileged account discovered outside the inventory shall be treated as unauthorized, disabled within one business day of discovery, and reported to Security Operations.

## IV. Provisioning

- A. Privileged access shall be requested and approved in accordance with PRO-IS-031 before provisioning.
- B. Named administrative accounts shall be separate from the same person's standard user account. Privileged accounts shall not be used for email, web browsing, or other routine work.
- C. Privileged access shall be granted to the narrowest role or permission set available on the target system. Membership in broad administrative groups (for example, domain or tenant-wide administrator) requires CISO approval.
- D. Access shall be time-bound. Standing privileged access is permitted only where the PAM platform cannot broker just-in-time elevation for the target system, and the justification shall be recorded in the inventory.
- E. AI agent accounts shall be granted only the permissions needed for the agent's approved scope, shall never hold broad administrative group membership, and shall be reviewed whenever the agent's scope or underlying model changes. `[Beyond framework minimum]`

## V. Authentication

- A. All privileged access shall require multi-factor authentication. For named administrative accounts, the second factor shall be phishing-resistant (hardware security key or platform authenticator bound to the device). `[Beyond framework minimum; aligned with NIST SP 800-63B guidance]`
- B. Privileged credentials shall be vaulted in the PAM platform. Personnel shall not know or store the password of a vaulted account.
- C. Where a system does not support vaulting or MFA, access shall be brokered through a PAM-managed jump host that enforces both.
- D. Requests to reset, re-enroll, or recover privileged authentication shall not be fulfilled on the basis of a voice or video call alone. The IAM team shall verify the requester through a second channel the requester does not control. `[Beyond framework minimum]`

## VI. Session Controls

- A. Interactive privileged sessions on production IT, customer-facing, and OT systems shall be initiated through the PAM platform and recorded.
- B. Sessions shall expire after a maximum of 8 hours. Idle sessions shall terminate after 15 minutes.
- C. Checked-out credentials shall be rotated automatically at check-in and in any case no less than every 24 hours while checked out.

## VII. Credential Rotation

| Account type | Rotation |
|---|---|
| Named administrative (vaulted) | On every check-in; forced rotation at least every 90 days |
| Service | At least every 180 days, and immediately upon personnel departure of anyone with knowledge of the credential |
| AI agent | At least every 90 days, and immediately upon any change to the agent's scope, model, or hosting `[Beyond framework minimum]` |
| Shared functional | On every check-in; forced rotation at least every 30 days |
| Break-glass | Immediately after every use; verified quarterly |

Any credential suspected of exposure shall be rotated immediately and the event handled under RB-IR-007.

## VIII. Logging and Monitoring

- A. All privileged authentication events, session starts and ends, credential check-outs, and privilege elevations shall be forwarded to the SIEM within 5 minutes of occurrence.
- B. Session recordings shall be retained for 12 months. Log retention shall follow STD-IS-020.
- C. Security Operations shall maintain detection rules for, at minimum: privileged login outside approved hours or locations, privileged account creation outside the PAM platform, use of a break-glass account, repeated failed privileged authentication, and activity by an AI agent account outside its approved scope `[Beyond framework minimum]`.
- D. Detection rules for privileged account creation outside the PAM platform and for break-glass use shall trigger automatic disablement of the account pending review, without waiting for analyst confirmation. `[Beyond framework minimum]`

## IX. Access Review and Recertification

- A. Every privileged account shall be recertified by its owner quarterly using the review workflow in PRO-IS-031.
- B. Accounts not recertified within 10 business days of the review due date shall be disabled automatically.
- C. Privileged access shall be removed within 4 hours of notification of an involuntary termination and within 24 hours of any other departure or role change that removes the need.

## X. Operational Technology Exceptions

Where a production line controller, cleanroom environmental control, building management, or manufacturing execution system cannot support a requirement in Sections V through VIII, the facilities engineering lead and the IAM team shall jointly document the gap, the compensating control (for example, physical access restriction, network isolation, or supervised sessions), and a remediation date. The record shall be approved by the CISO under the exception process in POL-IS-004 and reviewed every 6 months.

## XI. Compliance Measurement

The IAM team shall report the following to the CISO monthly: number of privileged accounts by type, percentage vaulted, percentage with standing access, recertification completion rate, count of unauthorized accounts discovered, and count of AI agent accounts with a current scope record. These figures feed the quarterly security report to the executive team.

## XII. Control Mapping

| Framework | Reference |
|---|---|
| ISO/IEC 27001:2022 Annex A | A.5.16, A.5.18, A.8.2, A.8.5, A.8.15 Logging, A.8.16 Monitoring activities |
| SOC 2 (TSC 2017) | CC6.1, CC6.2, CC6.3, CC7.2 |
| NIST CSF 2.0 | PR.AA-01, PR.AA-03, PR.AA-05; DE.CM |
| NIST SP 800-53 Rev 5 | AC-2, AC-6, IA-2, IA-5, AU-2, AU-12 |
| PCI DSS 4.0 | Requirements 7, 8, and 10 |

Requirements tagged `[Beyond framework minimum]` are not required by any framework above. They follow from the AI-Era Assumptions section. Section V.A is aligned with NIST SP 800-63B, which recommends phishing-resistant authenticators but is not among the frameworks this standard is audited against.

## Revision History

| Version | Date | Author | Summary |
|---|---|---|---|
| 2.1 | 2026-11-01 | IAM / Information Security Governance | Added AI-Era Assumptions section; added AI agent account type, scope records, and rotation rule; added second-channel verification for authentication recovery (V.D); added automatic disablement trigger (VIII.D); added AI agent scope metric |
| 2.0 | 2026-10-01 | IAM / Information Security Governance | Moved parameters from POL-IS-004 v2.1 into this standard; added phishing-resistant MFA requirement; added OT exception section; set quarterly recertification |
| 1.0 | 2024-03-01 | IAM | Initial release |

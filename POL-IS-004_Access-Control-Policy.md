# Access Control Policy

| | |
|---|---|
| **Document ID** | POL-IS-004 |
| **Version** | 3.1 |
| **Owner** | Chief Information Security Officer |
| **Approver** | Chief Operating Officer |
| **Effective date** | November 1, 2026 |
| **Review cycle** | Annual, or upon material change to business, regulatory, or contractual requirements |
| **Classification** | Internal |
| **Related documents** | [STD-IS-012 Privileged Access Management Standard](STD-IS-012_Privileged-Access-Management-Standard.md); STD-IS-009 Authentication Standard; [PRO-IS-031 Privileged Access Request, Approval, and Review Procedure](PRO-IS-031_Privileged-Access-Request-and-Review-Procedure.md); POL-IS-001 Information Security Policy |
| **Questions** | infosec-governance@norsewind.example |

## Summary

This policy defines how Norsewind Precision Manufacturing, Inc. ("Norsewind") grants, reviews, and removes access to its information systems, plant systems, and customer-owned equipment on its floor. It applies to every person and every account with access to Norsewind systems, including accounts used by AI tools and agents. The requirements that matter most: access is granted on least privilege and need-to-know, every account is tied to an accountable owner, privileged access is separately controlled, and all access is reviewed on a defined schedule.

## AI-Era Assumptions

This policy is written to hold under the following assumptions. They are Norsewind's own and go beyond the minimum requirements of the frameworks listed under Control Mapping.

- Voice, video, and email can be convincingly impersonated, so no access decision rests on a live call or a single message alone.
- An attacker who gains a foothold can move at machine speed, so controls favor automated enforcement over manual review.
- AI tools and agents that act on Norsewind systems are privileged identities.
- Operational technology cannot be patched on IT timelines and relies on documented compensating controls.

Requirements that follow from these assumptions rather than from a mapped control are tagged `[Beyond framework minimum]`. Review the assumptions at each scheduled review of this document.

## I. Purpose

Norsewind manufactures precision components for customers whose production lines depend on them. Controlling who can reach which systems, and proving that control to customers and auditors, is fundamental to that responsibility. This policy establishes the mandatory requirements for access control across Norsewind. Measurable parameters (session timeouts, review intervals, authentication strength) are defined in the standards listed under Related Documents so they can be updated as technology and risk change without amending this policy.

## II. Scope

This policy applies to:

- A. All Norsewind employees, contractors, consultants, and vendor representatives ("Personnel") who are granted access to Norsewind systems or facilities.
- B. All Norsewind information systems, including corporate IT, security tooling, operational technology (production line controllers, the manufacturing execution system, cleanroom environmental controls, and building management systems), and any system that stores, processes, or transmits Confidential Information.
- C. All accounts, whether assigned to a person, a service, an AI tool or agent, a shared function, or an emergency (break-glass) purpose.

Customer-owned tooling and test equipment located at Norsewind, and customer-managed accounts on that equipment, are governed by the customer's own policies and the applicable supply agreement, except where Norsewind Personnel are granted access to them.

## III. Policy Statements

- A. **Least privilege.** Access shall be limited to the minimum necessary for Personnel to perform their assigned duties. Access that is convenient but not required shall not be granted.
- B. **Need to know.** Access to Confidential Information shall be granted only to Personnel with a documented business need, approved by the information owner.
- C. **Accountability.** Every account shall be assigned to a named owner who is accountable for its use. Shared accounts are prohibited except where technically unavoidable, in which case they shall be managed under STD-IS-012.
- D. **Unique identification.** Each person shall be issued a unique user ID. Credentials shall not be shared, and authentication shall meet the requirements of STD-IS-009.
- E. **Authorization before access.** No access shall be provisioned until a request has been approved by the appropriate authority as defined in the applicable procedure. Approval records shall be retained.
- F. **Privileged access.** Administrative and other privileged access shall be separately controlled, individually approved, subject to stronger authentication, monitored, and reviewed more frequently than standard access, as defined in STD-IS-012.
- G. **Separation of duties.** Critical functions shall be divided so that no single person can complete a sensitive transaction end to end without independent review. Where the organization's size makes this impractical, compensating controls shall be documented and approved by the CISO.
- H. **Timely removal.** Access shall be removed or adjusted when Personnel change roles, complete an engagement, or leave Norsewind, within the timeframes defined in the applicable standard. Involuntary terminations shall be treated as time-critical.
- I. **Periodic review.** Access rights shall be reviewed by the responsible owners at defined intervals, and access no longer justified shall be removed. Review results shall be recorded.
- J. **Physical and logical alignment.** Physical access to production floors, cleanrooms, tool rooms, network rooms, and utility areas shall be governed by the same principles of least privilege, approval, and review, and shall be coordinated with logical access where systems permit.
- K. **Logging.** Access events, including successful and failed authentication and privilege use, shall be logged, protected from tampering, and retained as defined in STD-IS-020 Logging and Monitoring Standard.
- L. **Verification of identity for access decisions.** `[Beyond framework minimum]` Any decision to grant, elevate, or restore access that is requested by voice, video, or email shall be confirmed through a second channel the requester does not control (a callback to a directory number, a ticket reference, or in-person verification) before it is acted on.
- M. **AI tools and agents.** `[Beyond framework minimum]` Accounts used by AI tools and autonomous agents that can act on Norsewind systems or data shall be treated as privileged, assigned an accountable human owner, registered in the AI tool inventory, and governed under STD-IS-012.

## IV. Roles and Responsibilities

| Role | Responsibility |
|---|---|
| Chief Information Security Officer | Owns this policy, approves standards under it, adjudicates exceptions |
| Information owners | Approve access to the information and systems they own; complete periodic reviews |
| Identity and Access Management (IAM) team | Operates provisioning, deprovisioning, and review tooling; maintains records |
| People managers | Request access for their staff, confirm role changes, and initiate removal on departure |
| Security Operations | Monitors access activity and responds to anomalies |
| Internal Audit / GRC | Tests compliance with this policy and reports results to management |
| All Personnel | Use only the access granted to them, protect their credentials, and report suspected misuse |

## V. Exceptions

Exceptions to this policy shall be requested in writing, describe the business justification and compensating controls, carry an expiration date not exceeding twelve months, and be approved by the CISO. Approved exceptions shall be recorded in the exception register and reviewed before expiry.

## VI. Enforcement

Violations of this policy may result in removal of access, disciplinary action up to and including termination, and, for vendor representatives, termination of the engagement. Suspected violations shall be reported to Security Operations.

## VII. Control Mapping

| Framework | Reference |
|---|---|
| ISO/IEC 27001:2022 Annex A | A.5.15 Access control; A.5.16 Identity management; A.5.18 Access rights; A.8.2 Privileged access rights; A.8.5 Secure authentication |
| SOC 2 (TSC 2017) | CC6.1, CC6.2, CC6.3 |
| NIST CSF 2.0 | PR.AA (Identity Management, Authentication, and Access Control) |
| NIST SP 800-53 Rev 5 | AC-2, AC-3, AC-5, AC-6, IA-2 |
| PCI DSS 4.0 | Requirements 7 and 8 |

Statements III.L and III.M are not required by any framework above. They follow from the AI-Era Assumptions section.

## VIII. Definitions

- **Confidential Information:** Information designated Internal, Confidential, or Restricted under POL-IS-002 Information Classification Policy, including customer designs and specifications and information about Norsewind processes, facilities, and systems.
- **Personnel:** Employees, contractors, consultants, and vendor representatives with access to Norsewind systems or facilities.
- **Privileged access:** Any access that permits changes to system configuration, security controls, or other users' access, or that bypasses normal controls.
- **AI tool or agent:** Software that uses a machine learning model to read, generate, or act on information, including assistants, copilots, and autonomous agents, whether built by Norsewind or licensed from a vendor.

## Revision History

| Version | Date | Author | Summary |
|---|---|---|---|
| 3.1 | 2026-11-01 | Information Security Governance | Added AI-Era Assumptions section; added statements III.L (second-channel verification) and III.M (AI tools and agents); added AI tool or agent definition; added accounts used by AI tools and agents to scope |
| 3.0 | 2026-10-01 | Information Security Governance | Restructured to separate policy statements from parameters now held in STD-IS-012 and STD-IS-009; added OT systems to scope; added physical and logical alignment statement |
| 2.1 | 2024-09-15 | Information Security Governance | Annual review; minor wording updates |
| 2.0 | 2023-09-01 | Information Security Governance | Added privileged access and separation of duties statements |

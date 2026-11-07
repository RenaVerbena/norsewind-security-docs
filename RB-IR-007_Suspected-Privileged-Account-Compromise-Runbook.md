# Runbook: Suspected Compromise of a Privileged Account

| | |
|---|---|
| **Document ID** | RB-IR-007 |
| **Version** | 2.2 |
| **Owner** | Manager, Security Operations |
| **Approver** | Director, Security Operations |
| **Effective date** | November 1, 2026 |
| **Last exercised** | August 2026 (tabletop) |
| **Review cycle** | After every use or exercise; at least every 6 months |
| **Classification** | Confidential |
| **Governing documents** | POL-IS-006 Incident Response Policy; [STD-IS-012 Privileged Access Management Standard](STD-IS-012_Privileged-Access-Management-Standard.md) |
| **Related documents** | [PRO-IS-031 Privileged Access Request, Approval, and Review Procedure](PRO-IS-031_Privileged-Access-Request-and-Review-Procedure.md); PRO-IR-002 Incident Declaration and Severity Procedure; PRO-IR-009 Evidence Handling Procedure; PRO-LG-003 Incident Materiality Assessment; RB-IR-004 Endpoint Isolation |

## Summary

Use this runbook when there is reason to believe a privileged account at Norsewind is being used by someone other than its owner, or its credential has been exposed. It takes the on-call analyst from confirmation through containment, investigation, recovery, and closure. Default severity is **High**. The three things that matter most: contain first (disable the account and kill its sessions), preserve evidence before you change anything else, and get Legal into the loop early because a privileged compromise on a customer-facing or OT system can be reportable.

## AI-Era Assumptions

This runbook is written to hold under the following assumptions. They are Norsewind's own and go beyond the minimum requirements of the frameworks listed under Control Mapping.

- Voice, video, and email can be convincingly impersonated, so no access decision rests on a live call or a single message alone.
- An attacker who gains a foothold can move at machine speed, so controls favor automated enforcement over manual review.
- AI tools and agents that act on Norsewind systems are privileged identities.
- Operational technology cannot be patched on IT timelines and relies on documented compensating controls.

Steps that follow from these assumptions rather than from a mapped control are tagged `[Beyond framework minimum]`. Review the assumptions after every use or exercise of this runbook.

## When to use this runbook

Any of the following:

- A detection rule from STD-IS-012 Section VIII.C fires (privileged login from an unapproved location or time, privileged account created outside the PAM platform, break-glass use without an open change, repeated failed privileged authentication followed by success, or an AI agent account acting outside its approved scope).
- An account owner reports activity they didn't perform.
- A vaulted credential is found outside the vault (in a ticket, chat, script, or repository).
- A threat intelligence or vendor notification names a Norsewind privileged credential.
- An adjacent incident (phishing, malware on an admin workstation) involves a user who holds privileged access.
- Someone claiming to be an account owner, executive, or IT staff member requests a privileged reset, re-enrollment, or access change by voice or video and the request fails second-channel verification. Treat the failed verification itself as a suspected compromise. `[Beyond framework minimum]`

If the account is a **customer-managed** account on customer-owned equipment, do not use this runbook. Notify the customer through the account team per the supply agreement.

## Severity

| Severity | Use when |
|---|---|
| **Critical** | Confirmed unauthorized use on an OT system, a customer-facing system, the PAM platform itself, or identity infrastructure (directory, SSO, MFA) |
| **High** (default) | Confirmed or strongly suspected unauthorized use on any other production IT system |
| **Medium** | Credential exposure with no evidence of use, contained within 1 hour |

Declare severity per PRO-IR-002. You can raise severity at any time. Lowering it requires the Incident Commander.

## Roles

| Role | Who | Does |
|---|---|---|
| On-call SOC analyst | Security Operations rotation | Runs this runbook until an Incident Commander is assigned |
| Incident Commander (IC) | Security Operations lead on call | Owns decisions, severity, and communications from assignment onward |
| IAM engineer | IAM on-call | Executes account and credential actions in the PAM platform and directory |
| Facilities engineering lead | Critical facilities on-call | Required for any OT system; validates that containment won't stop a running line or affect environmental controls or safety systems |
| Legal | Duty counsel | Materiality assessment and external notification decisions |
| Communications lead | Assigned by IC | Internal updates; customer notifications with the account team |

## Pre-authorized actions

The following containment actions are pre-approved by the Director of Security Operations for any High or Critical declaration under this runbook. The on-call analyst executes them without waiting for the IC and records them in the ticket. `[Beyond framework minimum]`

- Disable the suspected account in the PAM platform and directory (except service accounts and OT accounts, which follow steps 2.1 and the OT rule below).
- Terminate all active sessions and revoke tokens for the account.
- Isolate a Norsewind endpoint identified as the source.
- Suspend an AI agent whose account is involved.

## Before you start

- Open an incident ticket in the ITSM system and record the start time. Every action below gets a timestamped note in that ticket.
- Pull up the PAM inventory entry for the account: owner, systems, account type, standing versus just-in-time. For an AI agent account, also pull its scope record and the name of its human owner.
- If the account touches an OT system, page the facilities engineering lead **now**, before containment. Disabling an account on a line controller or environmental control system can stop production or have physical consequences.

## Steps

### 1. Confirm

1.1 Contact the account owner by calling back on the number listed in the corporate directory. Do not use a number, email, or meeting link supplied in the alert or by the person who raised it, and do not treat a voice or video call alone as proof of identity. `[Beyond framework minimum]` Ask what they did in the last 24 hours. Do not send the alert details in writing until you know the owner's endpoint is clean.

1.2 In the SIEM, pull the account's activity for the last 7 days: authentication events, source IPs and devices, session start/end, commands or changes where recorded, and PAM check-outs. For an AI agent account, pull the agent's action log and compare it to its scope record.

1.3 Compare against the owner's account. Unexplained sessions, new source locations, off-hours activity, actions the owner denies, or agent actions outside scope confirm the incident.

**Decision:** If the owner explains all activity and the SIEM agrees, close as a false positive with the evidence attached and stop here. Otherwise declare the incident and assign severity.

### 2. Contain

2.1 Disable the account in the PAM platform and in the directory. For service accounts, coordinate with the application owner on timing, but do not delay more than 30 minutes at High or Critical.

2.2 Terminate all active sessions and revoke tokens and refresh tokens for the account across SSO, cloud consoles, and VPN.

2.3 Rotate every vaulted credential the account could reach, and any credential the account may have viewed during the suspicious window.

2.4 If the source device is a Norsewind endpoint, isolate it per RB-IR-004. Do not power it off.

2.5 For a **break-glass** account, rotate it and confirm the sealed backup credential was not accessed.

2.6 For an **AI agent** account, suspend the agent's integration or runtime, preserve its action log and prompt history before anything is cleared, and notify the agent's human owner. `[Beyond framework minimum]`

**Verification:** Attempt to authenticate as the account from a test system. It must fail. Confirm in the SIEM that no new sessions have started since 2.2. Record both checks in the ticket.

### 3. Preserve evidence

3.1 Export the SIEM activity for the account (last 30 days) and the PAM session recordings for the suspicious window to the evidence share per PRO-IR-009. Record hashes.

3.2 Snapshot any affected system before remediation if the system supports it.

3.3 Do not run cleanup scripts, reimage, or delete accounts created by the attacker until the IC confirms evidence is preserved.

### 4. Investigate scope

4.1 Identify how the credential was obtained: phishing, deepfake voice or video used against the help desk or IAM, malware on an admin workstation, exposed in a repository or ticket, reused password, vendor compromise, or prompt injection against an AI agent. Check the owner's standard account and mailbox for related activity.

4.2 List every system the account authenticated to during the suspicious window. For each, look for: new accounts, changed permissions, new scheduled tasks or services, configuration changes, data access or export, changes to logging.

4.3 Check for persistence: additional privileged accounts created outside the PAM platform (STD-IS-012 Section III.C), new MFA devices registered, changed recovery settings, new API keys, and new or modified AI agent integrations.

4.4 If the account reached a customer-facing system or customer-owned equipment, identify which customers' designs, specifications, or equipment were reachable and whether any were accessed. Hand this list to the IC for the customer notification decision.

4.5 Update severity if the scope is larger than first assessed.

### 5. Eradicate

5.1 Remove attacker-created accounts, keys, tasks, and configuration changes identified in step 4, after evidence preservation is confirmed.

5.2 Remediate the initial access vector (block the sender, remove malware, purge the exposed credential from the repository and its history, close the vendor access, correct the agent's input handling or scope).

5.3 Reimage any endpoint used by the attacker rather than cleaning it.

### 6. Recover

6.1 Re-enable the account only after: the owner's endpoint is clean or replaced, a new credential is vaulted, and the owner has re-enrolled a phishing-resistant authenticator in person or over a video call initiated by IAM to a directory-listed contact, with a second IAM staff member confirming identity. `[Beyond framework minimum]`

6.2 Restore any affected configuration from a known-good state and have the system owner verify function.

6.3 For OT systems, the facilities engineering lead verifies operation before the system returns to normal monitoring.

6.4 For an AI agent, re-enable only after its scope record is reconfirmed by the human owner and its integration has been re-provisioned under [PRO-IS-031](PRO-IS-031_Privileged-Access-Request-and-Review-Procedure.md). `[Beyond framework minimum]`

6.5 Keep enhanced monitoring on the account and affected systems for 30 days.

### 7. Communicate

7.1 **Internal:** IC posts status to the incident channel at declaration, at each severity change, at containment, and at closure. CISO is notified at declaration for High and Critical.

7.2 **Legal:** IC notifies duty counsel at declaration for any High or Critical incident. Legal runs the materiality assessment under PRO-LG-003. If Norsewind is a public company, a determination that the incident is material starts the four-business-day SEC Form 8-K clock. Do not make or imply materiality determinations in the incident channel; that is Legal's call.

7.3 **Customers:** Any customer whose designs, specifications, or equipment were reachable is notified by the account team, in the timeframe their supply agreement requires, using the template Legal approves. The IC supplies the facts; the account team owns the relationship.

7.4 **External parties:** Law enforcement, regulators, insurers, and vendors are contacted only on Legal's instruction.

### 8. Close

8.1 Closure criteria: attacker access removed, initial vector remediated, affected systems verified, evidence archived, and all notifications complete.

8.2 IC records the timeline, root cause, and actions in the ticket and closes it.

8.3 Schedule the post-incident review within 5 business days. Attendees: IC, SOC analyst, IAM engineer, system owners, facilities engineering lead if OT was involved, and the AI agent's owner if an agent was involved. Output: a blameless review document with action items, owners, and dates. Detection gaps go to Security Operations; control gaps go to the STD-IS-012 owner for the next revision.

8.4 Update this runbook if anything in it was wrong, missing, or slower than it needed to be. Reconfirm the AI-Era Assumptions.

## If something goes wrong

| Problem | Do this |
|---|---|
| Disabling the account would stop a critical process (service account) | Rotate the credential and update the dependent service instead of disabling; IC approves |
| The compromised account is the PAM platform administrator | Escalate to Critical. Use the break-glass procedure in STD-IS-012 to regain control; the facilities engineering lead and CISO are on the call |
| The account owner is unreachable | Proceed with containment. Do not wait for the owner |
| The person on the phone insists they are the owner and demands immediate restoration | Containment stands. Restoration follows step 6.1 only. Record the request and the caller's details in the ticket `[Beyond framework minimum]` |
| Containment on an OT system is blocked by the facilities engineering lead | The facilities engineering lead's call stands for physical safety. IC documents the risk acceptance and applies network isolation as the compensating control |
| Evidence export fails | Screenshot what you can, note the failure and time in the ticket, and continue containment. Evidence loss is preferable to continued attacker access |

## Evidence checklist

- [ ] Incident ticket with timestamped actions
- [ ] SIEM export for the account (30 days) with hash
- [ ] PAM session recordings for the suspicious window with hash
- [ ] AI agent action log and prompt history, if an agent was involved
- [ ] System snapshots, if taken
- [ ] List of systems and customer designs, specifications, or equipment reachable
- [ ] Notification records (internal, Legal, customer)
- [ ] Post-incident review document

## Control Mapping

| Framework | Reference |
|---|---|
| ISO/IEC 27001:2022 Annex A | A.5.24 through A.5.28 (incident management planning, assessment, response, learning, evidence); A.8.16 Monitoring activities |
| SOC 2 (TSC 2017) | CC7.3, CC7.4, CC7.5 |
| NIST CSF 2.0 | RS.MA-01, RS.AN, RS.CO-02, RS.MI-01, RS.MI-02, RC.RP-01 |
| NIST SP 800-61 Rev 3 | Incident response lifecycle |
| PCI DSS 4.0 | Requirement 12.10 |

Steps and sections tagged `[Beyond framework minimum]` are not required by any framework above. They follow from the AI-Era Assumptions section. NIST SP 800-61 Rev 3 encourages predefined playbooks but does not require pre-authorized containment actions.

## Revision History

| Version | Date | Author | Summary |
|---|---|---|---|
| 2.2 | 2026-11-01 | Security Operations | Added AI-Era Assumptions section; added pre-authorized actions; added failed second-channel verification as a trigger; added directory callback rule (1.1), AI agent handling (2.6, 6.4), and the "insists they are the owner" row; added deepfake and prompt injection to access vectors |
| 2.1 | 2026-10-01 | Security Operations | Added OT pre-containment check and facilities engineering lead role after August 2026 tabletop; added SEC materiality handoff in 7.2; added "If something goes wrong" table |
| 2.0 | 2025-11-15 | Security Operations | Restructured to numbered steps with verification points |
| 1.0 | 2024-06-01 | Security Operations | Initial release |

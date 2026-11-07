# Cybersecurity Governance Document Set (Sample)

A four-tier sample set showing how a policy, a standard, a procedure, and a runbook fit together in a security governance program. All four documents cover one domain, privileged access, so you can follow a single requirement from board-approved intent down to the steps an on-call analyst takes at 2 a.m.

The company, Norsewind Precision Manufacturing, Inc., is fictional. All content is original. Written by [Jorena Collins](https://github.com/RenaVerbena/RenaVerbena).

## The hierarchy

| Tier | Answers | Owner / approver | Changes when |
|---|---|---|---|
| **Policy** | What must be true, and why | CISO owns, executive approves | Business or regulatory intent changes (rarely) |
| **Standard** | The measurable parameters that satisfy the policy | CISO owns and approves | Technology or risk appetite changes |
| **Procedure** | Who does what, in what order, to meet the standard | Team lead owns, CISO or delegate approves | Tools or roles change |
| **Runbook** | What to do when a specific condition occurs | Security Operations owns | After every exercise or real incident |

A tier should keep to its own lane as much as possible. A policy says what must be true and leaves the steps to procedures. A standard defines the measurable parameters so the policy doesn't have to change when a number does. A procedure lists the steps and details required to implement the controls of the standard. Real programs drift from these boundaries, and that's fine, but the closer a tier keeps to its lane, the easier it is to change a review interval without re-approving the policy.

## Files

| Document ID | Title | Tier | Classification |
|---|---|---|---|
| POL-IS-004 | [Access Control Policy](POL-IS-004_Access-Control-Policy.md) | Policy | Internal |
| STD-IS-012 | [Privileged Access Management Standard](STD-IS-012_Privileged-Access-Management-Standard.md) | Standard | Internal |
| PRO-IS-031 | [Privileged Access Request, Approval, and Review Procedure](PRO-IS-031_Privileged-Access-Request-and-Review-Procedure.md) | Procedure | Internal |
| RB-IR-007 | [Runbook: Suspected Compromise of a Privileged Account](RB-IR-007_Suspected-Privileged-Account-Compromise-Runbook.md) | Runbook | Confidential |

## Finding related documents

Every document lists its governing and related documents in the metadata block, by ID and title. Links follow one rule: a document links to anything at the same or lower classification, and refers to anything more sensitive by ID only.

In this set, the policy, standard, and procedure are Internal and link to each other. The runbook is Confidential because it describes detection logic, pre-authorized actions, and escalation paths. The runbook links up to the standard and the procedure. The procedure refers to the runbook by ID. Someone with access finds the runbook by its ID in the runbook repository. Someone without access learns that the runbook exists and who owns it. Knowing the owner is enough to request access. IDs without links refer to documents outside this sample set or at a higher classification.

Many programs also keep a document register: one index listing every document with its owner, classification, and location. The Files table above is a small document register.

## Conventions in this set

- **Document ID:** `TYPE-DOMAIN-NNN`. Types are POL, STD, PRO, RB, and GDL (guideline). Domains here are IS (information security) and IR (incident response). IDs are deliberately neutral; framework references live in each document's Control Mapping section rather than in the ID.
- **Metadata block** at the top: ID, version, owner, approver, effective date, review cycle, classification, and related documents. Auditors look here first.
- **Summary block** immediately after the metadata: what the document does, who it applies to, and the two or three requirements that matter most. Written so a person or a search tool can decide relevance without reading the whole document.
- **AI-era assumptions block** immediately after the summary: the four threat assumptions the document is written against. The block states that these assumptions are the organization's own and go beyond the minimum requirements of the mapped frameworks, so a reader can tell at a glance which requirements exceed what any framework mandates.
- **The tag `[Beyond framework minimum]`** marks an individual requirement or step that follows from those assumptions rather than from a mapped control. Everything untagged should trace to a control in the Control Mapping section.
- **Requirement language:** *must* and *shall* are mandatory, *should* is recommended and needs a documented reason to skip, *may* is optional.
- **Control mapping** at the end of each document lists the framework references the document supports. Many programs keep this mapping in a separate register or GRC tool instead, and that approach works. Keeping the mapping in the document puts the evidence next to the requirement. The mappings here are illustrative. In a real program they are verified against the organization's Statement of Applicability and control library.
- **Revision history** as the final section.

## Adapting the set

Replace the company name, adjust parameters in the standard to match the organization's risk appetite, and swap the generic system names (PAM platform, ITSM system, SIEM) for the real tools. The structure, section order, and language conventions are meant to carry over unchanged.

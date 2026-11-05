# Vehicle Maintenance Tracker — Plan

> **When:** Week 3 (Sections 1–2), Week 4 (Sections 3–4)  
> **Owner:** DeLorean Wiggins — Architecture Lead, Plan  
> **Version:** 0.7 working proposal — November 4, 2026  
> **Status:** Updated for the merged Specification; architecture approval pending.  
> **Approved on:** Pending  
> **Approved by:** DeLorean Wiggins — pending; Nasif Rahman — pending; Michal Broniek — pending; Kaleb Edwards — pending.  
> **Done when:** every item in the SVC Self-Check is true.  
> **Filled-in Example:** `docs/examples/plan-example.md`

**Basis:** Plan PR #2 and Specification PR #3 are merged. This revision follows Specification v1.0 (R1–R31) and the current Style Guide: one car per driver, phone/password account access, P1 scenarios S1 and S2, and no VIN or plate lookup. Q2 and Q3 are recorded as resolved; Q1 remains open with Nasif. A merge does not record final architecture approval.

**Planning status:** Requirement IDs and priorities now come from the merged Specification. Architecture choices remain proposals. The database, Q1 rules, simulation boundaries and genuine prototype findings still need agreement or evidence. Sections 3–4 remain a preliminary Week 4 inventory.

## 1. Approach

The phone-friendly app lets a driver create an account, save one car, see upcoming maintenance with plain-language explanations, and record completed work. The proposed course build demonstrates these journeys with fabricated records and clearly labeled simulated account access and storage. A shared data service follows Nasif's agreed contracts so a later real backend can replace the simulation. The real database, maintenance rules and exact simulation of the specified phone/password flow still need team agreement.

## 2. Key Decisions (ADRs)

| ADR | Decision | Req IDs | Alternatives considered | Why this one |
|---|---|---|---|---|
| ADR-00 | **Proposed course stack:** Vue, no build step; `collection-template` starter; simulated backend with JSON seed data and browser storage; GitHub Pages from the repository root. **Real database/provider designed for:** pending agreement between DeLorean and Nasif; copy the agreed choice into Data & API Section 0. | R1, R4–R15, R24–R28; later R16–R23, R29–R31 | Real backend now; frontend build toolchain | Uses course defaults for an end-to-end demonstration with little deployment setup. Local storage cannot establish shared persistence or real account security. Docker is not required by this proposed stack. |
| ADR-03 | **Proposed:** put maintenance calculation behind a shared data-service operation, using Nasif's agreed inputs and outputs. Sources, supported vehicles, baselines and date/mileage ordering remain open under Q1. | R4, R5, R8, R10; later R17 | Rules in each screen; screens calling an external schedule provider directly | One contract keeps schedule and completion behavior consistent and allows backend changes without rewriting screens. Both architecture owners must maintain that contract. |
| ADR-04 | **Superseded proposal — replaced by ADR-05:** a simulated user picker alone was proposed before Q3 was resolved; it is no longer the planned product sign-in flow. | R13, R24–R28 | Real authentication during the course | Retained to explain the change. A picker alone omits the phone/password journey now required by S1 and R24–R28. |
| ADR-05 | **Proposed, pending Michal and Nasif's review:** demonstrate the specified phone/password forms through a simulated account service using fabricated identities and designated demo credentials. Label account access/storage as simulated; a future real backend must authenticate drivers and enforce ownership. | R13, R24–R28; later R29–R31 | Picker-only interface; real authentication and SMS now | Exercises the specified account journey and validation without collecting private credentials. It adds simulation work and cannot prove security. No real passwords, phone numbers or SMS sending belong in this proposed demo; Michal must confirm the affected requirements' simulation boundaries. |
| ADR-06 | **Proposed:** validate a completion, save its history entry and return the recalculated schedule through one data-service operation. Screens must not save history and change the schedule independently. | R9–R12 | Separate screen-driven writes | Keeps a rejected completion from partly changing the record or schedule. Nasif must define the failure response and later transaction design; Michal must clarify the mileage baseline first. |

The Specification owns product choices, including one car per driver, phone/password access, privacy and no commercial promotion. These ADRs support those requirements; they do not reopen Q3.

**Week 3 decisions needed before approval:**

- **DeLorean and Nasif:** agree the real database/provider and make ADR-00 match Data & API Section 0. That file on `main` remains a template at this revision; no entity or endpoint contract is assumed.
- **Nasif, with Michal:** resolve Q1's recommendation source, supported vehicles, required baselines, missing history, units, date/mileage ordering and how completion sets the next interval (R4, R5, R8, R10).
- **Michal — cross-review ambiguity:** R9 saves a completion and R12 compares entered mileage with the car's “last recorded mileage,” but neither states whether saving a completion updates that baseline. If a car shows 60,000 and a service is recorded at 60,500, is the car now at 60,500? Clarify this before Nasif defines the operation and ADR-06 is settled. Also confirm whether historical service below current mileage is intentionally rejected.
- **Michal and Nasif:** confirm the demonstration boundary for R13 and R24–R31. Q3 selects the product login method; it does not establish real authentication/SMS in a static demo. Record approved simulations against affected requirement IDs.

When an approved decision changes, retain its ADR and add a replacement. No approval is inferred from a merge.

## 3. Screens & Build Order

Preliminary Week 4 inventory, to be refined from actual Week 3 prototype observations. Routes are proposed browser hash routes, not API paths. Apply the current Style Guide. R13 covers every driver-data operation; R14/R15 cover all screens/forms.

| # | Screen or part | Route | Shows / does | Req IDs | Priority |
|---|---|---|---|---|---|
| 1 | Minimum simulated data service | Not a screen | Supplies fabricated account, one-car and schedule records. Follow Nasif's contracts once agreed; extend alongside each journey. | R1, R4–R13, R24–R28 | P1 foundation |
| 2 | My car and upcoming maintenance | Proposed `#/car` | Shows one car, ordered tasks, due basis and the unsupported-car state. Start with a fabricated fixture, then connect the full account/setup journey. | R4, R5, R8, R13–R15 | P1 |
| 3 | Account creation and sign-in | Proposed `#/sign-up`, `#/sign-in` | Phone/password forms, specified validation messages and driver-scoped demo records; clearly show the agreed simulation boundary. | R13–R15, R24–R28 | P1 |
| 4 | Car setup | Proposed `#/car/setup` | Collects only make, model, year and mileage; names missing fields; saves one car and reaches its schedule or unsupported state. | R1–R5, R8, R13–R15 | P1 |
| 5 | Maintenance detail | Proposed `#/maintenance/:taskId` | Explains the task, why it matters and its due basis; shows the exact general-guidance notice. Completes S1 with the earlier parts. | R5–R7, R13–R15 | P1 |
| 6 | Mark maintenance done | Proposed `#/maintenance/:taskId/complete` | Validates date/mileage, saves completion and refreshes the next due value together. Shows the last completion after reload without requiring the later full history screen. Completes S2. | R9–R15 | P1 |
| 7 | Update mileage | Proposed `#/car/mileage` | Saves valid mileage and refreshes overdue status; reuses the shared rejection rule for lower readings. | R12–R17 | P2; shared P1 rules remain |
| 8 | Full maintenance history | Proposed `#/history` | Lists all completions newest first or the specified empty state. P1 completion storage already exists. | R9, R13–R15, R18, R19 | P2 |
| 9 | Password reset | Proposed `#/reset-password` | Demonstrates request, code, expiry and password-change states with fabricated data if simulation is agreed. Real SMS is not assumed. | R13–R15, R27–R31 | P2; shared P1 rules remain |
| 10 | Reminder preferences and delivery | Proposed `#/reminders`; delivery is not a screen | Validates a reminder email and demonstrates the agreed due-reminder behavior. Delivery/simulation needs agreement before implementation. | R20–R22 | P3 |

**Why this order:** Start with the smallest browse slice so data is visible, then finish S1's account-to-setup-to-schedule journey before S2. Complete all P1 work before P2 and P3. R23 is a P2 performance check on the upcoming list; Michal should define “home connection” before treating its three-second target as a repeatable acceptance check.

**Prototype handoff:** Mike shared a throwaway HTML prototype in Slack. DeLorean still needs actual observations from a target-like user; no completed session or outcome is claimed here. Give findings to Michal for the Spec's Evidence table, then carry useful screen findings into this inventory. Do not commit the throwaway prototype, copy its code into the app or treat its behavior as an approved requirement.

## 4. Risks & Assumptions

Ratings are preliminary. The decisions above are present blockers; these risks concern what could go wrong after choices are made.

| Risk or assumption | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Recommendations may not fit a car or may be mistaken for manufacturer advice. | Medium | High | Nasif and Michal limit supported coverage, check sources and use R8's unsupported state. Resolve Q1 before claiming the calculation is complete. |
| Missing history, unclear mileage baselines or mixed date/mileage rules may mislead. | Medium | High | Michal resolves R9/R12; Nasif specifies units, baselines and boundaries. Check concrete completion and missing-data examples. |
| Simulated login may be mistaken for real privacy protection. | Medium | High | Use only fabricated identities/demo credentials; label affected requirements as simulated. DeLorean and Nasif design real backend ownership checks for later. |
| Browser storage may disappear or be mistaken for cross-device synchronization. | Medium | Medium | Agree persistence limits and save/reset behavior. Make no synchronization promise. |
| Real SMS, email or recovery may expand course scope. | Medium | High | Keep reset at P2 and reminders at P3. Agree delivery/simulation and external dependencies before building them. |
| Sourcing schedules and explanations may exceed the proposed 20 team hours. | Medium | High | Treat the Business Case estimate as unvalidated; narrow coverage and estimate the agreed P1 work. |
| Drivers may struggle with signup, misunderstand guidance or expect P3 reminders. | Medium | High | Observe real prototype users without coaching. Give Michal the findings for deliberate Spec refinement; do not invent outcomes. |
| Plan and Data & API Design may drift. | Medium | High | DeLorean and Nasif cross-review the database, names, ownership, rules and operations; fix source documents before tasks/code. |

## SVC Self-Check

- [ ] **Simple** — the approach reads in under a minute; each ADR states one decision.
- [ ] **Valuable** — every ADR and screen has a requirement ID; every “Why this one” names a real trade-off.
- [ ] **Complete** — ADR-00 names the full stack, starter skill and database; every P1 requirement is covered by a screen or part; P1 comes first, one scenario end to end at a time; every risk has a mitigation; the team has approved it before Tasks are written.

**Approval:** DeLorean Wiggins, Nasif Rahman, Michal Broniek and Kaleb Edwards must review the completed Plan before approval or Tasks. Database selection, Q1, the mileage clarification, simulation scope and prototype findings remain pending. This revision does not mark Week 3 or Week 4 complete.


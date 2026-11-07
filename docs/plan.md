# Vehicle Maintenance Tracker — Plan

> **When:** Week 3 (Sections 1–2), Week 4 (Sections 3–4)  
> **Owner:** DeLorean Wiggins — Architecture Lead, Plan  
> **Version:** 0.9 review draft — November 7, 2026  
> **Status:** Sections 1–4 updated for Specification PR #5 and Data & API Design v1.0; team decisions and architecture approval pending.  
> **Approved on:** Pending  
> **Approved by:** DeLorean Wiggins — pending; Nasif Rahman — pending; Michal Broniek — pending; Kaleb Edwards — pending.  
> **Done when:** every item in the SVC Self-Check is true.  
> **Filled-in Example:** `docs/examples/plan-example.md`

**Basis:** Specification v1.0 after merged PR #5 (R1–R34), Data & API Design v1.0 Sections 0–2, and the current Style Guide. The product supports one car per driver and phone/password account access. Q1–Q3 are recorded as resolved. Q4 still marks historical maintenance as an assumption awaiting the whole team's answer. This Plan follows that assumption for review; a merge does not establish approval.

## 1. Approach

The phone-friendly app lets a driver save one car, understand its upcoming maintenance, and record completed work. The course build uses made-up records and clearly labeled simulated accounts and storage. A shared data service follows Nasif's design for PostgreSQL via Supabase, so a later real backend can replace the simulation while keeping the same data and actions.

## 2. Key Decisions (ADRs)

These are architecture proposals for team review unless a source decision is explicitly identified. Product requirements remain in the Specification.

| ADR | Decision | Req IDs | Alternatives considered | Why this one |
|---|---|---|---|---|
| ADR-00 | **Course stack:** Vue, no build step; `collection-template` starter; simulated backend with JSON seed data and browser storage; GitHub Pages from the repository root. **Real database designed for:** PostgreSQL via Supabase, matching Nasif's Data & API Section 0. | R1, R4–R15, R24–R28, R32–R34; later R16–R23, R29–R31 | Real backend during this course; frontend build toolchain | Keeps the course build focused on validating the documents while preserving the relational design for implementation later. Browser storage gives up real authentication and cross-device persistence; this build does not call Supabase. |
| ADR-03 | Put maintenance calculation in the shared data service. Follow resolved Q1: curated manufacturer-sourced intervals for supported make/model/year combinations; R8 for unsupported cars. Use `Car.initial_mileage + MaintenanceSchedule.interval_miles` until a task has a completion, then the most recent completion's mileage plus its interval. | R4, R5, R8, R10; later R17 | Rules duplicated in screens; live outside schedule API | One rule keeps the upcoming list and completion result consistent without an external API dependency. Curated coverage needs source checking and remains limited; using setup mileage assumes fresh service when history is missing. |
| ADR-04 | **Superseded proposal — replaced by ADR-05:** a simulated user picker alone was proposed before Q3 was resolved; it is no longer the planned product sign-in flow. | R13, R24–R28 | Real authentication during the course | Retained as decision history. A picker alone omits the phone/password journey now required by S1 and R24–R28. |
| ADR-05 | **Pending Michal and Nasif's review:** use the specified phone/password forms with a simulated account service, fabricated identities and designated demo credentials. Label the simulation. Follow Data & API Section 0's proposed on-screen reset code and in-app reminder log for later P2/P3 work. | R13, R24–R28; later R20–R22, R29–R31 | Picker-only interface; real authentication, SMS and email now | Exercises the specified journeys without collecting real credentials or relying on messaging providers. It requires more work than a picker and cannot establish real privacy or delivery; Michal must approve the affected simulation boundaries. |
| ADR-06 | Validate and save a completion through one data-service operation: add its history record, advance `Car.mileage` only if higher (R34), and derive the next due point. Show the R32 confirmation only after success. A rejected or failed save leaves all persisted records unchanged. | R9–R12, R32, R34 | Separate writes from each screen; a second mileage prompt | Prevents a history entry, odometer and success message from disagreeing. The simulation needs an all-or-nothing persistence boundary; the later real backend needs equivalent transaction behavior. |
| ADR-07 | **ASSUMPTION — follows open Q4:** accept past-dated, lower-mileage completions under R33. Calculate each task's due point from its most recent service, not the record entered last; retain `Car.initial_mileage` and never reduce current `Car.mileage`. | R9–R12, R33, R34; later R18 | Reject all lower readings; let the last-entered record set the schedule | Lets drivers record earlier work without undoing newer service or lowering the odometer. Historical ordering needs explicit rules in Nasif's contracts; if the team rejects Q4, Michal revises the Spec and this ADR is replaced before implementation. |

**Data handoff:** use the existing `Driver`, `Car`, `MaintenanceSchedule`, `MaintenanceCompletion` and `PasswordReset` names and fields from Data & API Design. Capture `Car.initial_mileage` from the mileage entered during setup; do not add a fifth car-setup field (R2). Due values are calculated, not stored as a second source of truth. `data.js` is the proposed boundary between screens and the simulation; its operations and response shapes must match Nasif's completed contracts. API paths are not invented here while his Section 4 is unfinished.

**Review items before approval:**

- **Michal, DeLorean, Nasif and Kaleb:** answer Q4 and record the decision in the Spec. Mike favors historical maintenance and Kaleb agreed in Slack; this is not a record of all four approvals.
- **Nasif, with DeLorean's cross-review:** finish Data & API Sections 3–4. Its current failure paragraph rejects every lower-mileage completion, which conflicts with the newer R12/R33 distinction. It also needs to use the most recent service rather than the newest inserted record. Confirm request/response shapes, all-or-nothing save behavior, service ordering for same-date records, and the date used to determine “today.”
- **Michal and Nasif:** Q1's source and baseline choice is resolved. The remaining implementation details are the supported vehicles, actual manufacturer references, and the boundary between “due now” and “overdue”: Data & API currently uses `>=`, while R17's wording says mileage “passes” the due point. Agree these before claiming the calculation's acceptance checks pass.
- **Michal and Kaleb:** decide how to explain the setup-baseline assumption when no past service is known. Keep R7's required general-guidance notice. Kaleb's Style Guide also needs its blanket lower-mileage rejection aligned with R12/R33 and the R32 saved message included.
- **Michal and Nasif:** confirm the simulated account, password-reset and reminder behavior against R13, R20–R22 and R24–R31. A proposed simulation is not proof of real authentication, SMS or email delivery.

R34 now records the automatic mileage change raised in DeLorean's cross-review; the earlier request to extend R9's test is no longer pending. Keep existing ADR history when replacing a decision.

## 3. Screens & Build Order

This is the Week 4 inventory for review. Routes are proposed browser hash routes, not API endpoints. Apply Kaleb's Style Guide throughout: visible labels and errors, keyboard focus, retained invalid input, and layouts that fit 320 px. R13 applies to every driver-data read and write, including navigation and switching between demo accounts.

| # | Screen or part | Route | Shows / does | Req IDs | Priority |
|---|---|---|---|---|---|
| 1 | Shared data service and persistence | Not a screen | Seeds fabricated records, scopes data to the active demo driver, and saves/reloads the one car and its completions. Add only the operations needed for each journey; use Nasif's fields and completed contracts. Keep initial mileage fixed and derive due values centrally. | R1, R4–R13, R24–R28, R32–R34 | P1 foundation |
| 2 | My car and upcoming maintenance | `#/car` | Shows one car, soonest-due tasks, their due mileage, and R8's exact unsupported-car message. Start with a fabricated fixture, then connect the account/setup journey. Keep the current page and next action visible. | R4, R5, R8, R13–R15 | P1 |
| 3 | Account creation and sign-in | `#/sign-up`, `#/sign-in` | Shows phone/password forms and R26–R28 validation messages. Demonstrates sign-out and returning sign-in with fabricated credentials. Shows no other driver's car or history and labels the agreed simulation. | R13–R15, R24–R28 | P1 |
| 4 | Car setup | `#/car/setup` | Collects only make, model, year and mileage; shows R3's missing-field message; saves one car and its initial mileage; reaches the schedule or unsupported state. | R1–R5, R8, R13–R15 | P1 |
| 5 | Maintenance detail | `#/maintenance/:taskId` | Shows task name, due point, what it is, why it matters and the exact R7 guidance notice. Provides the completion action. With the earlier parts, completes S1. | R5–R7, R13–R15 | P1 |
| 6 | Mark maintenance done and saved result | `#/maintenance/:taskId/complete` | Accepts a completion date and mileage. Rejects future dates and lower mileage for today with R11/R12 messages; accepts past lower readings under R33/Q4. Saves through ADR-06, preserves current mileage for older lower readings, and updates it for higher readings. Displays “Saved. Next [task] due at [next due mileage or date].” from the resulting schedule, plus the saved completion details. Reload retains the record and current mileage. Completes S2 without requiring the P2 full-history screen. | R9–R15, R32–R34 | P1 |
| 7 | Update mileage | `#/car/mileage` | Saves a non-decreasing current reading; shows the R12 error for a reduction. Refreshes due/overdue status without moving an uncompleted task's baseline. | R12–R17 | P2; shared P1 rules remain |
| 8 | Full maintenance history | `#/history` | Lists completed tasks with their service dates and mileage, newest service first, or R19's exact empty-state message. Does not reorder the maintenance baseline by entry time. | R9, R13–R15, R18, R19 | P2 |
| 9 | Password reset | `#/reset-password` | Shows request, code entry, expiry/error and new-password states; enforces the 10-minute validity and single-use behavior in Nasif's design. Uses the proposed on-screen code only if Michal approves the simulation. | R13–R15, R27–R31 | P2 |
| 10 | Upcoming-list loading and performance | Part of `#/car` and the data service | Loads the list and measures R23 against its 10-task fixture. Michal must define the “home connection” test conditions before the three-second result is repeatable. | R4, R23 | P2; R4 already built at P1 |
| 11 | Reminder preferences | `#/reminders` | Collects an email only when reminders are enabled; validates it and keeps reminders off for invalid input. | R13–R15, R20, R21 | P3 |
| 12 | Reminder delivery | Not a screen; visible result in the reminder view | Uses the due calculation and preferences to produce the R22 task reminder. Follow the proposed in-app notification log only after Michal confirms that simulation; do not imply a real email was sent. | R22 | P3 |

**Why this order:** Begin with the smallest browse view, then finish S1 from account creation through car setup and task explanation before starting S2's completion path. Complete all P1 behavior, including R32–R34, before P2 or P3; keep each screen connected to its data as it is added.

**Changes from prototype feedback:**

| Evidence in the merged Spec | Effect on this Plan |
|---|---|
| E7–E8: next service named without a due point, and uncertainty about saving | Keep the next due point visible on the list/detail screens and show the R32 saved result after persistence succeeds. Do not use a success message before the save completes. |
| E9: remembering the screen and what to click was difficult | Keep current-page navigation, labels and the main action visible; preserve entered values when validation fails. Kaleb reviews the layout against the Style Guide. |
| E10: tester expected email reminders | Keep preferences and delivery explicit in the inventory, at their existing P3 priority. This feedback does not approve real email integration. |
| E11: tester wanted to enter past maintenance | Include the R33 path in the P1 completion screen, subject to Q4; an older record cannot replace a newer service baseline. |

E7–E10 came from a tester's solo, self-reported feedback shared by DeLorean; they are not a watched session or completed acceptance tests. E11 was reported by Michal. Earlier source inspection of the throwaway prototype found a moving due threshold when current mileage changed; Q1's fixed initial mileage addresses that design issue. The prototype itself remains disposable and is not copied into the app.

**Review cases for the later build, not executed tests:**

- With an explicitly fabricated 5,000-mile interval, setup at 60,000 gives a due point of 65,000. Raising current mileage to 66,000 must leave that due point at 65,000.
- A valid completion today at 60,500 on a car showing 60,000 saves both history and current mileage; reload retains both and R32 reports the next due point (R9, R10, R32, R34).
- With a newer completion already present, adding an older, lower-mileage service adds history but neither lowers the odometer nor changes the newer service's due baseline (R10, R33, R34; Q4 pending).
- A future date or a lower reading entered for today leaves history and both mileage fields unchanged and shows the specified error, with no saved confirmation (R11, R12, R32).

The sample interval above is a test fixture, not a verified recommendation. Nasif and Michal still select and cite the real schedule seed data.

## 4. Risks & Assumptions

These ratings are proposals. The concrete review items in Section 2 are current dependencies; the table covers what could go wrong and how to limit it.

| Risk or assumption | Likelihood | Impact | Mitigation |
|---|---|---|---|
| A curated interval may not fit a particular car or be mistaken for specific advice. | Medium | High | Nasif and Michal record manufacturer sources and supported vehicles, retain R7's notice, and use R8 for unsupported cars. Do not invent schedule values. |
| No history means treating every task as freshly serviced at setup, potentially hiding overdue work. | High | High | Keep the initial baseline fixed; Michal and Kaleb agree clear wording for the assumption. Allow real past-service entries if Q4 is accepted, without suggesting that unknown history proves recent service. |
| A historical entry could replace a newer baseline or reduce current mileage. | Medium | High | Apply ADR-07 and R10/R33/R34 together; Nasif defines service ordering and same-date handling. Check the older-entry case before accepting the build. |
| A partial save could show success while history or mileage was not stored. | Medium | High | Keep validation, persistence and the result in one operation; Nasif defines its errors. Display R32 only after success and check reload plus rejected-save cases. |
| Simulated accounts may be mistaken for real privacy protection. | Medium | High | Use fabricated identities/demo credentials and label the simulation. Design real backend ownership checks for implementation later; never use client storage as proof of secure authentication. |
| Browser storage may be cleared or mistaken for cross-device synchronization. | Medium | Medium | Document local persistence and reset behavior with Nasif; provide repeatable fabricated fixtures and make no synchronization promise. |
| Password recovery and reminders could introduce unplanned provider work. | Medium | High | Keep recovery at P2 and reminders at P3. Michal confirms the proposed on-screen/log simulations before implementation; real providers remain outside the proposed course build. |
| Two prototype testers, including solo feedback, may miss important usability problems. | High | Medium | Treat E7–E11 as early findings. Michal and DeLorean use actual observed follow-up when available and record assistance and uncertainty; never count a repeat as another person. |
| The proposed 20 team hours may be insufficient for the agreed scope. | Medium | High | Kaleb estimates tasks after document approval; finish P1 before optional work and narrow supported schedule coverage with Michal and Nasif. |
| Plan, Spec, Style Guide and Data & API may drift as contracts are completed. | Medium | High | DeLorean cross-reviews with Nasif, Michal and Kaleb; resolve Section 2's named mismatches, then check requirement IDs, fields and responses before Tasks or code. |

## SVC Self-Check

- [ ] **Simple** — the approach reads in under a minute; each ADR states one decision.
- [ ] **Valuable** — every ADR and screen has a requirement ID; every “Why this one” names a real trade-off.
- [ ] **Complete** — ADR-00 names the full stack, starter skill and database; every P1 requirement is covered by a screen or part; P1 comes first, one scenario end to end at a time; every risk has a mitigation; the team has approved it before Tasks are written.

**Review status:** Sections 1–4 are filled in for review. The screen/part inventory covers all 23 current P1 IDs: R1–R15, R24–R28 and R32–R34. Final completion still depends on the Q4 answer, simulation confirmation, Nasif's finished contracts, the named cross-document fixes, and review by DeLorean Wiggins, Nasif Rahman, Michal Broniek and Kaleb Edwards. No team approval or acceptance-test result is recorded by this draft.

# Vehicle Maintenance Tracker — Tasks

**When:** Week 5, then updated through Weeks 6–8  
**Owner:** Delivery Lead (Kaleb Edwards)  
**Done when:** Every item in the SVC Self-Check is true.

**Approved on:** Pending  
**Approved by:** Delivery Lead — Kaleb Edwards · Architecture Leads — DeLorean Wiggins and Nasif Rahman (pending) · Product Owner — Michal Broniek (pending)

**Filled-in Example:** `docs/examples/tasks-example.md`

Tasks break the Plan into small steps that can be built and checked one at a time.

---

## 1. Task List

One row per task, P1 first, following the Plan's build order. Each task should take less than one day. Every task's Done when check identifies where to verify the result.

| ID | Task | Priority | Traces to (R# / ADR) | Depends on | Done when | Status |
|---|---|---|---|---|---|---|
| T0 | Scaffold the app using the collection-template starter named in ADR-00. | P1 | ADR-00 | — | The starter template's sample pages open in the browser and the project runs from the repository root. | Not started |
| T1 | Create the shared simulated data service using JSON seed data and browser storage. | P1 | R1, R4–R13, R24–R28 / ADR-00, ADR-03 | T0 | The data service loads the sample driver, car and maintenance schedule records in the browser console. | Not started |
| T2 | Add simulated account creation with phone number and password validation. | P1 | R24, R27, R28 / ADR-05 | T1 | Signing up with a valid 10-digit phone number and an 8-character password creates a new account; invalid inputs show the specified error messages on the sign-up screen. | Not started |
| T3 | Add simulated sign-in and driver-specific data access. | P1 | R13, R25, R26 / ADR-05 | T2 | Signing in with the correct demo credentials opens the driver's account, while an incorrect password shows the specified error and does not display another driver's car. | Not started |
| T4 | Build the car setup form with the four required fields. | P1 | R1, R2, R3 / ADR-00, Screen 4 | T3 | Entering Honda, Civic, 2015 and 60,000 saves the car; leaving mileage blank displays "Please enter your car's mileage." | Not started |
| T5 | Implement the supported-vehicle maintenance schedule and Q1 calculation rules. | P1 | R4, R5, R8, R10 / ADR-03 | T1 | A supported sample car displays its maintenance tasks in soonest-due order with due mileage or date, while an unsupported car displays the exact R8 message. | Not started |
| T6 | Connect the car dashboard to the saved car and upcoming maintenance service. | P1 | R4, R5, R8, R13, R14 / ADR-03, Screen 2 | T4, T5 | The My Car screen displays the signed-in driver's car and upcoming tasks in the correct order without horizontal scrolling at 320px. | Not started |
| T7 | Build the maintenance task detail screen with plain-language explanations. | P1 | R6, R7 / Screen 5 | T6 | Opening a maintenance task shows what it is, why it matters and the exact message "General guidance. Check your owner's manual or ask a mechanic." | Not started |
| T8 | Build the mark-maintenance-done form with date and mileage validation. | P1 | R9, R11, R12, R33 / ADR-06, Screen 6 | T7 | A valid completion is saved with its date and mileage; a future date or invalid same-day mileage shows the specified error and saves nothing. | Not started |
| T9 | Implement automatic car mileage updates and next-due recalculation after a completion. | P1 | R10, R32, R34 / ADR-03, ADR-06 | T8 | Saving an oil change at 60,500 when the car shows 60,000 updates the car to 60,500, displays the R32 success message and calculates the next due mileage from 60,500 plus the interval. | Not started |
| T10 | Add clear completion feedback and confirm saved data persists after reload. | P1 | R9, R10, R32, R34 / ADR-06, Screen 6 | T9 | After reloading the browser, the completed task, updated car mileage and next due value remain visible. | Not started |
| T11 | Check responsive layouts, form labels and keyboard accessibility across P1 screens. | P1 | R14, R15 / Screen 2–6 | T6, T7, T8 | Every P1 screen works at 320px without horizontal scrolling, every form field has a visible label, and all interactive controls can be used with a keyboard. | Not started |
| T12 | Implement the update-mileage form and overdue calculation. | P2 | R16, R17 / Screen 7 | T10 | Updating a car from 60,500 to 61,200 saves the new mileage and marks a task due at 61,000 as overdue. | Not started |
| T13 | Build the maintenance history screen. | P2 | R18, R19 / Screen 8 | T10 | The history screen displays all completed tasks newest first with their dates and mileages, or "No maintenance recorded yet." when empty. | Not started |
| T14 | Build the simulated password reset flow. | P2 | R27, R29, R30, R31 / ADR-05, Screen 9 | T3 | Requesting a reset displays a fabricated reset code; a valid unused code changes the password, while an incorrect, used or expired code displays the specified error. | Not started |
| T15 | Check upcoming maintenance list performance. | P2 | R23 / Screen 2 | T6 | The upcoming maintenance list for a car with 10 tasks loads within 3 seconds on the agreed home connection. | Not started |
| T16 | Build reminder preferences and email validation. | P3 | R20, R21 / Screen 10 | T3 | Turning on reminders without an email asks for one, and an invalid email displays "Please enter a valid email address." without enabling reminders. | Not started |
| T17 | Implement simulated reminder delivery and notification logging. | P3 | R22 / Screen 10 | T12, T16 | Updating mileage past a task's due threshold with reminders enabled creates a visible notification-log entry naming the task instead of sending a real email. | Not started |
| T18 | Run the final acceptance checks against the Specification. | P1 | R1–R15, R24–R28 / ADR-00, ADR-03, ADR-05, ADR-06 | T2–T11 | All P1 acceptance tests pass, and the results are recorded against the relevant requirement IDs. | Not started |

**Status:** Not started · In progress · Done · Blocked (Q#)

**Note:** T14 and T17 use fabricated data and simulated behavior only, pending approval of the simulation boundaries. No real SMS or email provider is called during the course build. 

---

## 2. Cross-Check (Week 5)

Every P1 requirement must be traceable through the Specification, Data & API Design, Plan and Task List. An empty cell indicates a gap that must be fixed before Week 6.

| Req ID | Data & API (entity / endpoint) | Plan (ADR / screen #) | Task(s) |
|---|---|---|---|
| R1 | Car / `POST /cars` | ADR-00 / Screen 4 | T1, T4 |
| R2 | Car / `POST /cars` | ADR-00 / Screen 4 | T4 |
| R3 | Car / `POST /cars` | ADR-00 / Screen 4 | T4 |
| R4 | MaintenanceSchedule / `GET /cars/:id/upcoming-maintenance` | ADR-03 / Screen 2 | T5, T6 |
| R5 | MaintenanceSchedule / `GET /cars/:id/upcoming-maintenance` | ADR-03 / Screen 2 | T5, T6 |
| R6 | MaintenanceSchedule / `GET /maintenance-tasks/:id` | ADR-03 / Screen 5 | T7 |
| R7 | MaintenanceSchedule / `GET /maintenance-tasks/:id` | ADR-03 / Screen 5 | T7 |
| R8 | MaintenanceSchedule / `GET /cars/:id/upcoming-maintenance` | ADR-03 / Screen 2, Screen 4 | T5, T6 |
| R9 | MaintenanceCompletion / `POST /cars/:id/completions` | ADR-06 / Screen 6 | T8, T9, T10 |
| R10 | MaintenanceCompletion / `POST /cars/:id/completions` | ADR-03, ADR-06 / Screen 6 | T5, T9, T10 |
| R11 | MaintenanceCompletion / `POST /cars/:id/completions` | ADR-06 / Screen 6 | T8 |
| R12 | MaintenanceCompletion, Car / `POST /cars/:id/completions` | ADR-06 / Screen 6 | T8 |
| R13 | Driver, Car, MaintenanceCompletion / All Member endpoints | ADR-05 / Screens 2–6 | T3, T6, T18 |
| R14 | All entities and endpoints that supply P1 screens | ADR-00 / Screens 2–6 | T6, T7, T8, T11 |
| R15 | Driver, Car, MaintenanceCompletion / `POST /drivers`, `POST /cars`, `POST /cars/:id/completions` | ADR-05 / Screens 3, 4, 6 | T2, T4, T8, T11 |
| R24 | Driver / `POST /drivers` | ADR-05 / Screen 3 | T2 |
| R25 | Driver / `POST /sessions` | ADR-05 / Screen 3 | T3 |
| R26 | Driver / `POST /sessions` | ADR-05 / Screen 3 | T3 |
| R27 | Driver / `POST /drivers` | ADR-05 / Screen 3 | T2 |
| R28 | Driver / `POST /drivers` | ADR-05 / Screen 3 | T2 |
| R32 | MaintenanceCompletion / `POST /cars/:id/completions` | ADR-06 / Screen 6 | T9, T10 |
| R33 | MaintenanceCompletion / `POST /cars/:id/completions` | ADR-06 / Screen 6 | T8 |
| R34 | MaintenanceCompletion, Car / `POST /cars/:id/completions` | ADR-06 / Screen 6 | T9, T10 |

### Cross-Check Result

All P1 requirements in Specification v1.0 have been mapped to the corresponding data entities and endpoints, Plan ADRs or screens, and implementation tasks.

The following items still require attention before final approval:

1. **Database:** PostgreSQL via Supabase is specified in Data & API Design Section 0, but ADR-00 in Plan v0.8 still needs to match this decision.
2. **Simulation boundaries:** Michal and Nasif must approve the fabricated account, reset-code and notification behavior.
3. **Prototype evidence:** Actual target-user observations must be reviewed and incorporated where necessary.

**Result:** P1 requirements are traced across the documents. Final cross-check approval remains pending resolution of the outstanding decisions and document alignment.


### SVC Self-Check

### Simple
- [ ] Every task starts with a clear action verb.
- [ ] Every task can reasonably be completed in less than one day.
- [ ] Tasks follow the Plan's build order, with P1 before P2 and P3.
- [ ] Every task has a one-sentence Done when check.
- [ ] Every Done when check identifies where to verify the result.
- [ ] T0 is included as the initial scaffolding task.

### Valuable
- [ ] Every task traces to a requirement ID or ADR.
- [ ] Every P1 requirement has at least one task.
- [ ] Every task contributes to an agreed requirement or architecture decision.
- [ ] Tasks implement one complete user journey at a time.
- [ ] The cross-check connects every P1 requirement to the Data & API Design, Plan and Task List.

### Complete
- [ ] All P1 requirements have been cross-checked.
- [ ] No requirement is missing a corresponding task.
- [ ] No task is unrelated to a requirement or ADR.
- [ ] Dependencies are listed.
- [ ] Outstanding questions are recorded in the Specification.
- [ ] Blocked tasks reference their relevant Open Question.
- [ ] Simulated behaviors are clearly distinguished from real services.
- [ ] The Delivery Lead, Architecture Leads and Product Owner have reviewed and approved the task list.

**Approval status:** Draft — pending team review and approval before the Week 6 build.

---

## 3. Gap Log (Weeks 6–7)

*During the Week 6 AI build, log every place the agent guessed, got stuck, or misread a document. In Week 7, each document's owner fixes their gaps; then mark whether the re-run confirmed the fix.*

| What the agent got wrong or guessed | Which document & section caused it | Fix applied | Fixed in re-run? |
|-------------------------------------|------------------------------------|-------------|------------------|
| | | | |

---

## 4. Build Notes (Week 8)

*What the final test build simulated or left out. Together with the documents, this is the handoff to System Implementation.*

- **Live URL:** *(The app's GitHub Pages address.)*
- **Simulated:** *(Each simulated requirement ID and how it's simulated, e.g. "R1: a 'log in as…' picker instead of real login.")*
- **Not built:** *(Each task you didn't build, with its priority — P2 or P3 only — or "None.")*

---

## SVC Self-Check

- [ ] **Simple** — each task is one action starting with a verb, small enough to finish in under a day, with a one-sentence Done when check.
- [ ] **Valuable** — every task traces to a requirement or ADR; nothing is built "just because."
- [ ] **Complete** — every P1 requirement has at least one task; P1 tasks come first in the Plan's build order; the cross-check has no empty cells; the team approved it before the Week 6 build; every gap has a fix or a reason it wasn't fixed; the Build Notes are filled in by the end of Week 8.

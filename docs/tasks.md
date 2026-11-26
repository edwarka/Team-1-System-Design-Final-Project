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
| Agent got stuck: T1 requires schedule seed records, but no supported make/model/year, task intervals, or manufacturer citations have been selected. The Plan says not to invent schedule values. | docs/plan.md Section 3, Screens & Build Order; docs/data-api-design.md Section 1, MaintenanceSchedule | | |
| Agent got stuck: T1 requires a sample Driver, but the proposed simulated credentials are pending approval and no fabricated account/password-hash fixture is specified. | docs/data-api-design.md Section 0, Proposed simulated behaviors, and Section 1, Driver; docs/plan.md ADR-05 | | |
| ASSUMPTION: Use the Honda Civic 2015 at 60,000 miles from R1, the 62,000 and 65,000 due points from R4, and the Plan's explicitly fabricated 5,000-mile fixture to seed 2,000- and 5,000-mile intervals. These are test fixtures, not verified manufacturer recommendations or citations. | docs/specification.md Section 4, R1 and R4; docs/plan.md Section 3, review cases and sample interval; docs/data-api-design.md Section 1, MaintenanceSchedule | | |
| ASSUMPTION: Use "Oil change" and "Fluid check" for the two schedule task names and author short explanations, because R4 gives due points but no names or explanations for both tasks. | docs/specification.md Section 1, Problem and Evidence, and Section 4, R6; docs/data-api-design.md Section 1, MaintenanceSchedule | | |
| ASSUMPTION: Hash the R24 sample password using PBKDF2-SHA-256 with a fixture salt and 100,000 iterations, serialized inside password_hash; the documents prohibit plaintext but specify no algorithm or representation. This is simulated fixture data only. | docs/specification.md Section 4, R24; docs/data-api-design.md Section 0, We do NOT store, and Section 1, Driver; docs/plan.md ADR-05 | | |
| ASSUMPTION: Set reminders_enabled to false and use a fixed timestamp for the required created_at seed field; the Driver design does not specify their initial values for fabricated seed data. | docs/data-api-design.md Section 1, Driver; Section 4, POST /drivers | | |
| ASSUMPTION: Log a sanitized Driver record in the browser console and omit phone/password_hash, because the task requests console visibility while the Data & API Design identifies both as sensitive. | docs/tasks.md Section 1, T1 Done when; docs/data-api-design.md Section 0, Sensitive fields | | |
| ASSUMPTION: Use the R24 formatted sample phone number in the seed record; R28 and POST /drivers specify 10 digits but do not say whether punctuation is accepted or normalized. | docs/specification.md Section 4, R24 and R28; docs/data-api-design.md Section 4, POST /drivers | | |
| ASSUMPTION: Use R24's documented fabricated phone/password example as the sample Driver credential even though the proposed simulated behavior remains pending team approval. | docs/specification.md Section 4, R24; docs/data-api-design.md Section 0, Proposed simulated behaviors; docs/plan.md ADR-05 | | |
| ASSUMPTION: Use fixed valid UUID values for seed primary and foreign keys because the documents specify UUID types but no fixture IDs. | docs/data-api-design.md Section 1, Driver, Car, and MaintenanceSchedule | | |
| ASSUMPTION: Seed MaintenanceCompletion with an empty list because T1 requires no sample completion and no agreed completion fixture is specified. | docs/tasks.md Section 1, T1; docs/data-api-design.md Section 1, MaintenanceCompletion | | |
| ASSUMPTION: Load the first fabricated Driver and only its linked Car for console inspection; an active signed-in driver/session is not yet available in T1. | docs/plan.md Section 3, Shared data service; docs/data-api-design.md Section 4, POST /sessions; docs/tasks.md Section 1, T1 and T3 | | |
| ASSUMPTION: Use developer-facing error text when the required sample Driver, linked Car, or supported schedule seed is absent; the documents define no seed-loading error copy. | docs/tasks.md Section 1, T1 Done when; docs/data-api-design.md Section 0, Build option | | |
| T2/R24 expects sign-up to leave the driver signed in, but POST /drivers returns only {id, phone} and POST /sessions is a separate operation. | docs/specification.md Section 4, R24; docs/data-api-design.md Section 4, POST /drivers and POST /sessions | | |
| ASSUMPTION: Create a simulated UUID session token, retain the active session in browser storage, and clear it for sign-out; token lifetime, storage behavior, and a sign-out endpoint are unspecified. | docs/data-api-design.md Section 4, POST /sessions; docs/plan.md Section 3, Screen 3 and Shared data service; ADR-05 | | |
| ASSUMPTION: Use the visible label "Simulation only." because ADR-05 requires labeling the simulation but no exact label copy is provided. | docs/plan.md ADR-05 and Section 3, Screen 3; docs/data-api-design.md Section 0, Proposed simulated behaviors | | |
| ASSUMPTION: After sign-up, route a new account to car setup and an existing account to My Car; the post-authentication destination is not specified. | docs/specification.md Section 3, S1; docs/plan.md Section 3, Screens 2–4 | | |
| The API defines a 409 duplicate-phone conflict but no exact message for the sign-up screen. | docs/data-api-design.md Section 4, POST /drivers; docs/style-guide.md Section 4, Form field | | |
| ASSUMPTION: Accept plain 10-digit and hyphenated phone values, normalize to 10 digits for uniqueness/storage, and reject other punctuation; accepted formatting and normalization are unspecified. | docs/specification.md Section 4, R24 and R28; docs/data-api-design.md Section 4, POST /drivers | | |
| ASSUMPTION: Use field-specific errors for missing make, model, or year and malformed integer values; only the missing-mileage message is exact. | docs/specification.md Section 4, R2–R3; docs/data-api-design.md Section 4, POST /cars; docs/style-guide.md Section 4, Form field | | |
| ASSUMPTION: Show an explicit error if a driver already has a car; the unique driver_id constraint has no specified user-facing conflict behavior. | docs/data-api-design.md Section 1, Car, and Section 4, POST /cars | | |
| The upcoming-maintenance endpoint specifies task ordering and unsupported-car behavior but not the response fields for computed due values or the unsupported result shape. | docs/data-api-design.md Section 4, GET /cars/:id/upcoming-maintenance; docs/plan.md Section 2, Data handoff | | |
| The task-detail endpoint requires a general-guidance notice but does not define its response field or shape. | docs/data-api-design.md Section 4, GET /maintenance-tasks/:id; docs/specification.md Section 4, R7 | | |
| ASSUMPTION: Use concise UI states for no saved car and missing task because the 404/no-car cases have no exact screen copy. | docs/data-api-design.md Section 4, GET /cars/me and GET /maintenance-tasks/:id; docs/plan.md Section 3, Screens 2 and 5 | | |
| ASSUMPTION: Use field-specific copy for an empty completion date/mileage and invalid negative or fractional mileage; the Style Guide defines numeric rules but not these exact messages. | docs/specification.md Section 4, R11–R12; docs/data-api-design.md Section 4, POST /cars/:id/completions; docs/style-guide.md Section 4, Form field | | |
| Completion validation conflicts: Data & API Section 1 rejects any mileage below the car reading, while Spec R33 and Data & API Section 4 allow lower mileage for past-dated entries. | docs/data-api-design.md Section 1, Failure behavior; Section 4, POST /cars/:id/completions; docs/specification.md Section 4, R12 and R33; docs/plan.md Section 2, Review items | | |
| The Specification still marks Q4 as Open/ASSUMPTION while Plan ADR-07 treats historical maintenance as confirmed; this build follows R33/ADR-07. | docs/specification.md Section 5, Q4; docs/plan.md ADR-07 and Section 2, Review items | | |
| Historical completion ordering for same-date records and which timezone defines "today" are unspecified. | docs/plan.md Section 2, Review items before approval; docs/data-api-design.md Section 1, MaintenanceCompletion; docs/specification.md Section 4, R11–R12 and R33 | | |
| The completion endpoint and Plan require a saved result and reload read-back, but exact response fields for the completion, car, and next due value are unfinished. | docs/data-api-design.md Section 4, POST /cars/:id/completions; docs/plan.md Section 2, Data handoff and review items; docs/specification.md Section 4, R9–R10 and R32–R34 | | |
| ASSUMPTION: Persist car and completion changes together in one browser-storage snapshot to honor all-or-nothing completion saves; the simulation's atomic storage mechanism is unspecified. | docs/plan.md ADR-06; docs/data-api-design.md Section 1, Failure behavior | | |
| T18's task trace omits R32–R34 although Plan review says T18 must include them. | docs/tasks.md Section 1, T18; docs/plan.md Section 2, Task cross-review | | |
| The user's no-testing rule prevents T11 responsive/keyboard checks and T18 acceptance tests/results, which their Done when checks require. | docs/tasks.md Section 1, T11 and T18; docs/plan.md Section 3, Task execution sequence | | |
| ASSUMPTION: Use #EFF6FF for the secondary button's light-blue hover because the Style Guide requires that treatment but gives no exact color. | docs/style-guide.md Section 2, Colors; Section 4, Button (secondary) | | |
| ASSUMPTION: Use the generic status label "Upcoming"; the documents provide example labels but no due-soon/due-now/overdue thresholds for this P1 list. | docs/style-guide.md Section 4, Card; docs/plan.md Section 2, Review items before approval | | |
| ASSUMPTION: Use "Please enter a valid date." for malformed calendar dates because the completion contract gives exact copy only for future dates. | docs/data-api-design.md Section 4, POST /cars/:id/completions; docs/style-guide.md Section 4, Form field | | |
| ASSUMPTION: Accept any whole-number car year because the Car field is an integer and the documents define no minimum, maximum, or supported input range. | docs/data-api-design.md Section 1, Car; docs/specification.md Section 4, R1–R3 | | |
| ASSUMPTION: Use "No upcoming maintenance tasks are listed." when a supported car has no task rows; no exact empty-list response or screen message is specified. | docs/data-api-design.md Section 4, GET /cars/:id/upcoming-maintenance; docs/plan.md Section 3, Screen 2 | | |
| ASSUMPTION: Use "Loading..." and "Please wait" for loading/submission feedback; the documents require feedback and duplicate-submit prevention but do not define its copy. | docs/style-guide.md Section 4, Button (primary); docs/plan.md Section 3, Screens 2 and 6 | | |
| Implementation error: the sample-data console log ran with an active session, and due-mileage calculation used completion records belonging to other cars. Both conflict with driver-only data access. | docs/specification.md Section 4, R13; docs/plan.md Section 3, Screens 1 and 3; docs/data-api-design.md Section 1, Car and MaintenanceCompletion | | |
| ASSUMPTION: Set newly created accounts' reminders_enabled value to false; the account-creation contract does not define its initial value. | docs/data-api-design.md Section 1, Driver; Section 4, POST /drivers | | |
| ASSUMPTION: Set created_at to the current UTC timestamp for newly created accounts; the API does not specify who supplies this value or its time basis. | docs/data-api-design.md Section 1, Driver; Section 4, POST /drivers | | |

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

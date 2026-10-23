# Vehicle Maintenance Tracker — Plan

> **When:** Week 3 (Sections 1–2), Week 4 (Sections 3–4)  
> **Owner:** DeLorean Wiggins — Architecture Lead, Plan  
> **Version:** 0.6 working proposal — October 23, 2026  
> **Status:** Draft for team review; architecture approval pending.  
> **Approved on:** Pending  
> **Approved by:** DeLorean Wiggins — pending; Nasif Rahman — pending; Michal Broniek — pending; Kaleb Edwards — pending.  
> **Done when:** every item in the SVC Self-Check is true.  
> **Filled-in Example:** `docs/examples/plan-example.md`  

**Basis:** PR #1 was merged on October 22, 2026, adding the Vehicle Maintenance Tracker Business Case and the Specification’s three Constitution principles. The Specification records Q2 (CARFAX comparison) as resolved; Q1 (maintenance recommendation method) remains open. This Plan follows those merged documents. Approval of this architecture draft is still pending.

**Planning status:** All architectural choices below are proposals. Requirement IDs and priorities remain pending Michal’s Specification; Business Case, Constitution, and Q1 references provide context, not substitute requirement IDs. No interview or test results are claimed.

## 1. Approach

The proposed phone-friendly app lets drivers enter a car, see upcoming maintenance with plain-language explanations, record completed work, and view its history. The course demonstration uses made-up records and simulated login and storage. Nasif’s Data & API Design will define how information is organized and exchanged so the demonstration can later be replaced by a real backend. The source and coverage of maintenance recommendations, and the future database, still need team decisions.

## 2. Key Decisions (ADRs)

| ADR | Decision | Req IDs | Alternatives considered | Why this one |
|---|---|---|---|---|
| ADR-00 | **Proposed course stack:** Vue, no build step; `collection-template` starter; simulated backend with JSON seed data and browser storage; GitHub Pages from the repository root. **Database designed for:** pending selection; provider also pending. | Pending | Real backend now; framework with a build step | Uses the course defaults to test the design without setting up production infrastructure. The simulation cannot demonstrate real account security or shared persistence. Docker is not required by this stack. |
| ADR-03 | **Proposed:** keep maintenance calculation behind a shared data-service operation, using the agreed inputs and outputs from Data & API Design. The source of recommendations and calculation rules remain unresolved under Q1. | Pending; Q1 | Embed calculation rules in each screen; let each screen call an outside schedule service directly | Gives screens one consistent way to request recommendations and keeps the calculation separate from presentation. This adds a shared contract to maintain, but allows the rules or backend to change without rewriting each screen. |
| ADR-04 | **Proposed:** use the course’s simulated user picker and fabricated profiles; scope mock data-service operations to the selected driver. Any future backend storing real records must enforce authenticated ownership checks. | Pending; Constitution 2 | Implement real authentication and authorization during this course | Demonstrates intended ownership behavior without collecting private driver data. A user picker and browser filtering do not provide real access control; the simulation cannot prove production privacy. |

The merged Business Case defines manual vehicle entry and maintenance completion. The Specification’s Constitution defines plain-language guidance, driver privacy, and the prohibition on commercial promotion. Those product requirements are inputs to the architecture.

When an approved architecture decision changes, retain its ADR and add a replacement. The current rows remain proposals.

## 3. Screens & Build Order

This is a candidate inventory, pending the Specification’s 1–2 P1 scenarios, requirement IDs, priorities, and the Week 3 prototype test. Routes are illustrative browser hash routes, not agreed API endpoints. Extend the data service alongside each scenario instead of building all infrastructure first.

| # | Screen or part | Route | Shows / does | Req IDs | Priority |
|---|---|---|---|---|---|
| 1 | Minimum simulated data service | Not a screen | Supplies only fabricated records needed for the first browse journey, following Nasif’s agreed contract. Extend with later journeys. | Pending | Pending; foundation for chosen P1 |
| 2 | Demonstration user selector | Proposed `#/demo-user` | Chooses a fabricated driver, labels the simulation, and changes the displayed records when the driver changes. | Pending | Pending |
| 3 | Vehicle list | Proposed `#/vehicles` | Shows that driver’s vehicles and agreed empty state, with a path to setup or detail. | Pending | Pending |
| 4 | Schedule and task explanation | Proposed `#/vehicles/:vehicleId` | Shows sample-vehicle maintenance, due basis, explanations, and the specified unknown, unsupported, and inaccessible-record states. | Pending; Q1 | Pending |
| 5 | Vehicle setup to schedule | Proposed `#/vehicles/new` | Collects specified inputs, validates them, and reaches a schedule or an explicit unavailable state. | Pending; Q1 | Pending |
| 6 | Maintenance completion | Proposed `#/vehicles/:vehicleId/complete/:taskId` | Records specified completion details, handles save failure, and updates schedule/history using agreed rules. | Pending; Q1 | Pending |
| 7 | Maintenance history | Proposed `#/vehicles/:vehicleId/history` | Displays recorded work and the specified empty state, with a route back to the schedule. | Pending | Pending |

**Why this order:** Complete browsing first through the data service and interface, then test setup-to-recommendation, the largest uncertainty in the Business Case. After Michal sets priorities, reorder as needed so every approved P1 scenario is complete end to end before any P2 or P3 work starts.

Before approval, resolve Q1’s date/mileage rules, units, missing service history, and how completion changes what is due next. A mileage threshold must not silently become a calendar prediction. Map every approved requirement to a screen or part; add edit/delete behavior only if the Specification requires it. No prototype evidence is available yet.

## 4. Risks & Assumptions

Ratings are preliminary judgments. The pending decisions above are present blockers; these risks describe what could go wrong after choices are made.

| Risk or assumption | Likelihood | Impact | Mitigation |
|---|---|---|---|
| A recommendation rule may not fit a vehicle or may be mistaken for verified manufacturer advice. | Medium | High | Limit supported coverage, check sources, show the recommendation’s basis, and use the agreed unknown state when a rule cannot be supported. Resolve through Q1. |
| Missing service history or conflicting date/mileage conditions may give a misleading result. | Medium | High | Specify required baselines, units, boundaries, and missing-input behavior; include meaningful acceptance-test examples before implementation. |
| Simulated login may be mistaken for privacy protection. | Medium | High | Use fabricated data only, disclose the demo boundary, and design real backend ownership checks for later. The picker alone does not enforce the Constitution’s real-record privacy promise. |
| Browser storage may be cleared, unavailable, or mistaken for cross-device synchronization. | Medium | Medium | Specify save errors, reset behavior, and persistence limits; make no synchronization promise. |
| Sourcing schedules and checking explanations may exceed the proposed 20 team hours. | Medium | High | Treat 20 hours as an unvalidated estimate, narrow supported coverage, and estimate agreed P1 work before committing to it. |
| Drivers may find the explanations confusing or see little advantage over another app. | Medium | High | Collect genuine user evidence and observe the proposed usability outcome before claiming success. |
| Plan and Data & API Design may drift apart. | Medium | High | DeLorean and Nasif cross-review names, ownership, rules, and operations; fix source documents before dependent tasks or code. |

## SVC Self-Check

- [ ] **Simple** — the approach reads in under a minute; each ADR states one decision.
- [ ] **Valuable** — every ADR and screen has a requirement ID; every "Why this one" names a real trade-off, not "it was easier."
- [ ] **Complete** — ADR-00 names the full stack, starter skill, and database; every P1 requirement is covered by a screen or part; P1 comes first, one scenario end to end at a time; every risk has a mitigation; the team has approved it before Tasks are written.

**Approval:** DeLorean Wiggins, Nasif Rahman, Michal Broniek, and Kaleb Edwards must review and approve the completed Plan before Tasks are written. Pending decisions and review requests are listed in PR #2.

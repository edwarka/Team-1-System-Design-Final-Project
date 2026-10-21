# Vehicle Maintenance Tracker — Plan

> **When:** Week 3 (Sections 1–2), Week 4 (Sections 3–4)  
> **Owner:** DeLorean Wiggins — Architecture Lead, Plan  
> **Version:** 0.5 working proposal — October 21, 2026  
> **Status:** Draft for team review; not approved and not a Week 1 submission.  
> **Approved on:** Pending  
> **Approved by:** DeLorean Wiggins — pending; Nasif Rahman — pending; Michal Broniek — pending; Kaleb Edwards — pending.  
> **Done when:** every item in the SVC Self-Check is true.  
> **Filled-in Example:** `docs/examples/plan-example.md`  

**Basis:** Mike’s open PR #1 proposes a Vehicle Maintenance Tracker, three Constitution principles, and questions Q1 (maintenance recommendation method) and Q2 (CARFAX comparison). The main-branch Plan was still the blank course template when checked. This draft follows that working proposal; it does not record unanimous team agreement or sponsor approval.

**ASSUMPTION:** The team adopts the direction in PR #1. All architectural choices below are proposals. Requirement IDs and priorities remain pending Michal’s Specification; Business Case, Constitution, and Q1 references provide context, not substitute requirement IDs. No interview or test results are claimed.

## 1. Approach

The proposed phone-friendly app lets drivers enter a car, see upcoming maintenance with plain-language explanations, record completed work, and view its history. The course demonstration uses made-up records and simulated login and storage. Nasif’s Data & API Design will define how information is organized and exchanged so the demonstration can later be replaced by a real backend. The source and coverage of maintenance recommendations, and the future database, still need team decisions.

## 2. Key Decisions (ADRs)

| ADR | Decision | Req IDs | Alternatives considered | Why this one |
|---|---|---|---|---|
| ADR-00 | **Proposed course stack:** Vue, no build step; `collection-template` starter; simulated backend with JSON seed data and browser storage; GitHub Pages from the repository root. **Database designed for:** pending DeLorean and Nasif’s proposal and team agreement. PostgreSQL is a candidate, not a selection; provider is pending. | Pending | Real backend now; framework with a build step; another agreed database | Uses the course defaults to test the design without setting up production infrastructure. It cannot demonstrate real account security or shared persistence. Name the database consistently here and in Data & API Design Section 0 before completing Week 3. Docker is not required by the default stack. |
| ADR-01 | **Proposed:** enter vehicle details and maintenance completion manually, using Michal’s required inputs and Nasif’s field definitions. | Pending; Business Case §2 | VIN decoding; connected mileage feeds; repair-shop integrations | Avoids external accounts and hardware, but requires driver entry and correction. Required inputs depend on the recommendation method. |
| ADR-02 | **Earlier local proposal withdrawn; replaced by ADR-03:** determine due work only from user-entered intervals. This was never approved. | Pending | User-supplied intervals; recommended schedules | Asking inexperienced drivers to know all intervals does not directly meet Mike’s proposed outcome. Keep the alternative if the team later changes that promise. |
| ADR-03 | **Proposed, subject to Q1:** use a small, source-checked maintenance catalog with plain-language explanations and a visible basis for recommendations. For unsupported vehicles or missing inputs, show an agreed unavailable/unknown state. | Pending; Constitution 1; Q1 | Licensed schedule service; user-entered intervals; history-only tracker | A bounded catalog can demonstrate the journey without a live vendor dependency, but limits coverage and requires content verification. Nasif proposes the method and supported coverage; Michal defines user-visible rules; the team decides. No intervals or automotive rules are invented here. |
| ADR-04 | **Proposed:** use the course’s simulated user picker and fabricated profiles; the mock data service limits each operation to the selected driver’s records. Design real backend ownership checks for any future version with real records. | Pending; Constitution 2 | Real authentication/backend now; single-user design if the team changes scope | Demonstrates intended ownership behavior without collecting private driver data. Browser filtering and a user picker are not real access control. Michal clarifies the simulation boundary; Nasif defines ownership and API behavior. |

Constitution principle 3 in PR #1 rules out promoting or selling shops, parts, or products. No commercial integration is proposed here. Push notifications, payments, live vehicle connections, and production account management are not assumed.

Retain replaced ADRs and add a new row when a choice changes. Every current choice above remains unapproved.

## 3. Screens & Build Order

This is a candidate inventory, pending the Specification and Week 3 prototype test. Routes are illustrative browser hash routes, not agreed API endpoints. Michal must identify the 1–2 P1 scenarios and assign actual requirement IDs and priorities; Nasif owns the data contract, and Kaleb owns the Style Guide. Extend the data service alongside each scenario instead of building all infrastructure first.

| # | Screen or part | Route | Shows / does | Req IDs | Priority |
|---|---|---|---|---|---|
| 1 | Minimum simulated data service | Not a screen | Supplies only fabricated records needed for the first browse journey, following Nasif’s agreed contract. Extend with later journeys. | Pending | Pending PO; foundation for chosen P1 |
| 2 | Demonstration user selector | Proposed `#/demo-user` | Chooses a fabricated driver, labels the simulation, and changes the displayed records when the driver changes. | Pending | Pending PO |
| 3 | Vehicle list | Proposed `#/vehicles` | Shows that driver’s vehicles and agreed empty state, with a path to setup or detail. | Pending | Pending PO |
| 4 | Schedule and task explanation | Proposed `#/vehicles/:vehicleId` | Shows sample-vehicle maintenance, due basis, explanations, and the specified unknown, unsupported, and inaccessible-record states. | Pending; Q1 | Pending PO |
| 5 | Vehicle setup to schedule | Proposed `#/vehicles/new` | Collects specified inputs, validates them, and reaches a schedule or an explicit unavailable state. | Pending; Q1 | Pending PO |
| 6 | Maintenance completion | Proposed `#/vehicles/:vehicleId/complete/:taskId` | Records specified completion details, handles save failure, and updates schedule/history using agreed rules. | Pending; Q1 | Pending PO |
| 7 | Maintenance history | Proposed `#/vehicles/:vehicleId/history` | Displays recorded work and the specified empty state, with a route back to the schedule. | Pending | Pending PO |

**Why this order:** Complete browsing first through the data service and interface, then test setup-to-recommendation, the largest uncertainty in the Business Case. After the PO sets priorities, reorder as needed so every approved P1 scenario is complete end to end before any P2 or P3 work starts.

Before approval, resolve Q1’s date/mileage rules, units, missing service history, and how completion changes what is due next. A mileage threshold must not silently become a calendar prediction. Map every approved requirement to a screen or part; add edit/delete behavior only if the Specification requires it. No prototype evidence is available yet.

## 4. Risks & Assumptions

Ratings are preliminary judgments. The pending decisions above are present blockers; these risks describe what could go wrong after choices are made.

| Risk or assumption | Likelihood | Impact | Mitigation |
|---|---|---|---|
| A catalog rule may not fit a vehicle or may be mistaken for verified manufacturer advice. | Medium | High | Limit supported coverage, check sources, show the recommendation’s basis, and use the agreed unknown state when a rule cannot be supported. Resolve through Q1. |
| Missing service history or conflicting date/mileage conditions may give a misleading result. | Medium | High | Specify required baselines, units, boundaries, and missing-input behavior; include meaningful acceptance-test examples before implementation. |
| Simulated login may be mistaken for privacy protection. | Medium | High | Use fabricated data only, disclose the demo boundary, and design real backend ownership checks for later. The picker alone does not enforce the Constitution’s real-record privacy promise. |
| Browser storage may be cleared, unavailable, or mistaken for cross-device synchronization. | Medium | Medium | Specify save errors, reset behavior, and persistence limits; make no synchronization promise. |
| Sourcing schedules and checking explanations may exceed the proposed 20 team hours. | Medium | High | Treat 20 hours as an unvalidated estimate, narrow supported coverage, and estimate agreed P1 work before committing to it. |
| Drivers may find the explanations confusing or see little advantage over another app. | Medium | High | Collect genuine user evidence, resolve Michal’s Q2, and observe the proposed usability outcome before claiming success. |
| Plan and Data & API Design may drift apart. | Medium | High | DeLorean and Nasif cross-review names, ownership, rules, and operations; fix source documents before dependent tasks or code. |
| Private repository settings may prevent the intended free GitHub Pages deployment. | Medium | Medium | Kaleb confirms hosting eligibility and resolves the difference from the README’s public-repository setup before deployment. No visibility change is made by this Plan. |

## SVC Self-Check

- [ ] **Simple** — the approach reads in under a minute; each ADR states one decision.
- [ ] **Valuable** — every ADR and screen has a requirement ID; every "Why this one" names a real trade-off, not "it was easier."
- [ ] **Complete** — ADR-00 names the full stack, starter skill, and database; every P1 requirement is covered by a screen or part; P1 comes first, one scenario end to end at a time; every risk has a mitigation; the team has approved it before Tasks are written.

**Review needed:** DeLorean and Nasif agree the architecture proposal; Michal confirms scope, requirements, priorities, and Q1/Q2 implications; Kaleb checks delivery constraints; all four provide actual approval before this becomes the approved Plan. This early draft does not replace the October 22 Week 1 business-case, Constitution, and weekly-status work.

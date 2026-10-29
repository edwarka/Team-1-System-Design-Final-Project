# Vehicle Maintenance Tracker: Specification

> **When:** Week 1 (Section 0), Week 2 (the rest), refined in Week 3 · **Owner:** Product Owner (Michal Broniek) · **Done when:** every item in the SVC Self-Check at the end is true.
>
> **Version:** 1.0
>
> **Approved on:** ___ **by:** Product Owner Michal Broniek ___ · Architecture Leads Delorean Wiggins ___ and Nasif Rahman ___ · Delivery Lead Kaleb Edwards ___
>
> **Filled-in Example:** `docs/examples/specification-example.md`

The Spec is the source of truth for *what* you're building. Write it so a teammate, or an AI agent, could build from it without asking you a question.

---

## 0. Constitution

| # | Principle | Why it exists |
|---|-----------|---------------|
| 1 | Every maintenance task is explained in plain language (what it is and why it matters) and labeled as general guidance, pointing drivers to their owner's manual or a mechanic when in doubt. | Not knowing what the car needs is the problem we're solving, and these drivers can't tell when general advice is wrong for their car. |
| 2 | A driver's car details and maintenance history are visible only to that driver. | Maintenance records are personal. Anyone who sees a car can read its plate, and many could guess the owner's last name, so data must never be looked up by what's visible on the car alone. |
| 3 | The app never promotes or sells specific shops, parts, or products. | Drivers who can't judge advice for themselves need to trust that recommendations are about their car, not about selling them something. |

---

## 1. Problem & Evidence

**Problem and success:** Many drivers, especially first-time car owners, don't know what maintenance their car needs or when it's due. Oil changes and fluid checks get missed, and small problems turn into expensive repairs. Success: in testing with 5 to 10 drivers who don't know much about cars, at least 8 out of 10 can set up their car and tell us what maintenance is due next and when, without help.

**Evidence:**

| ID | What we heard or saw | From whom |
|----|----------------------|-----------|
| E1 | Couldn't name any maintenance beyond gas and wiper fluid, and relies on the mechanic to say what's needed. 2 of 5 drivers said this. | Drivers, interviews (Persons 2 and 4) |
| E2 | Skipped a recommended timing belt change; it snapped on the highway and destroyed the engine. All 5 described a missed or delayed task that cost them money or time. | Drivers, interviews (all 5; the story is Person 4's) |
| E3 | Agrees to whatever the shop recommends because they don't know enough to push back. | Driver, interview (Person 1) |
| E4 | Records were kept 5 different ways: glovebox receipts, the dealership's system, a notebook, the windshield sticker, a phone calendar. | Drivers, interviews (all 5) |
| E5 | Deleted a maintenance app because setup asked for too much information. Another driver installed CARFAX but kept forgetting to check it. | Drivers, interviews (Persons 4 and 1) |
| E6 | 2 of 5 wanted to check it on their phone, one while standing at the repair shop. The other 3 preferred email, a text message, or the car's dashboard screen. | Drivers, interviews (all 5; Persons 1 and 5 wanted the phone) |

**Riskiest assumption:** Drivers will come back to check the app on their own, without being reminded, when maintenance is due. One interviewee installed CARFAX but kept forgetting to check it (E5), and 3 of 5 wanted reminders to come to them. We'll find out in the Week 3 prototype test by asking testers how they'd expect to learn something is due, and by watching whether they look for a reminder option.

---

## 2. Scope

**In scope:** Setting up one car with its make, model, year, and mileage. Seeing upcoming maintenance with a plain-language explanation of each task. Marking tasks done with a date and mileage. Updating the car's mileage. Viewing past maintenance. Creating an account with a phone number and a password, signing in, and resetting a forgotten password by text. Reminders when a task is due, if time allows (S5).

**Out of scope:** More than one car per driver. Looking up a car by VIN or license plate. Connecting to the car, a sensor, or a dealer account. Recommending or booking shops, and selling parts or products. Repair cost estimates. Diagnosing problems or warning lights. A downloadable phone app.

---

## 3. User Scenarios

**S1: Set up my car and see what it needs**, Priority: P1
- **Actor:** A driver who doesn't know much about cars
- **Trigger:** Wants to know what maintenance their car needs
- **Steps:** Opens the app → creates an account with a phone number and a password → enters make, model, year, and current mileage → sees upcoming maintenance, soonest first → opens a task to read what it is and why it matters
- **Success outcome:** Sees what's due next and when. Each task is explained in plain language and labeled as general guidance.
- **Failure outcome:** The app has no schedule for that car → sees "We don't have a schedule for this car yet. Check your owner's manual or ask a mechanic." If a detail is missing, the app says which one.

**S2: Mark maintenance as done**, Priority: P1
- **Actor:** Driver
- **Trigger:** Just had an oil change or another task done
- **Steps:** Signs in → opens their car → picks the task → marks it done with the date and mileage
- **Success outcome:** The task moves to their history, and the next time it's due shows up in the upcoming list.
- **Failure outcome:** The date is in the future, or the mileage is lower than the last recorded mileage → the task isn't saved, and the app says what to fix.

**S3: Update my car's mileage**, Priority: P2
- **Actor:** Driver
- **Trigger:** Has driven a while since last opening the app
- **Steps:** Opens their car → enters the current mileage
- **Success outcome:** The upcoming list updates, with anything due or overdue at the top.
- **Failure outcome:** The new mileage is lower than the last entry → it isn't saved, and they see "Mileage can't be lower than your last entry."

**S4: Look back at what's been done**, Priority: P2
- **Actor:** Driver
- **Trigger:** A shop recommends a service, and the driver wants to check when it was last done
- **Steps:** Opens their car → opens its maintenance history
- **Success outcome:** Sees every completed task with its date and mileage, newest first.
- **Failure outcome:** Nothing has been recorded yet → sees "No maintenance recorded yet."

**S5: Get reminded when something is due**, Priority: P3
- **Actor:** Driver
- **Trigger:** A task comes due while they aren't using the app
- **Steps:** Turns on reminders → gets a reminder when a task is due → opens the app to see it
- **Success outcome:** Finds out a task is due without having to remember to check.
- **Failure outcome:** The driver hasn't given a way to reach them → the app asks for one when they turn on reminders.

---

## 4. Requirements & Acceptance Tests

| ID | Requirement | Priority | Acceptance test (do → expect) | Evidence |
|----|-------------|----------|-------------------------------|----------|
| R1 | When a driver enters a car's make, model, year, and current mileage, the system shall save the car. | P1 | Enter Honda, Civic, 2015, 60,000 → reload the page → the car is still there with those details | E5 |
| R2 | The system shall not ask for more than four details to set up a car. | P1 | Start car setup → count the fields → no more than 4 | E5 |
| R3 | If a required car detail is missing, then the system shall show which detail is missing. | P1 | Leave mileage blank and save → see "Please enter your car's mileage." | WCAG 2.2 SC 3.3.1 (Error Identification), E5 |
| R4 | When a car is saved, the system shall list its upcoming maintenance tasks, soonest due first. | P1 | Save a sample car with one task due at 62,000 miles and one at 65,000 → the 62,000 task is listed first | E1 |
| R5 | The system shall show each upcoming task's next due date or mileage. | P1 | Open the upcoming list → every task shows a date, a mileage, or both | E1 |
| R6 | When a driver opens a task, the system shall show what the task is and why it matters, in plain language. | P1 | Open "Oil change" → see a short explanation with no unexplained car terms | E1, E3, Constitution 1 |
| R7 | The system shall label every task "General guidance. Check your owner's manual or ask a mechanic." | P1 | Open any task → see "General guidance. Check your owner's manual or ask a mechanic." | Constitution 1 |
| R8 | If the system has no schedule for the entered car, then the system shall show "We don't have a schedule for this car yet. Check your owner's manual or ask a mechanic." | P1 | Enter a make and model not in the schedule data → that message appears and no task list is shown | Constitution 1 |
| R9 | When a driver marks a task done with a date and mileage, the system shall add it to the car's history. | P1 | Mark "Oil change" done on 2026-10-01 at 60,500 → reload the page → the oil change shows it was last done on 2026-10-01 at 60,500 | E4 |
| R10 | When a task is marked done, the system shall work out its next due date or mileage from that completion. | P1 | Mark "Oil change" done at 60,500 → its next due mileage is 60,500 plus the interval in the schedule data | E2 (depends on Q1) |
| R11 | If a completion date is in the future, then the system shall reject it with "The date can't be in the future." | P1 | Mark a task done with tomorrow's date → message appears and nothing is saved | WCAG 2.2 SC 3.3.1 and 3.3.3 (Error Suggestion) |
| R12 | If any mileage entered is lower than the car's last recorded mileage, then the system shall reject it with "Mileage can't be lower than your last entry." | P1 | Car is at 60,500. Mark a task done at 59,000 → message appears and nothing is saved. Update the car's mileage to 60,000 → message appears and the mileage stays 60,500 | WCAG 2.2 SC 3.3.1 and 3.3.3, E2 |
| R13 | The system shall show a driver only their own car and maintenance history. | P1 | Sign in as test driver A → none of test driver B's car or history appears anywhere | Constitution 2 |
| R14 | The system shall show every screen at 320 pixels wide without sideways scrolling. | P1 | Open each screen at 320 px wide → no horizontal scroll bar | WCAG 2.2 SC 1.4.10 (Reflow), E6 |
| R15 | The system shall give every form field a visible text label. | P1 | Inspect car setup and the mark-done form → each field has a label on screen | WCAG 2.2 SC 3.3.2 (Labels or Instructions) |
| R16 | When a driver enters a new current mileage, the system shall save it as the car's mileage. | P2 | Car is at 60,500. Enter 61,200 and save → reload → the car shows 61,200 | E2 |
| R17 | When a car's mileage passes a task's due mileage, the system shall mark that task as overdue. | P2 | A sample task is due at 61,000. Update mileage to 61,200 → that task is marked overdue | E2 |
| R18 | When a driver opens their car's maintenance history, the system shall list every completed task with its date and mileage, newest first. | P2 | Mark two tasks done, on 2026-09-01 and 2026-10-01 → history shows the 2026-10-01 task first, each with its date and mileage | E3, E4 |
| R19 | If no maintenance has been recorded, then the system shall show "No maintenance recorded yet." | P2 | Open history for a newly set up car → that message appears | E4 |
| R20 | If a driver turns on reminders with no email saved, then the system shall ask for an email address. | P3 | Turn on reminders with no email saved → the app asks for an email before reminders turn on | E5, E6 |
| R21 | If the email address entered for reminders isn't valid, then the system shall reject it with "Please enter a valid email address." | P3 | Enter "driver@" and save → message appears and reminders stay off | WCAG 2.2 SC 3.3.1 and 3.3.3 |
| R22 | When a task becomes due, the system shall send the driver an email reminder naming the task. | P3 | With reminders on, update mileage past a task's due mileage → an email reminder for that task is sent to the saved address | E5, E6 |
| R23 | When a driver opens the upcoming list, the system shall show it within 3 seconds on a home connection. | P2 | Open the upcoming list for a car with 10 tasks → the list is visible within 3 seconds | E5 |
| R24 | When a driver signs up with a phone number and a password, the system shall create an account. | P1 | Sign up with 555-201-4477 and "Oilchange#1" → signed in to a new, empty account | Constitution 2 |
| R25 | When a driver signs in with their phone number and password, the system shall open their account. | P1 | Sign out, then sign in with 555-201-4477 and "Oilchange#1" → their car appears | Constitution 2 |
| R26 | If the phone number and password don't match an account, then the system shall show "That didn't match an account. Check your phone number and password and try again." | P1 | Sign in with the wrong password → message appears and no car is shown | WCAG 2.2 SC 3.3.1, Constitution 2 |
| R27 | If a new password is shorter than 8 characters, then the system shall reject it with "Your password needs at least 8 characters." | P1 | Sign up with "car123" → message appears and no account is created | NIST SP 800-63B |
| R28 | If the phone number isn't 10 digits, then the system shall reject it with "Please enter a 10-digit phone number." | P1 | Sign up with 555-2014 → message appears and no account is created | WCAG 2.2 SC 3.3.1 and 3.3.3 |
| R29 | When a driver asks to reset their password, the system shall text a reset code to their account's phone number. | P2 | Tap "Forgot password," enter 555-201-4477 → a reset code is sent by text to that number | Constitution 2 |
| R30 | When a driver enters a valid reset code and a new password, the system shall replace the old password. | P2 | Enter the texted code and "Brakes#2026" → sign in works with the new password and not the old one | Constitution 2 |
| R31 | If a reset code is wrong or more than 10 minutes old, then the system shall reject it with "That code didn't work. Request a new one." | P2 | Enter a code 11 minutes after it was sent → message appears and the password doesn't change | WCAG 2.2 SC 3.3.1 and 3.3.3 |

---

## 5. Open Questions

| ID | Question | Raised by | Owner | Status |
|----|----------|-----------|-------|--------|
| Q1 | How will the app decide which maintenance each car needs and when (hand-built rules, standard intervals by mileage/age, or an outside data source)? | Michal Broniek (Business Case §2) | Nasif Rahman | Open |
| Q2 | Does CARFAX Car Care already explain maintenance tasks in plain language? Someone on the team should try it before Business Case Section 3 is final. | Michal Broniek (Business Case §3) | Michal Broniek | Resolved: No. CARFAX Car Care does not explain maintenance in a way that people who know nothing about vehicles can easily understand. Business Case §3 updated. |
| Q3 | How does a driver get back to their saved car? One idea is last name and license plate, but anyone who sees the car could guess both, which breaks Constitution rule 2. Options: last name and plate plus a PIN, plate plus a PIN, an email link, or a regular login system with an email and password. A login system is the most familiar to drivers, but it adds account setup, which E5 says can drive people away. Affects R13. | Michal Broniek (Spec §3, R13) | Whole Team | Resolved: drivers create an account with a phone number and a password of at least 8 characters. Forgotten passwords are reset with a code sent by text. Email is only collected for reminders. This goes against E5, but it's the safest option. Scope, S1, S2, and R24 to R31 updated. |

---

## SVC Self-Check

- [x] **Simple** — each requirement is one sentence in one EARS pattern; no jargon a new teammate wouldn't know.
- [ ] **Valuable** — the Constitution has 2–3 principles that would actually stop a feature; the Evidence table has 3+ rows from real people outside the team, plus the prototype test; each P1 requirement cites evidence or a standard, or says ASSUMPTION; the riskiest assumption is named.
- [ ] **Complete** — 3–5 scenarios, each with actor, trigger, steps, and both outcomes; P1 is 1–2 scenarios that make a complete journey; every requirement has a unique ID, a priority, and an acceptance test; no open question blocks a P1 requirement; the team has approved it.

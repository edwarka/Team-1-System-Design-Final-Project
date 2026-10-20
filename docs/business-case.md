# Vehicle Maintenance Tracker — Business Case

> **When:** Week 1 · **Owner:** Product Owner (Michal Broniek) · **Done when:** every item in the SVC Self-Check at the end is true.
>
> **Sponsor decision:** Go / No-go · **By:** ___ · **Date:** ___
>
> **Filled-in Example:** `docs/examples/business-case-example.md`

The Business Case answers one question: *should we do this at all?* Keep it to about one page — detailed requirements belong in the Spec.

---

## 1. Problem

**Who is affected?** Drivers who aren't car-savvy, especially first-time owners like college students and recent grads, who don't know what maintenance their car needs or when.

**What's wrong today?** They don't know which maintenance their car needs or when it's due, so they rely on memory, a windshield sticker, or a reminder from the shop, and things like oil changes and fluid checks get missed. Small missed tasks can turn into expensive repairs or breakdowns.

**How will we know it worked?** In testing with 5–10 non-car-savvy drivers, at least 80% can set up their car and say which maintenance is due next, and when, without outside help.

---

## 2. Proposed Solution

A simple web app, designed to be easy to use on a phone, where drivers enter basic details about their car, such as its make, model, year, and mileage, and get a recommended maintenance schedule. The schedule combines tasks every car needs, like oil changes and fluid checks, with tasks specific to that car's age and type, and explains each one in plain language: what it is and why it matters. Drivers can mark tasks as done, see what's due next, and keep a record of what was done and when, without needing to know anything about cars.

---

## 3. Options Considered

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| Do nothing | Keep relying on memory, windshield stickers, and reminders from the shop | No cost, no effort | About a third of drivers already skip or delay recommended service; repairs average $500–$600 |
| Option A | Use a do-it-yourself tracker app (Drivvo, Simply Auto, Fuelly) | Free to start; paid tiers about $10–$50/yr | Drivers must already know what maintenance to schedule and when |
| Option B | Use CARFAX Car Care, or FIXD with its sensor | CARFAX is free and recommends a schedule for the car | Not built around explaining maintenance to non-car-savvy drivers (to verify, Q2); FIXD needs a sensor and Premium costs about $70–$100/yr |
| **Option C (recommended)** | Build a simple, phone-friendly web app that recommends a schedule and explains each task in plain language | Made for non-car-savvy drivers; no sensor and no dealer or third-party account needed | Takes build time; recommendations depend on Q1; must clearly beat a free competitor |

Sources: [AAA app roundup (Jan 2026)](https://www.acg.aaa.com/connect/blogs/4c/auto/best-car-maintenance-apps) · [AAA Newsroom (Apr 2017)](https://newsroom.aaa.com/2017/04/one-three-u-s-drivers-cannot-pay-unexpected-car-repair-bill/) · [Simply Auto](https://apps.apple.com/us/app/id893278325) · [Drivvo](https://apps.apple.com/app/id1206041425) · [CARFAX app review (2013)](https://agirlsguidetocars.com/app-maintenance-reminders-carfax) · [FIXD reviews](https://www.consumeraffairs.com/automotive/fixd.html) · [FIXD owner review](https://iamcarhacker.com/?p=15082)

---

## 4. Can We, and Is It Worth It?

| Question | Answer |
|----------|--------|
| **Will people actually use it?** | Likely. About a third of drivers already skip or delay recommended service, and non-car-savvy drivers (including a team member who has owned cars for nearly 20 years) say they don't know what's needed or when. We'll confirm with 3–5 drivers outside the team. |
| **Can we build it with what we have?** | Yes. All four team members have built a website before, and the course provides a ready-made starting point for the app. The main unknown is how the app decides what each car needs (Q1); a small built-in set of common maintenance rules should be enough for this project. |
| **Can it be done in time to matter?** | Yes. A basic version covering setup, the recommended schedule, and marking tasks done can be ready by Week 8 of the course. |
| **What does it cost?** One-time and per year — include ongoing costs such as hosting. | About 20 team hours to build. Hosting is $0 on a free hosting service, and ongoing cost stays $0 a year unless we later add online storage. |
| **What do we gain?** Say which benefits are measurable, and how much. | Drivers know what's due next and when (measurable: the 80% target in Section 1). Fewer missed tasks and avoided repairs, which average $500–$600 each (real, but hard to measure in this project). |

---

## 5. Recommendation

Approve Option C: build a simple, phone-friendly web app this term that recommends each car's maintenance schedule and explains every task in plain language, with a basic version ready by Week 8. About a third of drivers already skip or delay recommended service, and each wait risks another $500–$600 repair.

---

## SVC Self-Check

- [x] **Simple** — fits on about one page; no tech stack or screen details.
- [x] **Valuable** — the problem names a specific group and a specific pain, not a solution; success is a measurable outcome, not a feature list.
- [ ] **Complete** — "Do nothing" is compared; ongoing costs are included; the recommendation is one clear choice with a reason to act now; the sponsor has decided.

---

*AI assistance: drafted with help from Claude Opus 5.5. Competitor research and cost figures were gathered with Claude and checked against the linked sources; all wording was reviewed and approved by Michal Broniek.*

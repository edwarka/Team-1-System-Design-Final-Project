# Vehicle Maintenance Tracker — Style Guide

**When:** Week 2, alongside the Spec  
**Owner:** Delivery Lead (whole team approves)  
**Version:** 1.0

**Approved on:** ___ by: Delivery Lead (Kaleb Edwards) · Product Owner ___ · Architecture Lead ___

## 1. Brand & Voice

### Brand principles

Vehicle Maintenance Tracker is meant to help people who aren't very familiar with cars understand what maintenance they need to keep up with. The app should be simple to use, easy to understand and not make car maintenance feel like a complicated job.

We want drivers to feel more confident about looking after their cars. The app shouldn't scare people, blame them for missing a service or try to sell them anything. If we're not sure that maintenance advice applies to their particular car, we'll say it's general guidance and recommend checking the owner's manual or speaking to a mechanic.

### Tone

- **Plain:** Write the way people normally speak. Avoid technical terms where possible.
- **Reassuring:** Don't make people feel bad if they've missed a service.
- **Brief:** Keep messages short and get to the point.
- **Honest:** Make it clear when advice is general rather than specific to their car.
- **Helpful:** Tell drivers what to do next if they need more information.

### Writing rules

The app should use simple wording that makes sense to someone who knows very little about cars.

| Situation | Write this | Not this |
|---|---|---|
| Saving a car | Save my car | Submit |
| Finishing a task | Mark as done | Finalize service record |
| Changing mileage | Update mileage | Modify odometer data |
| Upcoming maintenance | Oil change due soon | Critical maintenance event |
| Missing mileage | Please enter your car's mileage. | Invalid input |
| Incorrect mileage | Mileage can't be lower than your last entry. | Invalid input |
| Incorrect date | The date can't be in the future. | Date validation failed |
| No schedule available | We don't have a schedule for this car yet. Check your owner's manual or ask a mechanic. | No data found |
| Empty history | No maintenance recorded yet. | No records available |
| Invalid Email | Please enter a valid email address.| Response invalid |

Every maintenance task must include this message:

> General guidance. Check your owner's manual or ask a mechanic.

### Logo

**File location:** N/A — no logo has been created yet.

For now, we'll just use the project name. If a logo is made later, it shouldn't be stretched or use a car manufacturer's branding in a way that suggests they're connected to the app.

## 2. Colors

The app will mainly use blue, white and light grey. Blue will be used for buttons and links, while white backgrounds and dark text will keep the information easy to read.

The contrast ratios below are calculated against the backgrounds shown. They still need to be checked with the WebAIM Contrast Checker before the style guide is approved.

| Name | Hex | Use | Contrast vs. background |
|---|---|---|---|
| Primary | `#1D4ED8` | Main buttons and links | 6.4:1 vs. `#F8FAFC` |
| Text | `#1F2937` | Headings, labels and body text | 14.3:1 vs. `#F8FAFC` |
| Background | `#F8FAFC` | Main page background | N/A |
| Error | `#B91C1C` | Error messages | 6.4:1 vs. `#F8FAFC` |
| Border | `#64748B` | Form fields and button borders | 4.5:1 vs. `#F8FAFC` |
| Primary hover | `#1E40AF` | Primary buttons when hovered over | 7.8:1 vs. `#F8FAFC` |

### Color rules

- Use blue for the main buttons and links.
- Keep text dark and backgrounds light.
- Show errors in red and explain what went wrong in a message.
- Make sure form fields and buttons have borders that are easy to see.
- Primary buttons should turn a darker blue when hovered over.
- Disabled buttons should look different from active ones but still be readable.
- Normal text needs a contrast ratio of at least 4.5:1. Large text needs at least 3:1.
- Important borders and focus indicators need at least 3:1 contrast.
- Check the colors together, especially white text on buttons and the focus outline, before approval.

## 3. Type & Spacing

### Typography

We'll use the standard system font so the app looks familiar on different devices. There's no need to add another font.

| Role | Font | Size | Weight |
|---|---|---|---|
| Heading 1 | System UI | 32px | 700 (bold) |
| Heading 2 | System UI | 24px | 700 (bold) |
| Heading 3 | System UI | 20px | 600 (semibold) |
| Body | System UI | 16px | 400 (regular) |
| Supporting text | System UI | 14px | 400 (regular) |
| Buttons and field labels | System UI | 16px | 600 (semibold) |

### Typography rules

- Use normal sentence case instead of writing headings in all capitals.
- Keep the main text at 16px.
- Use smaller text only for extra details.
- Split longer pages into sections so they're easier to follow.
- Avoid decorative fonts and unnecessary italics.
- Make sure text fits on smaller screens and isn't cut off.

### Spacing scale

We'll use an 8px base unit throughout the app so the spacing stays consistent.

**Spacing scale:** `8 / 16 / 24 / 32 / 48px`

| Token | Size | Use |
|---|---:|---|
| XS | 8px | Small gaps between items |
| S | 16px | Form fields and card padding |
| M | 24px | Space between related sections |
| L | 32px | Space between main sections |
| XL | 48px | Larger gaps between page sections |

**Maximum content width:** 1200px, centred on wider screens.

**Page padding:** 16px on smaller screens and 32px on wider screens.

**Responsive layout:** The app must work on screens as narrow as 320px without horizontal scrolling. Content should fit the screen rather than forcing users to scroll sideways.

## 4. Components

### Button (primary)

- **Appearance:** Blue background (`#1D4ED8`), white text, 8px rounded corners and a minimum height of 44px.
- **Text:** Use labels such as "Save my car" and "Mark as done."
- **Hover:** Change the background to `#1E40AF`.
- **Keyboard focus:** Show a visible 2px outline with a 2px offset.
- **Disabled:** Make the button look inactive and stop it from being clicked.
- **Error:** Show an error message beside the relevant field or action.
- **Behaviour:** If saving takes time, show that it's happening and prevent the same action from being submitted twice.

### Button (secondary)

- **Appearance:** White background, blue text, 2px blue border, 8px rounded corners and a minimum height of 44px.
- **Text:** Use for actions such as "Cancel" and "View history."
- **Hover:** Add a light-blue background, making sure the text stays readable.
- **Keyboard focus:** Use the same visible outline as the primary button.
- **Disabled:** Make it look inactive and prevent it from being used.
- **Error:** Show errors in a message rather than only changing the button's colour.

### Card

- **Appearance:** White background, 1px border (`#64748B`), 8px rounded corners and 16px padding.
- **Content:** Show the task name, when it's due and its status.
- **Hover:** Cards that only display information don't need a hover effect. Clickable cards should change slightly to show they're interactive.
- **Keyboard focus:** Clickable cards must work with a keyboard and have a visible outline.
- **Disabled:** If an action isn't available, make that clear without hiding the card's information.
- **Error:** Show a message explaining the problem inside or beside the card.
- **Maintenance status:** Use labels such as "Due soon," "Due now" and "Overdue." Don't rely on colour alone.

### Form field

- **Appearance:** White background, 1px border (`#64748B`), 8px rounded corners and a minimum height of 44px.
- **Labels:** Every field must have a visible label above it. A placeholder isn't a replacement for a label.
- **Help text:** Explain what to enter if it might not be obvious. For example, "Enter the mileage shown on your dashboard."
- **Hover:** Keep the border visible and make it slightly more noticeable if needed.
- **Keyboard focus:** Show a visible 2px outline with a 2px offset.
- **Disabled:** Make the field look inactive and prevent changes.
- **Error:** Show what's wrong next to the field and explain how to fix it. The message must also be available to assistive technology.
- **Validation:** If something is entered incorrectly, keep the value so the user doesn't have to type it again. Don't save invalid information.
- **Input rules:** Mileage must be a non-negative whole number. Completion dates can't be in the future, and completion mileage can't be lower than the last recorded mileage.

### Navigation

- **Appearance:** Keep the menu simple, with options such as "My car," "Upcoming maintenance" and "History."
- **Layout:** Navigation must fit smaller screens. Use a compact menu if needed.
- **Hover:** Add a slight change to the text or background.
- **Keyboard focus:** Every menu item must work with a keyboard and show a visible outline.
- **Disabled:** Don't let users open pages that aren't available.
- **Error:** If something goes wrong while moving between pages, explain the problem and don't silently lose unsaved information.
- **Current page:** Make it clear which page the user is viewing.
- **Privacy:** Drivers must only see their own car and maintenance history. Their information must be protected by a sign-in system.

### Maintenance task detail

- **Appearance:** Show the task name, when it's due, what the task involves and why it matters.
- **Content order:** Task name → due date or mileage → what it is → why it matters → general guidance.
- **Hover and focus:** Informational text doesn't need a hover effect. Any buttons and fields must follow their own rules.
- **Disabled:** If a task can't be marked as done, explain why but still let the driver read the details.
- **Error:** Show any errors beside the relevant field and explain what needs correcting.
- **Required message:** Every task must include: "General guidance. Check your owner's manual or ask a mechanic."
- **Maintenance advice:** Don't make general maintenance schedules sound like they apply to every car. If the driver isn't sure, direct them to their owner's manual or a mechanic.

## Accessibility & Consistency Rules

The app should meet WCAG 2.2 AA requirements.

- Everything must be usable with a keyboard.
- Focus outlines must be visible and not hidden behind other content.
- Text, buttons, borders and focus indicators must meet the required contrast levels.
- Every form needs visible labels and clear error messages.
- Errors must be identified following WCAG 2.2 SC 3.3.1, and form labels and instructions must follow SC 3.3.2.
- The app must fit a 320px-wide screen without horizontal scrolling for ordinary content, following SC 1.4.10.
- Don't use colour alone to show errors or maintenance status.
- Avoid unnecessary animations and respect reduced-motion settings.
- Buttons and other interactive areas should be at least 44px high where possible.
- Before release, check the screens on a narrow display and test that everything works with a keyboard.

## SVC Self-Check

### Simple

- [x] Six defined colours are listed.
- [x] One font family is used.
- [x] All spacing follows the 8px scale.

### Valuable

- [x] The design avoids scare tactics, confusing language and sales promotions.
- [x] The component rules explain how the main parts of the app should look and behave.
- [x] The guide is written for people who might not know much about cars.

### Complete

- [x] Every section is filled in or marked N/A with an explanation.
- [ ] All colour combinations have been checked with the WebAIM Contrast Checker.
- [x] Components include hover, focus, disabled and error states.
- [ ] The Delivery Lead, Product Owner and Architecture Lead have approved the guide.
- [ ] The approval date and names have been added.

**Approval status:** Draft. The team still needs to check the colour contrast and approve the guide.
# Smart Roster Manager

Live app: https://harsh7982.github.io/smart-roster-manager/

A browser-based tool that helps an operations manager generate, validate and edit monthly rosters for 33 employees across 25 brands and 3 pods. It is a single static HTML file with no backend, so there is nothing to install: open `index.html` in any browser. No environment variables are needed.

## What it does
- Generates a monthly roster from each process's operating calendar, leaves, comp-offs, and public holidays.
- Flags coverage gaps and rule violations, and explains who is unavailable and why.
- Suggests fixes you can apply in one click: moving a weekly off, or borrowing a spare employee as cover.
- Company, pod, brand, process and employee views. Click any cell to edit it and the roster is re-validated instantly.
- A Configure panel to edit calendars, minimum staffing, day-specific minimums, assignments, holidays, leaves, employees and brands without touching code.

## Assumptions
- Weeks run Monday to Sunday. Every employee gets one weekly off per full week and should not work more than 7 days in a row.
- A day when none of an employee's processes operate (e.g. Sunday for a Sun-off process) counts as their weekly off.
- Coverage is mandatory. Preferred off days are soft and only produce warnings.
- Sample holidays and names are fictional. Data is held in memory, so a refresh resets it. There is no export or approval workflow yet.
- I used an AI assistant (Claude) to help build this, as the assignment allows, and I tested the generator myself.

## Tell us how you think

**1. Data model.** Employees belong to a pod (each pod has a lead). Brands contain processes. Each process has its own calendar (365 days / daily except holidays / Sunday and holidays off), a minimum staffing number, and optional day-specific minimums. An allocation links an employee to a brand and a process, so one employee can be on several. The brand's operating calendar is separate from each employee's schedule, which is generated.

**2. Generation and validation.** Each day starts as Working if any of the employee's processes need coverage, otherwise Off. Leaves and comp-offs are applied. Then the generator places each employee's weekly off on the day with the most spare coverage, handling the most constrained employees first, and softly prefers their preferred day. Validation counts staff working for every brand, process and day against the requirement, and checks weekly-off and consecutive-day rules.

**3. Single vs multi-employee brands.** 20 of 25 brands have one employee who handles all processes. Their weekly off has to fall on a non-operating day, which is why their processes use Sunday-off calendars. The other 5 brands have 2 to 3 employees, split across processes or sharing one, and the generator staggers their offs.

**4. Sole employee unavailable.** Every process they cover is flagged as a mandatory gap for that day, with the cause shown. The app suggests spare employees from other brands who can cover without leaving their own brand short. The manager decides whether to apply it.

**5. Conflicts and insufficient staffing.** Nothing is hidden. Mandatory issues (coverage, weekly-off rules) are listed separately from preference warnings. The Publish button refuses while mandatory issues are open. If no safe backup exists, the app says so and leaves the decision to the manager.

**6. Automated vs human.** Automated: baseline roster, gap detection, suggestions. Human: approving leave, applying cover, and publishing. Cover changes people's real work and needs a manager's judgment.

**7. If the company doubled.** I would move to a database with a backend, replace the greedy generator with a constraint solver (e.g. OR-Tools), and add roles, approvals and notifications.

**8. One more week.** Persistence, CSV/Excel export, fairer rotation of weekly offs, and a stricter 6-day streak rule.

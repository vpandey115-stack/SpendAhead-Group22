# SpendAhead
### Know What You Can Afford Before You Spend

**UCF · COP 5818 — Full-Stack Development in FinTech**  
**Final Project · Group 22**

**Team:** Vihan Pandey and Hiba El Harrak  
**Status:** Project proposal / planning. Application code is not yet implemented.

## Problem and unique value

A positive bank balance does not necessarily mean a discretionary purchase is affordable. Rent, utilities, and other bills may be due **before** the next paycheck, making a temporary cash shortfall easy to overlook.

SpendAhead is a **fictional, single-currency (USD) cash-flow simulator** for students and early-career workers. Users enter today's available cash, future bills and income, and a safety buffer. The app projects 30 days of cash flow and identifies the **lowest** projected balance, then checks a proposed purchase against that low point.

**Our USP:** Unlike a basic expense tracker, SpendAhead helps users make a **before-you-spend** decision based on *when* cash enters and leaves their budget, not just the current or end-of-month balance.

## Four essential features

1. **Financial plan:** Set an opening cash amount, a plan date and a protected minimum cash buffer.
2. **Cash-flow schedule:** Add, view, edit and delete upcoming fictional bills and expected income. Entries can be one-time or monthly recurring so regular bills do not have to be re-entered.
3. **30-day forecast:** View the projected daily balances, lowest balance, first buffer-breach date (if any) and safe-to-spend amount.
4. **Purchase check:** Enter a hypothetical purchase amount and receive a clear risk result without making or recording any real payment.

**Optional after core features are tested:** a simple timeline chart and manual paid-status tracking.  
**Out of scope:** bank connections, real payments, live financial APIs, user accounts, multi-currency, machine learning, public deployment, and mobile apps.

## Fictional example

| Date | Entry | Projected cash |
|---|---|---:|
| Day 0 | Opening available cash: $900 | $900 |
| Day 1 | Rent: -$750 | $150 |
| Day 3 | Expected paycheck: +$1,000 | $1,150 |
| Day 4 | Utilities: -$200 | $950 |

With a protected **$100** safety buffer:

- Lowest projected cash = **$150**
- Safe to spend today = `max(0, $150 − $100)` = **$50**
- Hypothetical $200 purchase lowers the forecast's lowest point to **−$50**, or **$150 below the buffer**.

This is an educational forecast based on **manually supplied fictional records**. The application does not know about unentered bills and does not guarantee real-world affordability.

## Planned stack

| Layer | Technology | Responsibility |
|---|---|---|
| Frontend | HTML, CSS, vanilla JavaScript | Forms, schedule, forecast and purchase-check views |
| Backend | Node.js, Express | Validation, REST endpoints and financial calculations |
| Data storage | File-backed SQLite | Persistent plans and cash-flow records |

All monetary values will be stored and calculated in **integer cents**, with JSON responses formatted in USD. The backend will use prepared statements and return appropriate HTTP errors.

## Proposed API

| HTTP method | Path | Purpose |
|---|---|---|
| GET | `/api/plan` | Read the current fictional plan |
| PUT | `/api/plan` | Create/update opening cash, buffer and start date |
| GET | `/api/events` | List scheduled bills and income |
| POST | `/api/events` | Add a cash-flow entry |
| PUT | `/api/events/:id` | Edit an entry |
| DELETE | `/api/events/:id` | Delete an entry |
| GET | `/api/forecast` | Recalculate the forecast from saved records |
| POST | `/api/check-purchase` | Simulate a purchase; do not persist it |

## Database design

- `plans`: `id` primary key; `start_date`; `starting_cash_cents`; `buffer_cents`.
- `cashflow_events`: `id` primary key; `plan_id` foreign key referencing `plans.id`; `label`; `kind` (`bill` or `income`); `amount_cents`; `due_date`; `repeat_rule` (`once` or `monthly`).
- Validate genuine calendar dates and sensible monetary input on the server. Store records in a local SQLite file that is not committed to GitHub.

## Team responsibilities

| Member | Main implementation work | Review and testing |
|---|---|---|
| **Vihan Pandey** | Browser views, input forms, API integration and user feedback | Review backend errors, forecast output and restart persistence |
| **Hiba El Harrak** | Express routes, SQLite database, validation and forecasting algorithm | Review frontend validation, API integration and calculations |

Both members implement and test important features. Work will be coordinated through GitHub Issues, short branches, commits and pull requests with cross-review before merging. Proposed availability is **3–4 flexible hours per member per week**, subject to both members' confirmation; schedule a short weekly check-in.

## Proposed timeline

- **Week 9:** Repository, README, tasks, data model and interface outline.
- **Week 10:** Database, backend and request validation.
- **Week 11:** Browser screens plus first fully persisted end-to-end workflow.
- **Week 12:** Forecast and hypothetical-purchase calculation.
- **Week 13:** Integration testing, usability and error messages.
- **Week 14 / final buffer:** Bug fixes, running instructions and demo preparation.

Adapt these targets to the final deadline in Webcourses.

## Initial acceptance tests

1. **Financial math:** $900 opening cash, $100 buffer and fictional Day 1/3/4 entries above yield a $150 minimum and $50 safe-to-spend amount.
2. **Purchase simulation:** A $200 hypothetical purchase yields a −$50 minimum and $150 shortfall without saving a transaction.
3. **Invalid input:** Negative amount, malformed date or unsupported event type gets a clear HTTP 400 and does not change the database.
4. **Browser-to-database and restart:** A bill entered in the browser appears after reload and persists when the server restarts.
5. **Missing record:** Editing an unknown event ID returns HTTP 404.

## Learning and collaboration

We will use [MDN JavaScript Date documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) and [SQLite date/time documentation](https://www.sqlite.org/lang_datefunc.html) to investigate monthly recurrence, valid calendar dates and daily aggregation.

Repository workflow: create an Issue for each feature; use a short feature branch; commit meaningful updates; open a pull request; have the other teammate review and approve before merging. Track each member's implementation and testing contributions.

**Repo preparation:** This starter intentionally contains no application code. Once development begins, add the Node project, SQLite schema/setup scripts, runnable tests and accurate setup instructions. Never commit real credentials, `.env`, `node_modules/` or local `.db` files.

**Academic integrity:** Follow the course's requirements concerning attribution and permitted AI assistance.

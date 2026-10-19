# SpendAhead

**Know What You Can Afford Before You Spend**

**Course:** COP 5818 — Full-Stack Development in FinTech  
**Team:** Group 22 — Vihan Pandey and Hiba El Harrak  
**Repository:** https://github.com/vpandey115-stack/SpendAhead-Group22.git

## Project Overview

SpendAhead is a single-currency cash-flow planning web application for students and early-career professionals. It helps users determine whether a hypothetical purchase could put their finances at risk before their next paycheck or other expected income.

Unlike a conventional expense tracker that primarily shows previous spending, SpendAhead projects daily cash balances for the upcoming 30 days and identifies the lowest projected balance. The application then assesses the impact of a proposed purchase against a user-defined cash safety buffer.

The project uses fictional financial information. No real money is transferred.

## Essential Features

1. **Financial plan:** Enter starting available cash, a planning date, and a minimum safety buffer.
2. **Upcoming cash-flow management:** Create, view, edit, and delete expected bills and income, including one-time and monthly entries.
3. **30-day forecast:** Calculate projected daily cash balances, the lowest balance, a safe-to-spend amount, and potential buffer shortfalls.
4. **Purchase simulation:** Test a purchase amount without recording a real payment and receive a warning if it would cause a projected buffer shortfall.

## Financial Calculation

The server orders expected income and bill occurrences by date and calculates the balance after each day. It includes today's starting balance when finding the minimum.

`safe_to_spend_cents = max(0, lowest_projected_balance_cents - safety_buffer_cents)`

For example, consider a fictional starting balance of $900, rent of $750 due tomorrow, a $1,000 paycheck due three days from now, utilities of $200 due the following day, and a protected cash buffer of $100.

| Stage | Projected Balance |
|---|---:|
| Starting cash | $900 |
| After rent | $150 |
| After paycheck | $1,150 |
| After utilities | $950 |

The lowest projected balance is **$150** and the maximum safe-to-spend amount is **$50**. A hypothetical $200 purchase today would reduce the lowest projected balance to **-$50**, which is **$150 below the safety buffer**.

The estimate depends on the fictional bills and income entered and is not a guarantee of actual affordability.

## Technology Stack

- **Frontend:** HTML, CSS, vanilla JavaScript
- **Backend:** Node.js and Express
- **Database:** File-backed SQLite
- **Development and collaboration:** Git and GitHub

Financial amounts are stored and calculated as integer cents. Express performs server-side validation and uses prepared SQLite statements.

## Planned REST Endpoints

| Method | Path | Responsibility |
|---|---|---|
| GET | `/api/plan` | Retrieve the current plan |
| PUT | `/api/plan` | Save or update starting cash, plan date, and buffer |
| GET | `/api/events` | Retrieve scheduled bills and income |
| POST | `/api/events` | Create a scheduled cash-flow entry |
| PUT | `/api/events/:id` | Update an entry |
| DELETE | `/api/events/:id` | Remove an entry |
| GET | `/api/forecast` | Retrieve a computed 30-day forecast |
| POST | `/api/check-purchase` | Calculate a hypothetical purchase outcome |

## Database Model

**plans:** `id` (primary key), `start_date`, `starting_cash_cents`, `buffer_cents`.

**cashflow_events:** `id` (primary key), `plan_id` (foreign key), `label`, `kind`, `amount_cents`, `due_date`, `repeat_rule`.

A financial event is either income or a bill. Amounts must be positive integers in cents, due dates must be valid calendar dates, and the recurrence rule must be `once` or `monthly`.

## Team Responsibilities

| Team Member | Development Lead | Shared Responsibility |
|---|---|---|
| Vihan Pandey | Frontend, forms, API integration, dashboard, and purchase-check interface | Integration testing, code review, and usability |
| Hiba El Harrak | Express backend, SQLite schema, input validation, and forecast calculations | Financial testing, code review, and persistence tests |

Both members will coordinate asynchronously and hold a short weekly check-in. Each member will implement and test a substantial part of the application.

## Project Milestones

| Target | Milestone |
|---|---|
| Week 9 | Finalize plan and database design, repository, and interface structure |
| Week 10 | Implement SQLite and backend validation |
| Week 11 | Connect forms and list views to the backend and persistent database |
| Week 12 | Complete forecasting and purchase simulation |
| Week 13 | Perform integration tests and resolve defects |
| Week 14 | Complete documentation, final verification, and demonstration preparation |

## Testing Plan

- Confirm financial calculations using fictional $900 starting cash and scheduled rent, income, and utilities.
- Confirm that a hypothetical $200 purchase produces the correct shortfall without creating a payment record.
- Reject invalid amounts, dates, and event types with clear errors.
- Verify that a new entry travels from the browser through Express to SQLite.
- Restart the application to confirm data persists; verify missing IDs receive a 404 response.

## GitHub Collaboration

Features will be managed through GitHub Issues. Development will occur on short feature branches with descriptive commits and pull requests reviewed by the other member before merging.

The repository will contain project source, database schema or setup scripts, dependency manifests, running instructions, and test instructions as development progresses. Local database files, `node_modules`, environment secrets, and credentials will not be committed.

## Scope and Learning Resources

Optional features include a simple chart and manual payment-status tracking. Real banking access, live market data, payments, accounts, mobile applications, and multiple currencies are outside the project scope.

The team will investigate reliable monthly recurring-date calculation using [MDN JavaScript Date documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) and [SQLite date and time documentation](https://www.sqlite.org/lang_datefunc.html).

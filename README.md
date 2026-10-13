# Personal Budget Management System

A Python desktop app that reads your bank statement CSV, sorts each transaction into a category, and shows where your money goes.

## Why it exists

This was a four-person team project for the course ECE 4574 (Fall 2025). The goal was a budgeting tool that works from the CSV files banks already give you, so there is no bank login to set up and your data stays on your computer.

## How it works

1. **Create an account** and log in. Accounts are stored in a local JSON file (`gui/data/users.json`).
2. **Upload a bank statement.** The CSV import understands Wells Fargo, Chase and Truist formats (`core/fileUpload.py`).
3. **Auto-categorize.** Each transaction is matched against keyword and regex rules in `data/rules.json`. The first matching category wins. You can change a category or add a note to any transaction by hand.
4. **Look at the results.** The Budget Analysis page has eight tabs:
   - Overview
   - Categories
   - Monthly Trends
   - Budget Goals
   - Income vs Spending
   - Forecast
   - Subscriptions
   - Loan Calculator
5. **Set limits and goals.** You can set monthly and weekly spending limits, a savings goal and per-category limits. The dashboard shows alerts and a streak count for staying on target.
6. **Export and support.** You can save a window as PNG or PDF. A support form sends an email to the developers through Gmail SMTP, using credentials you put in a `.env` file.

The forecast and loan calculator run on local data only. The loan calculator makes no external calls.

## How to run it

You need Python 3.9 or newer (the code uses `list[...]` type hints).

```bash
pip install -r requirements.txt
python -m gui.app
```

You can also run `python gui/app/main.py`.

To try the categorizer without the GUI, run the demo script, which uses made-up transactions:

```bash
python scripts/quick_demo.py
```

The support email feature needs `EMAIL_ADDRESS` and `EMAIL_PASS` in a `.env` file. The rest of the app works without them.

`gui/data/users.json` is sample data: made-up test accounts and transactions, kept so the app has something to show on first run.

## Results

This repo has no automated tests and no benchmarks, so there are no measured results to report.

Known limits:

- Passwords are stored as plain SHA-256 hashes with no salt. That is fine for a course project but not for a real product.
- `core/database/` contains MongoDB code (`pymongo`), but nothing in the GUI imports it. The app stores data in JSON.

## Team

Team 8 of ECE 4574.

- **Jason Huang** (Scrum Master)
- **Sheng Lu**
- **Ankush Chaudhary**
- **Luke Graham**

Who wrote what, based on the author lines in the files:

| Person | Files |
|---|---|
| Ankush Chaudhary | `gui/app/main.py`, `gui/app/models/user.py`, `gui/app/pages/` (login, dashboard, profile, transactions) |
| Sheng Lu | `core/analytics/` (spending, income, months, forecasting, loan), `core/view_spending.py`, loan tab |
| Luke Graham | `core/models.py`, `core/categorize_edit.py`, `scripts/` |
| Jason Huang | `core/database/`, `core/fileUpload.py`, `core/exportWin.py`, `core/supportMail.py` |

## Tech stack

Python, PyQt6, PyQt6-Charts, JSON file storage, python-dotenv. `pymongo` is listed in `requirements.txt` but the GUI does not use it.

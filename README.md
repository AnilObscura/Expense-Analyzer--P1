# 💰 Expense Analyzer - P1

> **A Full-Stack Personal Finance & Expense Analyzer built entirely from scratch.** 
> Plan monthly budgets, track expenses effortlessly (just 4 times a month), and let the system automatically analyze your wealth growth, financial score, and investment readiness. Also, make all the statistics and data visually represented using graphs and plots.

---

📑 Table of Contents
1. [Project Purpose](#-project-purpose)
2. [Project Screenshots](#project-screenshots)
3. [System Architecture & Design](#️-system-architecture--design)
4. [Core Logics](#core-logics)
5. [Tech Stack](#-tech-stack)
6. [Key Features & Components](#-key-features--components)
7. [Database Design (The Data Flow)](#️-database-design-the-data-flow)
8. [How to Run Locally](#-how-to-run-locally)
9. [Security & Error Handling](#️-security--error-handling)
10. [License](#-license)

---

## Project Purpose

Most finance apps fail because they require **daily** expense logging, which people usually forget to do. 
**Expense Analyzer** solves this with a **"Weekly Check-in" model**. Users only set their expected monthly plan at the start of the month, and then input their total weekly expense just **4 times a month**. The system handles the heavy lifting—calculating variance, predicting end-of-month totals, and adjusting wealth metrics.

---

## Project Screenshots

Here is a visual walkthrough of the application in action:

### 1. The Planning Phase (Home Dashboard)
Users start by selecting the month and entering their expected income sources and expense categories. This sets the financial baseline for the month.
![Empty Dashboard](https://github.com/AnilObscura/Expense-Analyzer--P1/blob/0057b70e1b9158452ff01ff0491b0642a86ee8b8/ss.1.PNG)

### 2. Instant Financial Feedback
Upon clicking "Calculate Budget", the backend engine runs complex logic (Wealth Carry-Forward, Financial Score formula). The user instantly sees their KPI cards update and a dynamic Gauge Chart displaying their financial health score.
![Dashboard Calculated](https://github.com/AnilObscura/Expense-Analyzer--P1/blob/0057b70e1b9158452ff01ff0491b0642a86ee8b8/ss.2.PNG)

### 3. Tracking Real-World Spending (Weekly Tracking)
Instead of daily logging, the user enters their actual spending just once a week. The system automatically calculates Deviations %, updates the Status (On Track / Overspending), and plots a real-time "Actual vs Expected" graph to visualize spending behavior.
![Weekly Tracking](https://github.com/AnilObscura/Expense-Analyzer--P1/blob/0057b70e1b9158452ff01ff0491b0642a86ee8b8/ss.3.PNG)

### 4. Deep Analytics & Trends (Reports)
The Reports page pulls aggregated data from the database to generate interactive Plotly charts. This includes Expense Trend Analysis (Planned vs. Actual), Savings Trends, and the highly visual Wealth Growth chart.
![Reports Trends](https://github.com/AnilObscura/Expense-Analyzer--P1/blob/0057b70e1b9158452ff01ff0491b0642a86ee8b8/ss.4.PNG)

### 5. Financial Intelligence & Investment Readiness
The system automatically analyzes your data to generate an Expense Breakdown Donut Chart, showing exactly where your money goes. It also calculates a 6-month Emergency Fund target, compares it with your actual wealth, and dynamically outputs your "Investment Capacity".
![Reports Investment](https://github.com/AnilObscura/Expense-Analyzer--P1/blob/0057b70e1b9158452ff01ff0491b0642a86ee8b8/ss.5.PNG)

---

## System Architecture & Design

This project follows a strict **3-Tier MVC (Model-View-Controller)** architecture with a **Service Layer** pattern. This ensures that the application is scalable, testable, and maintainable.

```text
[Browser] 
    ⬇
[Routes (Flask Blueprints)]  <-- Handles HTTP requests & returns JSON/HTML
    ⬇
[Services (Business Logic)]  <-- The "Brain" (Does ALL calculations here)
    ⬇
[Models (SQLAlchemy ORM)]    <-- Communicates with the SQLite Database
```

---

## Core Logics

These are the complex algorithmic systems built into the backend:

**💰 Wealth Carry-Forward Logic:**  
Instead of asking users to manually enter their starting wealth every month, the system automatically runs a chronological database query. It finds the *previous* month's `Expected Wealth`, and auto-fills it into the *current* month's `Starting Wealth` field.

**📊 Financial Score Formula (0 - 100):**  
The system dynamically calculates a financial health score based on 3 weighted parameters:
- **Savings Rate (60% weight):** `(Expected Savings / Total Income) * 100`
- **Expense Discipline (20% weight):** `(Total Expense / Total Income) * 100` (Inverted logic: lower expense ratio yields a higher score).
- **Wealth Growth (20% weight):** `((Current Wealth - Previous Wealth) / Previous Wealth) * 100`

The score is then mapped to a visual Plotly Gauge (Red: 0-40, Yellow: 40-60, Green: 60-100).

**📅 Forecasting & Deviation Engine:**  
When weekly actuals are entered, the system calculates `Deviation % = ((Actual - Expected) / Expected) * 100`. Based on deviation, it assigns a dynamic Status: `On Track`, `Warning`, `Overspending`, or `Under Budget`. The Forecasting engine predicts month-end expenses using: `Predicted Expense = Actual_So_Far * (4 / Weeks_Completed)`.

---

## Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Backend Framework** | Python, Flask |
| **Database ORM** | SQLAlchemy |
| **Database Engine** | SQLite (Lightweight, single-file DB) |
| **Frontend UI** | HTML5, CSS3, Bootstrap 5, Jinja2 |
| **Frontend Logic** | Vanilla JavaScript (ES6) |
| **Data Visualization** | Plotly.js |
| **Development Tools** | VS Code, Git, GitHub |

---

##  Key Features & Components

### 1. 🏠 Home Dashboard
- An interactive form to input Income Sources (Salary, Rental, Interest) and Expense Categories (Food, Rent, Travel, etc.).
- Automatically calculates and displays 4 KPI Cards: `Total Income`, `Planned Expense`, `Expected Savings`, and `Expected Wealth`.
- **Financial Score Gauge:** A custom Plotly Gauge Chart visualizing the calculated health score with color-coded zones.

### 2. 📅 Weekly Expense Tracking
- Automatically divides the monthly planned expense into 4 weekly cumulative targets (20%, 25%, 25%, 30% distribution).
- Allows users to input actual cumulative spending for each week.
- Generates a **Forecast Card** that updates in real-time to show predicted month-end expense and predicted savings.
- Includes an **Actual vs Expected Graph** to visualize spending trends over the month.

### 3. 📊 Reports & Analytics Engine
- **Expense Trend Analysis:** Line graph comparing Planned vs. Actual expenses month-over-month.
- **Savings Trend:** Tracks how savings are growing over the year.
- **Wealth Growth:** Visualizes the user's net worth evolving over time based on *actual* realized savings.
- **Expense Breakdown:** A Donut chart that aggregates spending by category across all months to show where money is going.
- **Investment Readiness Calculator:**
  - Calculates a 6-month Emergency Fund Target based on average monthly spending.
  - Calculates `Investment Capacity = Actual Wealth - Emergency Fund Target`.
  - Displays a dynamic status: `"Ready to Invest"` or `"Build Emergency Fund First"`.

---

## Database Design (The Data Flow)

The database uses **normalized SQLAlchemy Models** with strict relationships:

- **`users`:** Stores basic user info.
- **`monthly_budgets`:** Stores the main timeline. Has a `UNIQUE(user_id, month, year)` constraint to prevent duplicate budgets.
- **`income_sources` & `expense_categories`:** Linked to `budget_id`. Stores raw input data.
- **`weekly_tracking`:** Stores the 4 cumulative records per month. Auto-generated when a budget is calculated.
- **`monthly_forecasts`:** Stores predicted expenses/savings. Updates dynamically when new weekly data is entered.
- **`financial_notes`:** (Future expansion) Used in Reports to store personal goals.

---

## How to Run Locally

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/AnilObscura/Expense-Analyzer-P1.git
   cd Expense-Analyzer-P1
Create and Activate a Virtual Environment (Recommended):

# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate

# Install Dependencies:
pip install flask flask-sqlalchemy plotly

# Start the Application:
python app.py

Open in Browser:
Navigate to http://127.0.0.1:5000 in your web browser.

## Security & Error Handling
Backend Validations: All incoming JSON data is parsed with defaults (float(inc.get(key, 0))). If a user leaves a field empty, it automatically defaults to 0, preventing Python NoneType errors.

Atomic Transactions: The db.session.commit() and db.session.rollback() logic ensures that if any part of the saving process fails (e.g., generating weekly tracking), the entire operation reverts to a safe state.

Division by Zero Protection: Every mathematical calculation (like Savings Rate or Deviation %) explicitly checks if total_income > 0 or if expected > 0 before dividing, completely preventing backend crashes.

## License

This project is open-source under the MIT License.
Created with ❤️ by Anil Kumar Tanwar (AnilObscura).

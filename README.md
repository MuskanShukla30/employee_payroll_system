<h1 align="center">Employee Payroll System</h1>

<p align="center">
  <strong>A lightweight Flask application for managing employee records and calculating payroll.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3-blue?style=flat-square&logo=python" alt="Python 3" />
  <img src="https://img.shields.io/badge/Flask-web%20framework-black?style=flat-square&logo=flask" alt="Flask" />
  <img src="https://img.shields.io/badge/SQLite-database-003B57?style=flat-square&logo=sqlite" alt="SQLite" />
  <img src="https://img.shields.io/badge/Bootstrap-5.3.3-7952B3?style=flat-square&logo=bootstrap" alt="Bootstrap 5.3.3" />
</p>

---

## ✨ Overview

The **Employee Payroll System** is a small, browser-based payroll app built with Flask. An administrator can add employee details, view the employee list, review calculated net salaries, and remove records. Employee data is stored locally in SQLite.

## 🚀 Features

- 🔐 Admin login with a Flask session
- 👤 Add employee contact, department, joining date, salary, and absence details
- 📋 View employee records and net salary on a dashboard
- 🧮 Calculate allowances and deductions when an employee is added
- 🗑️ Delete an employee record
- 💾 Save employee data in a local SQLite database
- 📱 Bootstrap-based pages with custom CSS styling

## 🏗️ Architecture

```text
┌────────────────────────────┐
│ Browser                    │
│ Login · Add Employee       │
│ Employee Dashboard         │
└──────────────┬─────────────┘
               │ HTTP
┌──────────────▼─────────────┐
│ Flask application (app.py) │
│ Routes · Login session     │
│ Payroll calculation        │
└──────────────┬─────────────┘
               │ SQL
┌──────────────▼─────────────┐
│ SQLite (payroll.db)        │
│ Employee records           │
└────────────────────────────┘
```

## 🧰 Tech Stack

| Area | Technology |
|---|---|
| Server | Python · Flask |
| Database | SQLite (`sqlite3`) |
| UI | HTML · Bootstrap 5.3.3 · custom CSS |

## 📁 Project Structure

```text
employee_payroll_system/
├── app.py             # Flask routes, database setup, and payroll calculation
├── payroll.db         # SQLite data file (initialized by the app)
├── login.html         # Admin login page
├── dashboard.html     # Employee list page
├── add.html           # Add employee form
├── edit.html          # Empty placeholder; editing is not implemented
├── style.css          # Custom styles
├── logo.png           # Empty placeholder
└── requirements.txt   # Empty; Flask must currently be installed separately
```

Flask expects HTML files in a `templates/` directory and static assets such as CSS in a `static/` directory. The repository currently keeps these files at its root, so arrange them before starting the app:

```text
templates/
├── login.html
├── dashboard.html
└── add.html
static/
└── style.css
```

## 🧮 Payroll Calculation

The app calculates salary using the following rules when an employee is added:

```text
HRA               = basic salary × 20%
DA                = basic salary × 10%
Other deduction   = basic salary × 5%
Absence deduction = absent days × (basic salary ÷ 30)
Total deduction   = other deduction + absence deduction
Net salary        = basic salary + HRA + DA − total deduction
```

The absence calculation uses a 30-day month. The app stores the allowances, total deduction, absence days, and net salary with the employee record.

## 🔧 Installation & Local Setup

### Prerequisites

- Python 3 and pip
- A terminal in the project directory

### 1. Clone the repository

```bash
git clone https://github.com/MuskanShukla30/employee_payroll_system.git
cd employee_payroll_system
```

### 2. Create and activate a virtual environment (recommended)

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

macOS/Linux:

```bash
source .venv/bin/activate
```

### 3. Install Flask

`requirements.txt` is empty in the current repository, so install Flask directly:

```bash
python -m pip install Flask
```

### 4. Arrange the Flask templates and static file

Create `templates/` and `static/`, then move the three page files into `templates/` and `style.css` into `static/`, as shown in [Project Structure](#-project-structure). This matches the paths used by `render_template()` and `url_for('static', ...)` in the app.

### 5. Run the application

```bash
python app.py
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser. On startup, the app creates the `employee` table in `payroll.db` if it does not already exist. The database file is relative to the app's working directory.

## 🔑 Demo Login

The current credentials are hard-coded in `app.py`:

| Username | Password |
|---|---|
| `admin` | `admin123` |

These credentials and the built-in Flask secret key are for local demonstration only. Replace them and configure secrets securely before deployment. The Flask development server is not intended for production use.

## 📝 Current Limitations

- Employee editing is not implemented; `edit.html` is empty.
- The repository's HTML and CSS files need to be placed in Flask's `templates/` and `static/` folders for the app to locate them.
- `requirements.txt` does not yet list Flask or other dependencies.
- Authentication uses a single hard-coded demo account.
- The delete route does not check for an authenticated session.

---

<p align="center"><strong>Employee Payroll System</strong></p>

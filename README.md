<p align="center">
  <img src="https://raw.githubusercontent.com/MuskanShukla30/employee_payroll_system/main/logo.png" alt="Employee Payroll System Logo" width="120" />
</p>

<h1 align="center">Employee Payroll System</h1>

<p align="center">
  <strong>A lightweight, browser-based payroll management application built with Flask — automating salary computation, allowance breakdowns, and employee record management entirely on your local machine.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-blueviolet?style=flat-square" />
  <img src="https://img.shields.io/badge/python-3-3776AB?style=flat-square&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/flask-web%20framework-000000?style=flat-square&logo=flask&logoColor=white" />
  <img src="https://img.shields.io/badge/sqlite-database-003B57?style=flat-square&logo=sqlite&logoColor=white" />
  <img src="https://img.shields.io/badge/bootstrap-5.3.3-7952B3?style=flat-square&logo=bootstrap&logoColor=white" />
  <img src="https://img.shields.io/badge/license-MIT-22c55e?style=flat-square" />
</p>

---

## ✨ Overview

The **Employee Payroll System** is a small, self-hosted payroll application built with Python and Flask. It provides an admin-only interface for adding employee details, computing net salaries via a rule-based calculation engine, viewing an organized employee dashboard, and removing records — all backed by a local SQLite database with zero external dependencies.

Designed for simplicity and speed, the system removes the complexity of spreadsheets or expensive SaaS payroll tools for small teams and internal demos.

### Key Highlights

- 🔐 **Secure Admin Session** — Admin-only login powered by Flask session management keeps the dashboard gated and protected.
- 🧮 **Automated Salary Engine** — Instantly calculates HRA, DA, allowances, and absence-based deductions the moment an employee record is added.
- 📋 **Live Employee Dashboard** — A clean, sortable table displays every employee's full record and computed net salary at a glance.
- 💾 **Local SQLite Storage** — All employee and payroll data is stored in a single `payroll.db` file on your machine — no cloud, no tracking.
- 🗑️ **One-Click Record Deletion** — Remove employee entries directly from the dashboard.
- 📱 **Bootstrap-Powered UI** — Responsive, mobile-friendly pages styled with Bootstrap 5.3.3 and custom CSS.

---

## 🏗️ Architecture

The system is structured as a classic three-tier web application — browser, Flask server, and SQLite data layer:

```
┌──────────────────────────────────────────────────────┐
│                  Browser (Frontend)                  │
│  ┌────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  Login     │  │  Add         │  │  Employee    │  │
│  │  Page      │  │  Employee    │  │  Dashboard   │  │
│  └────────────┘  └──────────────┘  └──────────────┘  │
│          Bootstrap 5.3.3 · Custom CSS                │
└──────────────────────────┬───────────────────────────┘
                           │ HTTP (GET / POST)
┌──────────────────────────▼───────────────────────────┐
│             Flask Application  (app.py)              │
│  Routes · Admin Session · Payroll Calculation Engine │
└──────────────────────────┬───────────────────────────┘
                           │ SQL  (sqlite3)
┌──────────────────────────▼───────────────────────────┐
│                SQLite  (payroll.db)                  │
│             Employee Records · Payroll Data          │
└──────────────────────────────────────────────────────┘
```

### Tech Stack

| Layer | Technology |
|---|---|
| **Server** | Python 3 · Flask |
| **Database** | SQLite (`sqlite3` — built-in) |
| **Frontend** | HTML5 · Bootstrap 5.3.3 · Custom CSS |
| **Templating** | Jinja2 (via Flask) |
| **Session Auth** | Flask Session |

---

## 📁 Project Structure

```text
employee_payroll_system/
├── app.py               # Flask routes, DB setup, and payroll calculation engine
├── payroll.db           # SQLite data file (auto-created on first run)
├── login.html           # Admin login page
├── dashboard.html       # Employee records & net salary dashboard
├── add.html             # Add new employee form
├── edit.html            # Placeholder — editing not yet implemented
├── style.css            # Custom styles for all pages
├── logo.png             # Placeholder logo asset
└── requirements.txt     # Dependency list (see Installation for current state)
```

> [!IMPORTANT]
> Flask resolves HTML templates from a `templates/` folder and static assets from a `static/` folder by default. Before running the app, move the files as shown below to match Flask's expected paths:

```text
employee_payroll_system/
├── app.py
├── templates/
│   ├── login.html
│   ├── dashboard.html
│   └── add.html
├── static/
│   └── style.css
└── payroll.db
```

---

## 🚀 Features

### 🔐 Authentication

| Feature | Description |
|---|---|
| **Admin Login** | A dedicated login page with form-based credential submission, validated against a configured admin account. |
| **Flask Session Management** | Successful login creates a server-side session that gates access to all protected routes. |
| **Logout** | Clears the active session and redirects back to the login page. |

### 👤 Employee Management

| Feature | Description |
|---|---|
| **Add Employee** | A structured form captures the employee's name, contact info, department, designation, joining date, basic salary, and absence days. |
| **Employee Dashboard** | A full-width table listing all employee records with computed allowances, deductions, and final net salary columns. |
| **Delete Record** | Remove any employee entry from the database directly from the dashboard with a single action. |

### 🧮 Payroll Calculation Engine

| Feature | Description |
|---|---|
| **Automatic Computation** | All salary components are calculated server-side at the moment a new employee record is submitted. |
| **HRA & DA Allowances** | House Rent Allowance and Dearness Allowance are derived as percentages of the basic salary. |
| **Absence Deductions** | Absent days are automatically factored in and deducted from the net salary using a 30-day month model. |
| **Persistent Storage** | All computed values — allowances, deductions, absence count, and net salary — are stored alongside the employee record in SQLite. |

---

## 🧮 Salary Calculation Formula

The payroll engine applies the following rule-based formula automatically when an employee record is created:

```
Basic Salary        →  Entered at the time of adding the employee

HRA                 =  Basic Salary × 20%    (House Rent Allowance)
DA                  =  Basic Salary × 10%    (Dearness Allowance)

Other Deduction     =  Basic Salary ×  5%
Absence Deduction   =  Absent Days  × (Basic Salary ÷ 30)
                    ──────────────────────────────────────────────
Total Deduction     =  Other Deduction + Absence Deduction

Net Salary          =  Basic Salary + HRA + DA − Total Deduction
```

> [!NOTE]
> The absence calculation uses a fixed **30-day month model**. Daily rate = Basic Salary ÷ 30.

---

## 🔧 Installation & Local Setup

### Prerequisites

| Tool | Version | Purpose |
|---|---|---|
| [Python](https://www.python.org/downloads/) | 3.x | Runtime environment |
| pip | Latest | Package installation |
| A modern browser | Any | Access the web UI |

### 1. Clone the Repository

```bash
git clone https://github.com/MuskanShukla30/employee_payroll_system.git
cd employee_payroll_system
```

### 2. Create & Activate a Virtual Environment

```bash
python -m venv .venv
```

**Windows (PowerShell):**
```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**
```bash
source .venv/bin/activate
```

### 3. Install Flask

> [!NOTE]
> `requirements.txt` is currently empty in the repository. Install Flask directly:

```bash
python -m pip install Flask
```

### 4. Arrange Templates & Static Files

Move the HTML files into a `templates/` directory and `style.css` into a `static/` directory so Flask can resolve them correctly:

**Windows (PowerShell):**
```powershell
mkdir templates, static
Move-Item login.html, dashboard.html, add.html templates\
Move-Item style.css static\
```

**macOS / Linux:**
```bash
mkdir -p templates static
mv login.html dashboard.html add.html templates/
mv style.css static/
```

### 5. Run the Application

```bash
python app.py
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser.

On startup, the app automatically creates the `employee` table inside `payroll.db` if it does not already exist. The database file is written to the application's working directory.

---

## 🔑 Demo Login

The admin credentials are currently hard-coded inside `app.py`:

| Field | Value |
|---|---|
| **Username** | `admin` |
| **Password** | `admin123` |

> [!CAUTION]
> These credentials and the built-in Flask `SECRET_KEY` are intended for **local demonstration only**. Replace them and configure secrets securely through environment variables before any deployment. The Flask development server (`flask run`) is not designed for production traffic.

---

## 📝 Known Limitations

| Area | Status |
|---|---|
| **Employee Editing** | ❌ Not implemented — `edit.html` is an empty placeholder. |
| **File Placement** | ⚠️ HTML and CSS files must be manually moved to `templates/` and `static/` before running (see [Installation](#-installation--local-setup)). |
| **`requirements.txt`** | ⚠️ Currently empty — Flask must be installed separately. |
| **Authentication** | ⚠️ Single hard-coded admin account; no multi-user or role support. |
| **Delete Auth Check** | ⚠️ The delete route does not currently verify an active session before removing a record. |

---

## 🗺️ Roadmap

- [ ] 📝 Implement the employee edit / update flow
- [ ] 📋 Populate `requirements.txt` with all dependencies
- [ ] 🔑 Move credentials and `SECRET_KEY` to environment variables
- [ ] 🛡️ Add session check to the delete route
- [ ] 📄 Payslip PDF generation & download
- [ ] 📊 Analytics dashboard with salary distribution charts
- [ ] 👥 Multi-user support with role-based access (Admin / HR / Employee)
- [ ] 🐳 Docker support for one-command deployment

---

## 🤝 Contributing

Contributions, bug fixes, and feature requests are welcome!

1. **Fork** the repository
2. Create a feature branch — `git checkout -b feature/your-feature`
3. **Commit** your changes — `git commit -m "feat: add your feature"`
4. **Push** to your branch — `git push origin feature/your-feature`
5. Open a **Pull Request** describing your changes

Please follow [PEP 8](https://peps.python.org/pep-0008/) conventions and include inline comments for any new calculation logic.

---

## 📝 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  <strong>Built with ❤️ by <a href="https://github.com/MuskanShukla30">Muskan Shukla</a></strong><br/>
  <em>Simplifying payroll, one calculation at a time.</em>
</p>

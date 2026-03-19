# 💰 Expense & Income Tracker

> A full-stack personal finance web application built as an IBM Student Project — Yenepoya University, 2023–2026 BCA Batch.

Track your **expenses and income**, set **monthly budgets**, visualise your finances on an interactive **dashboard**, and get **AI-powered insights** — all running locally on your machine with one command.

---

## 📸 Screenshots

| Dashboard | Expenses | AI Insights |
|-----------|----------|-------------|
| _9 widgets including charts_ | _Full CRUD with filters_ | _11 smart rules_ |

> **IBM Reviewer:** Run `docker compose up --build` and open http://localhost:5173 to see the live application.

---

## ✨ Features

### Core features
- **Expense tracking** — add, edit, delete, and filter expenses by category, date, and amount
- **Income tracking** — record income by source (Salary, Freelance, Business, Investment, Gift, Other)
- **Budget management** — set monthly budgets per category, see actual vs budgeted spend
- **Net balance** — real-time view of income minus expenses with savings rate
- **CSV export** — download your expense or income data for any date range

### Dashboard (9 widgets)
- Total Spend This Month (with month-over-month change)
- Total Income This Month
- Net Balance + Savings Rate
- Spend by Category (pie chart)
- Budget vs Actual (bar chart)
- Monthly Trend — income and expenses over 6 months (line chart)
- Recent Expenses table
- Recent Income table
- AI Insights panel

### AI Insights engine (11 rules — no external API)
- Budget overspend alerts
- Burn rate warnings (80% of budget used)
- Month-over-month spending spikes
- Unusual expense detection
- Savings rate alerts
- Income vs expense balance warnings
- And 5 more...

---

## 🛠️ Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Backend | **FastAPI** (Python 3.11) | Auto Swagger docs, fast, Pydantic built-in |
| ORM | **SQLModel** | One class = DB table + validation schema |
| Database | **PostgreSQL 15** | Relational, ACID, IBM requirement |
| Frontend | **React 18 + Vite** | Component-based, fast build, wide ecosystem |
| Charts | **Recharts** | Built for React, all chart types needed |
| Deployment | **Docker + Compose** | One command to run everything |
| Version control | **GitHub + Sourcetree** | Cloud backup + visual Git management |

---

## 🚀 Quick Start

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop) — **this is all you need to install**

> No Python, Node.js, or PostgreSQL installation required. Docker handles everything.

### 1. Clone the repository
```bash
git clone https://github.com/[your-username]/expense-income-tracker.git
cd expense-income-tracker
```

### 2. Create your .env file
```bash
cp .env.example .env
```

Then open `.env` and set your database password:
```env
DATABASE_URL=postgresql://tracker_user:your_password@db:5432/tracker_db
POSTGRES_USER=tracker_user
POSTGRES_PASSWORD=your_password
POSTGRES_DB=tracker_db
VITE_API_URL=http://localhost:8000
```

> ⚠️ The password in `DATABASE_URL` must match `POSTGRES_PASSWORD` exactly.

### 3. Start the application
```bash
docker compose up --build
```

First run takes 3–5 minutes (downloading Docker images). After that, 10–30 seconds.

### 4. Open in your browser

| What | URL |
|------|-----|
| 🖥️ React Dashboard | http://localhost:5173 |
| 📖 API Documentation (Swagger) | http://localhost:8000/docs |
| ❤️ API Health Check | http://localhost:8000/health |

### 5. Load demo data (optional — recommended for review)
```bash
docker exec -it expense-income-tracker-backend-1 python scripts/seed_demo_data.py
```
This seeds 3 months of sample expenses, income, and budgets so the dashboard has data to show.

### Stop the application
```bash
docker compose down          # Stop — keeps your data
docker compose down -v       # Stop + wipe all data (fresh start)
```

---

## 🧪 Running Tests

```bash
# Run the full test suite (17 tests)
docker exec -it expense-income-tracker-backend-1 pytest -v
```

Expected output: **17 passed, 0 failed**

```
test_expenses.py::test_create_expense         PASSED
test_expenses.py::test_invalid_amount_422     PASSED
test_income.py::test_create_income            PASSED
test_income.py::test_invalid_source_422       PASSED
test_budgets.py::test_budget_status           PASSED
test_insights.py::test_overspend_triggers     PASSED
... (17 total)
============= 17 passed in 2.34s =============
```

---

## 📁 Project Structure

```
expense-income-tracker/
├── backend/
│   ├── app/
│   │   ├── main.py          # FastAPI app entry point
│   │   ├── models.py        # Database models (SQLModel)
│   │   ├── schemas.py       # Request/response schemas (Pydantic)
│   │   ├── routers/         # API endpoints (8 router files)
│   │   └── services/        # Business logic (6 service files)
│   ├── tests/               # pytest test suite (17 tests)
│   └── requirements.txt     # Python dependencies (pinned versions)
├── frontend/
│   ├── src/
│   │   ├── api/             # Axios API calls (6 files)
│   │   ├── components/      # Reusable React components
│   │   └── pages/           # Dashboard, Expenses, Income, Budgets, Reports
│   └── package.json
├── docs/                    # All 9 planning documents (.docx)
├── docker-compose.yml       # Orchestrates all 3 containers
├── .env.example             # Template for environment variables
└── README.md
```

---

## 📡 API Endpoints

All endpoints are available at `http://localhost:8000/api/v1/`

Full interactive documentation: **http://localhost:8000/docs**

| Resource | Endpoints |
|---------|-----------|
| Expenses | `GET POST /expenses` · `GET PUT DELETE /expenses/{id}` |
| Income | `GET POST /income` · `GET PUT DELETE /income/{id}` |
| Categories | `GET POST /categories` · `DELETE /categories/{id}` |
| Budgets | `POST GET /budgets` · `GET /budgets/status` |
| Balance | `GET /balance` · `GET /balance/trend` |
| Reports | `GET /reports/monthly-summary` · `/by-category` · `/trend` · `/savings-rate` |
| Insights | `GET /insights` |
| Export | `GET /export/expenses` · `GET /export/income` |

**Total: 24 endpoints** — all documented in Swagger UI.

---

## 🗄️ Database

4 tables in PostgreSQL:

```
expenses    — amount, category, description, notes, date
income      — amount, source, description, notes, date
categories  — name, is_default (9 seeded by default)
budgets     — category, amount, month, year (unique per period)
```

Data is stored in a Docker named volume (`pg_data`) and **persists across restarts**.

---

## 🔒 Security

- **Input validation** — Pydantic v2 validates every API request. Invalid data returns 422 with field-level errors.
- **SQL injection** — SQLModel ORM uses parameterised queries. No raw SQL string interpolation.
- **Secrets** — All credentials in `.env` (gitignored). Never hardcoded.
- **CORS** — Only requests from `localhost:5173` accepted. Other origins receive 403.
- **Dependency audit** — `pip-audit`: 0 CVEs found.

---

## 📅 Project Timeline

| Week | Dates | Focus |
|------|-------|-------|
| Week 1 | 9–15 Mar 2026 | All 9 planning documents |
| Week 2 | 16–22 Mar 2026 | Backend — all CRUD APIs |
| Week 3 | 23–29 Mar 2026 | Reports, AI insights, tests |
| Week 4 | 30 Mar–6 Apr 2026 | React frontend, final docs |
| Submission | By 10 Apr 2026 | IBM submission |

---

## 📄 Documentation

All planning documents are in the `/docs` folder:

| Document | Purpose |
|----------|---------|
| PRD v2.1 | Requirements — what to build and why |
| HLD v1.0 | System architecture and component design |
| LLD v1.0 | Detailed implementation design |
| Architecture & Tech Stack | Why each technology was chosen |
| Security Design | OWASP mapping, threat model, security controls |
| Database Design | ERD, schemas, indexes, normalisation |
| Proposal | Project justification for IBM review panel |
| Final Report | Complete project report |
| Viva Questions | 45 Q&A for IBM review preparation |
| Handover Document | Installation, troubleshooting, future work |

---

## 🗺️ Future Work (v2 planned)

- [ ] JWT authentication — multi-user support
- [ ] Recurring transactions — auto-generate monthly expenses/income
- [ ] Mobile responsive design
- [ ] CSV import from bank statements
- [ ] Cloud deployment (Hetzner/DigitalOcean + HTTPS)
- [ ] Alembic database migrations

---

## 👨‍💻 Author

**Sayu-V** — Yenepoya University, BCA 2023–2026

IBM Student Project Submission — April 2026

---

## 📜 Licence

This project is submitted as an academic project for Yenepoya University / IBM Student Programme.
See [LICENSE](LICENSE) for details.

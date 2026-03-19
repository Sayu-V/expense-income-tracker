# Changelog

All notable changes to the Expense & Income Tracker are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [v1.0.0] — 2026-04-10 — IBM Submission (Final Release)

### Summary
Final submission for IBM Student Project — Yenepoya University, 2023–2026 BCA Batch.
Complete full-stack application with all Must Have and Should Have features delivered.

### Added
- Complete React dashboard with 9 interactive widgets
- Income page — full CRUD with source filtering
- Budgets page — monthly budget management with status bars
- Reports page — all 6 analytics charts + CSV export
- End-to-end docker compose up --build validation on clean machine
- Final Report v1.0 and Handover Document v1.0 committed to /docs
- Demo seed data script (3 months of sample data)
- README.md with full quick-start guide

### Changed
- Dashboard upgraded: dual-line Monthly Trend chart now shows both income and expenses
- AI Insights panel: severity colour-coding added (red = alert, amber = warning, blue = info)

### Fixed
- Date display timezone bug: dates now parsed as local (not UTC) in React frontend
- Budget status query N+1 problem: replaced per-category loop with single LEFT JOIN query

---

## [v0.3.0] — 2026-03-29 — Analytics & AI Complete

### Summary
All backend analytics, AI insights engine, and test suite complete.
Phase 3 gate passed: 17/17 pytest tests passing, 0 failures.

### Added
- Reports endpoints: monthly-summary, by-category, income-by-source, trend, top-expenses, savings-rate
- Balance endpoints: /balance and /balance/trend (income minus expenses per month)
- AI Insights engine: all 11 rules implemented in insights_service.py
  - Budget overspend alert
  - Burn rate warning (80% of budget)
  - Month-over-month spike detection (30%+ increase)
  - Top spending category identification
  - Unusual expense detection (>2× category average)
  - Savings opportunity (consistently under budget)
  - Streak insight (no entries in 3+ days)
  - Savings rate alert (<10%) — NEW income rule
  - Income vs expense balance alert — NEW income rule
  - Best income month highlight — NEW income rule
  - No expenses yet reminder
- CSV export endpoints: /export/expenses and /export/income
- Pytest suite: 17 test cases across 4 test files (all passing)
- Security audit: pip-audit run — 0 CVEs found

### Changed
- insights_service.py: loads income data for income-aware rules
- report_service.py: monthly-summary now includes total_income and savings_rate

---

## [v0.2.0] — 2026-03-22 — Backend Foundation Complete

### Summary
All CRUD endpoints operational. Docker Compose working.
Phase 2 gate passed: all endpoints return correct responses in Swagger UI.

### Added
- Docker Compose setup: 3 containers (frontend, backend, db) with health checks
- PostgreSQL health check: db starts first, backend waits for service_healthy
- SQLModel models: Expense, Income, Category, Budget (all 4 tables)
- Pydantic schemas: Create, Update, Read variants for expenses and income
- Expense router: POST, GET, GET/{id}, PUT/{id}, DELETE/{id}
- Income router: POST, GET, GET/{id}, PUT/{id}, DELETE/{id} (v2.0 feature)
- Categories router: GET, POST, DELETE/{id}
- Budgets router: POST (upsert), GET, GET/status
- FastAPI startup event: create_all() + default category seeding (9 categories)
- Global exception handler: generic 500 response, full traceback logged server-side
- CORS middleware: localhost:5173 whitelist only
- Swagger UI: all endpoints documented at /docs
- conftest.py: TestClient with SQLite in-memory DB override for tests

### Fixed
- SQLModel session not closing: switched to context manager pattern in get_db()
- Docker startup order: backend health check dependency on db service_healthy added

---

## [v0.1.0] — 2026-03-15 — Documentation Phase Complete

### Summary
All 9 planning documents completed and committed to /docs.
Phase 1 gate passed: documentation complete before any code written.

### Added
- PRD v2.1 — requirements with income feature and 1-month timeline
- HLD v1.0 — system architecture, component diagram, Docker topology
- LLD v1.0 — full model definitions, schemas, service signatures, router contracts
- Architecture & Tech Stack v1.0 — 5 ADRs, comparison matrices, patterns
- Security Design v1.0 — STRIDE, OWASP Top 10, input validation, secrets strategy
- Database Design v1.0 — ERD, schemas, indexes, normalisation analysis
- Proposal v1.0 — project justification and SMART objectives for IBM panel
- GitHub repository created with main/dev branch structure
- .env.example committed with placeholder values
- .gitignore: excludes .env, __pycache__, node_modules, venv/
- Project folder structure created (frozen as per LLD)
- Gantt chart, development flowchart, GitHub strategy diagram (in PRD)

### Project setup
- Start date: 9 March 2026
- GitHub branching strategy: main / dev / feature/* / hotfix/*
- Commit convention: feat: / fix: / docs: / test: / chore:
- Sourcetree configured for visual branch management

---

## [Prototype] — Before March 2026 — Original (Deprecated)

The original prototype was built in one week without documentation or tests.
It has been superseded by this full rebuild. Key issues with the original:

- No income tracking (expenses only)
- No formal documentation (no PRD, HLD, or LLD)
- No test suite
- Inconsistent API structure
- No Docker containerisation
- No version control discipline

This changelog documents the rebuild only. The prototype codebase is not included in this repository.

---

*Maintained by Sayu-V — Yenepoya University — IBM Student Project 2026*

// Test Automation Framework (UI + API + E2E) //

A scalable and production-ready automation framework built using **Python, Playwright, and pytest**.

## 🔥 Features
- UI Automation (Playwright)
- API Automation (pytest + requests)
- End-to-End Testing (API + UI integration)
- Page Object Model (POM)
- CI/CD with GitHub Actions
- Allure Reporting
- Parallel Execution (pytest-xdist)

## 🧰 Tech Stack
- Python
- Playwright
- pytest
- requests
- Allure Reports

## ▶️ How to Run

```bash
pip install -r requirements.txt
playwright install
pytest -v
```

## 📊 Run with Allure Report

```bash
pytest --alluredir=allure-results
allure serve allure-results
```

## 🧪 Test Types
- UI Tests
- API Tests
- End-to-End Tests

## 💡 Example E2E Flow
- Create user via API
- Login via UI
- Validate dashboard

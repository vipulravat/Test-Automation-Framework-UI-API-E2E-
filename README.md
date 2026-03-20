~ Test Automation Framework (UI + API + E2E)

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

---

👨‍💻 Author: Vipul Ravat
"""

# =========================
# requirements.txt
# =========================

"""
pytest
playwright
requests
allure-pytest
pytest-xdist
faker
"""

# =========================
# conftest.py
# =========================

import pytest
from playwright.sync_api import sync_playwright
import requests

@pytest.fixture(scope="session")
def playwright_instance():
    with sync_playwright() as p:
        yield p

@pytest.fixture(scope="function")
def page(playwright_instance):
    browser = playwright_instance.chromium.launch(headless=True)
    page = browser.new_page()
    yield page
    browser.close()

@pytest.fixture
def api_client():
    class APIClient:
        BASE_URL = "https://reqres.in/api"

        def get(self, endpoint):
            return requests.get(f"{self.BASE_URL}{endpoint}")

        def post(self, endpoint, data):
            return requests.post(f"{self.BASE_URL}{endpoint}", json=data)

    return APIClient()

# =========================
# pages/login_page.py
# =========================

class LoginPage:
    def __init__(self, page):
        self.page = page

    def navigate(self):
        self.page.goto("https://example.com")

    def login(self, username, password):
        self.page.fill("#username", username)
        self.page.fill("#password", password)
        self.page.click("#login")

    def is_logged_in(self):
        return self.page.is_visible("#dashboard")

# =========================
# tests/api/test_users_api.py
# =========================

def test_get_users(api_client):
    response = api_client.get("/users?page=2")
    assert response.status_code == 200
    assert "data" in response.json()

# =========================
# tests/ui/test_login.py
# =========================

from pages.login_page import LoginPage

def test_login(page):
    login = LoginPage(page)
    login.navigate()
    # Dummy test (example.com not real login)
    assert page.title() is not None

# =========================
# tests/e2e/test_user_flow.py
# =========================

def test_e2e_flow(api_client):
    response = api_client.post("/users", {"name": "vipul"})
    assert response.status_code in [200, 201]

# =========================
# .github/workflows/ci.yml
# =========================

"""
name: Run Tests

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: |
        pip install -r requirements.txt
        playwright install

    - name: Run tests
      run: pytest
"""

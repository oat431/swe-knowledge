---
title: Test Framework Design
parent: Automation
topic: Building maintainable automation frameworks
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - test-automation
  - framework-design
---

# Test Framework Design

> **Core Principle:** A well-designed test framework separates concerns, promotes reuse, and makes tests easy to write, run, and maintain.

## What Test Framework Design Means

A test framework is:
- **Infrastructure:** Code that supports test execution
- **Structure:** Organization of tests and test code
- **Utilities:** Helper functions and abstractions
- **Standards:** Conventions for writing tests

Good framework design enables:
- Easy test creation
- Consistent test structure
- Reusable components
- Maintainable test code
- Fast test execution

## Framework Components

### 1. Test Runner

**Purpose:** Execute tests and report results

**Responsibilities:**
- Discover tests
- Execute tests in order
- Capture results (pass/fail/error)
- Generate reports (console, HTML, XML)
- Handle setup and teardown

**Examples:**
- Pytest (Python)
- JUnit (Java)
- Jest (JavaScript)
- NUnit (.NET)

**Key features:**
- Test filtering (by name, tag, directory)
- Parallel execution
- Retry failed tests
- Timeout handling

### 2. Test Structure

**Purpose:** Organize tests logically

**Common patterns:**

**By feature:**
```
tests/
├── login/
│   ├── test_valid_login.py
│   ├── test_invalid_login.py
│   └── test_account_lockout.py
├── checkout/
│   ├── test_guest_checkout.py
│   └── test_registered_checkout.py
└── user_profile/
    ├── test_update_profile.py
    └── test_change_password.py
```

**By test type:**
```
tests/
├── unit/
│   ├── test_calculator.py
│   └── test_validator.py
├── integration/
│   ├── test_api.py
│   └── test_database.py
└── e2e/
    ├── test_login_flow.py
    └── test_checkout_flow.py
```

**By layer (pyramid):**
```
tests/
├── unit/          # Fast, many
├── integration/   # Moderate
└── e2e/           # Slow, few
```

### 3. Page Object Pattern (UI Testing)

**Purpose:** Encapsulate UI interactions

**Problem without page objects:**
```python
def test_login():
    driver.get("https://example.com/login")
    driver.find_element(By.ID, "username").send_keys("user")
    driver.find_element(By.ID, "password").send_keys("pass")
    driver.find_element(By.ID, "login-btn").click()
    assert "Dashboard" in driver.title
```

**With page objects:**
```python
class LoginPage:
    def __init__(self, driver):
        self.driver = driver
        self.username_field = (By.ID, "username")
        self.password_field = (By.ID, "password")
        self.login_button = (By.ID, "login-btn")
    
    def open(self):
        self.driver.get("https://example.com/login")
    
    def login(self, username, password):
        self.driver.find_element(*self.username_field).send_keys(username)
        self.driver.find_element(*self.password_field).send_keys(password)
        self.driver.find_element(*self.login_button).click()
    
    def get_title(self):
        return self.driver.title

def test_login():
    login_page = LoginPage(driver)
    login_page.open()
    login_page.login("user", "pass")
    assert "Dashboard" in login_page.get_title()
```

**Benefits:**
- Reusable page interactions
- Tests read like business logic
- Easy to update when UI changes
- Separation of concerns

### 4. Test Data Management

**Purpose:** Provide test data reliably and efficiently

**Strategies:**

**Hardcoded data:**
```python
def test_login():
    login("testuser", "password123")
```
- Simple but inflexible
- Hard to maintain
- Limited scenarios

**Data files (CSV, JSON, YAML):**
```python
# test_data.csv
# username,password,expected
# user1,pass1,success
# user2,wrong,error

@pytest.mark.parametrize("username,password,expected", 
                         load_test_data("login_data.csv"))
def test_login(username, password, expected):
    result = login(username, password)
    assert result == expected
```
- Separates data from logic
- Easy to add scenarios
- Non-technical users can edit

**Database fixtures:**
```python
@pytest.fixture
def test_user(db):
    user = db.create_user(username="testuser", password="pass123")
    yield user
    db.delete_user(user.id)

def test_login(test_user):
    login(test_user.username, "pass123")
    assert is_logged_in()
```
- Realistic data
- Automatic cleanup
- Isolated tests

**Factory pattern:**
```python
class UserFactory:
    @staticmethod
    def create_valid_user():
        return User(
            username=f"user_{random_string()}",
            email=f"test_{random_string()}@example.com",
            password="ValidPass123!"
        )
    
    @staticmethod
    def create_user_with_invalid_email():
        user = UserFactory.create_valid_user()
        user.email = "invalid-email"
        return user

def test_registration():
    user = UserFactory.create_valid_user()
    register(user)
    assert user_exists(user.username)
```
- Flexible test data creation
- Reusable across tests
- Clear intent

### 5. Fixtures and Setup/Teardown

**Purpose:** Prepare and clean up test environment

**Pytest fixtures example:**
```python
@pytest.fixture(scope="session")
def browser():
    """Create browser instance once per test session"""
    driver = webdriver.Chrome()
    yield driver
    driver.quit()

@pytest.fixture(scope="function")
def page(browser):
    """Create fresh page for each test"""
    browser.get("https://example.com")
    yield browser
    browser.delete_all_cookies()

@pytest.fixture
def logged_in_user(page):
    """Login before test, logout after"""
    login_page = LoginPage(page)
    login_page.login("testuser", "password")
    yield
    logout()

def test_dashboard(logged_in_user):
    assert "Dashboard" in page.title
```

**Fixture scopes:**
- `session`: Once per test session
- `module`: Once per test file
- `class`: Once per test class
- `function`: Once per test (default)

**Benefits:**
- Reusable setup code
- Automatic cleanup
- Dependency injection
- Clear test dependencies

### 6. Assertions and Matchers

**Purpose:** Verify expected outcomes

**Basic assertions:**
```python
assert result == expected
assert "error" in message
assert len(items) > 0
```

**Rich assertions (Pytest):**
```python
assert response.status_code == 200
assert user.email == "test@example.com"
assert item in cart.items
```

**Custom matchers:**
```python
def assert_valid_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    assert re.match(pattern, email), f"Invalid email: {email}"

def test_user_email():
    user = create_user()
    assert_valid_email(user.email)
```

### 7. Reporting and Logging

**Purpose:** Provide test execution visibility

**Console output:**
```
test_login.py::test_valid_login PASSED
test_login.py::test_invalid_login PASSED
test_login.py::test_account_lockout FAILED
```

**HTML reports:**
- Test summary
- Pass/fail breakdown
- Execution time
- Failure details
- Screenshots (for UI tests)

**Logging:**
```python
import logging

logging.info(f"Testing login with username: {username}")
logging.error(f"Login failed: {error_message}")
logging.debug(f"Response: {response.text}")
```

## Framework Design Principles

### 1. Separation of Concerns

**Separate:**
- Test logic (what to test)
- Test infrastructure (how to test)
- Test data (inputs and expected outputs)
- Configuration (environment settings)

**Example:**
```python
# test_login.py (test logic)
def test_valid_login(login_page):
    login_page.login("user", "pass")
    assert login_page.is_dashboard_displayed()

# pages/login_page.py (infrastructure)
class LoginPage:
    def login(self, username, password):
        # UI interaction code

# data/login_data.yaml (test data)
valid_credentials:
  username: "testuser"
  password: "password123"

# config/test_config.yaml (configuration)
base_url: "https://staging.example.com"
timeout: 30
```

### 2. Reusability

**Create reusable components:**
- Page objects for UI
- API client classes
- Test data factories
- Utility functions

**Avoid:**
- Code duplication
- Copy-paste tests
- Hardcoded values

### 3. Maintainability

**Design for change:**
- Use abstractions (page objects, API clients)
- Avoid brittle selectors
- Parameterize tests
- Use meaningful names

**Example - brittle:**
```python
driver.find_element(By.XPATH, "/html/body/div[2]/form/input[1]")
```

**Example - maintainable:**
```python
driver.find_element(By.ID, "username")
```

### 4. Readability

**Tests should read like specifications:**
```python
def test_user_can_login_with_valid_credentials():
    given_user_exists("testuser", "password123")
    when_user_logs_in("testuser", "password123")
    then_user_sees_dashboard()
```

**Use clear naming:**
- Test names describe behavior
- Variables have meaningful names
- Functions do one thing

### 5. Reliability

**Tests should be:**
- Deterministic (same result every time)
- Independent (no test dependencies)
- Fast (quick feedback)
- Isolated (no side effects)

**Avoid:**
- Time-based waits (use explicit waits)
- Shared state between tests
- External dependencies (mock them)
- Order dependencies

## Framework Architecture Example

### Layered Architecture

```
┌─────────────────────────────────────┐
│         Test Cases                  │  What to test
├─────────────────────────────────────┤
│      Page Objects / API Clients     │  How to interact
├─────────────────────────────────────┤
│         Utilities / Helpers         │  Common functions
├─────────────────────────────────────┤
│      Test Runner / Framework        │  Execution engine
└─────────────────────────────────────┘
```

### Example Structure

```
automation/
├── tests/                    # Test cases
│   ├── test_login.py
│   ├── test_checkout.py
│   └── test_user_profile.py
├── pages/                    # Page objects
│   ├── login_page.py
│   ├── checkout_page.py
│   └── base_page.py
├── api/                      # API clients
│   ├── user_api.py
│   ├── order_api.py
│   └── base_api.py
├── utils/                    # Utilities
│   ├── test_data.py
│   ├── config.py
│   └── logger.py
├── fixtures/                 # Pytest fixtures
│   ├── conftest.py
│   └── browser_fixtures.py
├── data/                     # Test data
│   ├── users.csv
│   └── products.json
├── config/                   # Configuration
│   ├── test_config.yaml
│   └── environments/
├── reports/                  # Test reports
│   └── html/
└── requirements.txt          # Dependencies
```

## Framework Implementation Steps

### Step 1: Choose Tools

**Consider:**
- Programming language (team skills)
- Test runner (pytest, junit, jest)
- UI automation (selenium, cypress, playwright)
- API testing (requests, restassured)
- Reporting (allure, html reports)

### Step 2: Set Up Project Structure

**Create:**
- Directory structure
- Configuration files
- Base classes
- Sample tests

### Step 3: Build Core Components

**Implement:**
- Test runner configuration
- Base page objects
- API client classes
- Utility functions
- Fixtures

### Step 4: Write Sample Tests

**Create:**
- Smoke tests
- Critical path tests
- Demonstrate patterns

### Step 5: Document Standards

**Document:**
- How to write tests
- Naming conventions
- Code style
- Review process

### Step 6: Integrate with CI/CD

**Configure:**
- Test execution in pipeline
- Report generation
- Failure notifications

## Common Framework Patterns

### 1. Screenplay Pattern

**Actors perform tasks using abilities:**
```python
actor = Actor.named("User")
actor.can(BrowseTheWeb.using(browser))

actor.attempts_to(
    Go.to(login_page),
    Enter.the_text("user").into(username_field),
    Enter.the_text("pass").into(password_field),
    Click.on(login_button)
)

actor.should(See.the(dashboard_displayed))
```

### 2. Arrange-Act-Assert (AAA)

```python
def test_add_to_cart():
    # Arrange
    product = create_product(name="Widget", price=10.00)
    cart = ShoppingCart()
    
    # Act
    cart.add(product, quantity=2)
    
    # Assert
    assert cart.total == 20.00
    assert cart.item_count == 2
```

### 3. Given-When-Then (BDD)

```python
def test_user_registration():
    # Given a new user
    user_data = {"username": "newuser", "email": "new@example.com"}
    
    # When they register
    response = register_user(user_data)
    
    # Then account is created
    assert response.status_code == 201
    assert user_exists("newuser")
```

## Key Takeaways

1. **Separate concerns:** Test logic, infrastructure, data, configuration
2. **Use page objects:** Encapsulate UI interactions
3. **Manage test data:** Use fixtures, factories, or data files
4. **Design for maintainability:** Abstractions, clear names, DRY
5. **Follow patterns:** AAA, Given-When-Then, Screenplay

## Related Topics

- [[01_Automation_Strategy]]: What to automate
- [[03_Maintainable_Tests]]: Writing robust tests
- [[04_CI_CD_Integration]]: Running tests in pipelines

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/11_Test_Automation]]: Automation frameworks and patterns

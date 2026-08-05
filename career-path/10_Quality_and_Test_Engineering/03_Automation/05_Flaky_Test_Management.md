---
title: Flaky Test Management
parent: Automation
topic: Identifying, analyzing, and fixing unreliable tests
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - test-automation
  - flaky-tests
---

# Flaky Test Management

> **Core Principle:** Flaky tests erode trust in automation. Identify them systematically, quarantine them immediately, and fix them promptly.

## What Flaky Tests Are

Flaky tests produce inconsistent results:
- **Pass sometimes, fail sometimes** without code changes
- **Non-deterministic:** Different outcomes on different runs
- **Unreliable:** Cannot trust pass or fail results
- **Time-wasting:** Require re-runs and investigation

## Why Flaky Tests Are Dangerous

**Impact of flaky tests:**

| Problem | Consequence |
|---------|-------------|
| **Eroded trust** | Team ignores test failures |
| **Wasted time** | Re-running tests, investigating false failures |
| **Delayed feedback** | Slow pipeline due to retries |
| **Hidden defects** | Real failures dismissed as flaky |
| **Reduced confidence** | Uncertainty about code quality |
| **Team frustration** | Demoralizing to deal with unreliable tests |

**Cost calculation:**
- 100 flaky tests
- Each fails 5% of the time
- 10 minutes to investigate each failure
- 50 builds per week

**Weekly cost:** 100 × 5% × 50 × 10 min = 2,500 minutes = 42 hours

## Identifying Flaky Tests

### Symptoms

**Test is likely flaky if:**
- Fails intermittently without code changes
- Passes on retry
- Fails more often at certain times
- Fails on some machines but not others
- Failure message varies
- Timing-related failures

### Detection Methods

#### 1. Retry Analysis

**Track retry frequency:**
```python
# Pytest plugin for retry tracking
@pytest.mark.flaky(reruns=3, reruns_delay=2)
def test_suspicious():
    # Test code
    pass
```

**Monitor:**
- Tests that frequently retry
- Tests that pass on second attempt
- Tests with high retry count

#### 2. Historical Analysis

**Analyze test history:**
```bash
# Find tests with inconsistent results
pytest --cache-show

# Check test result history
pytest --lf  # Last failed
pytest --ff  # Failed first
```

**Look for patterns:**
- Tests that flip between pass/fail
- Tests that fail then pass without changes
- Tests with low pass rate (< 95%)

#### 3. Statistical Analysis

**Calculate flakiness score:**
```python
def calculate_flakiness(test_results):
    """
    test_results: list of 'pass' or 'fail' for last N runs
    """
    total_runs = len(test_results)
    pass_count = test_results.count('pass')
    fail_count = test_results.count('fail')
    
    # Flakiness = inconsistency in results
    if pass_count == 0 or fail_count == 0:
        return 0.0  # Consistent (all pass or all fail)
    
    # Higher flakiness when results are mixed
    return 1.0 - abs(pass_count - fail_count) / total_runs

# Examples
print(calculate_flakiness(['pass', 'pass', 'pass']))  # 0.0 (consistent)
print(calculate_flakiness(['pass', 'fail', 'pass']))  # 0.33 (somewhat flaky)
print(calculate_flakiness(['pass', 'fail', 'fail', 'pass']))  # 0.5 (very flaky)
```

#### 4. Isolation Testing

**Run test in isolation multiple times:**
```bash
# Run same test 10 times
for i in {1..10}; do
    pytest test_login.py::test_valid_login
done
```

**If results vary, test is flaky.**

### Flaky Test Categories

| Category | Description | Example |
|----------|-------------|---------|
| **Timing** | Race conditions, timeouts | Wait for element, async operations |
| **Resource leaks** | Memory, file handles, connections | Test passes first time, fails later |
| **Test order dependency** | Relies on state from previous test | Passes alone, fails in suite |
| **External dependencies** | Network, database, third-party API | Fails when service is slow |
| **Data issues** | Shared data, non-deterministic data | Fails with certain data |
| **Environment** | Machine-specific, time zone, locale | Fails on CI but not locally |

## Common Causes of Flaky Tests

### 1. Timing Issues

**Problem:**
```python
# Bad: Hard-coded wait
driver.find_element(By.ID, "submit").click()
time.sleep(2)  # Hope element appears within 2 seconds
driver.find_element(By.ID, "result").click()
```

**Solution:**
```python
# Good: Explicit wait
driver.find_element(By.ID, "submit").click()
wait = WebDriverWait(driver, 10)
result = wait.until(EC.element_to_be_clickable((By.ID, "result")))
result.click()
```

### 2. Resource Leaks

**Problem:**
```python
# Bad: No cleanup
def test_1():
    db.create_user("user1")
    # Test code...
    # user1 remains in database

def test_2():
    db.create_user("user1")  # Fails: user already exists
```

**Solution:**
```python
# Good: Cleanup in fixture
@pytest.fixture
def test_user(db):
    user = db.create_user(f"user_{uuid.uuid4()}")
    yield user
    db.delete_user(user.id)

def test_1(test_user):
    # Test code...

def test_2(test_user):
    # Test code...
```

### 3. Test Order Dependency

**Problem:**
```python
# Bad: Tests depend on order
def test_create_item():
    create_item("item1")
    assert item_exists("item1")

def test_delete_item():
    # Assumes item1 exists from previous test
    delete_item("item1")
    assert not item_exists("item1")
```

**Solution:**
```python
# Good: Each test is independent
@pytest.fixture
def existing_item():
    item = create_item(f"item_{uuid.uuid4()}")
    yield item
    if item_exists(item.id):
        delete_item(item.id)

def test_create_item():
    item = create_item("test_item")
    assert item_exists(item.id)
    delete_item(item.id)

def test_delete_item(existing_item):
    delete_item(existing_item.id)
    assert not item_exists(existing_item.id)
```

### 4. External Dependencies

**Problem:**
```python
# Bad: Relies on external service
def test_payment():
    result = process_payment(amount=100)  # Calls real payment API
    assert result == "success"
```

**Solution:**
```python
# Good: Mock external dependencies
def test_payment(mock_payment_api):
    mock_payment_api.return_value = "success"
    result = process_payment(amount=100)
    assert result == "success"
    mock_payment_api.assert_called_once_with(amount=100)
```

### 5. Non-Deterministic Data

**Problem:**
```python
# Bad: Random data causes failures
def test_search():
    query = random.choice(["python", "java", "ruby"])
    results = search(query)
    assert len(results) > 0  # Sometimes fails if no results
```

**Solution:**
```python
# Good: Controlled test data
def test_search():
    # Ensure test data exists
    create_article(title="Python Tutorial")
    create_article(title="Python Guide")
    
    results = search("python")
    assert len(results) >= 2
```

### 6. Environment Differences

**Problem:**
```python
# Bad: Assumes specific environment
def test_date_format():
    date = format_date("2024-01-15")
    assert date == "01/15/2024"  # Fails in non-US locale
```

**Solution:**
```python
# Good: Control environment or test appropriately
def test_date_format():
    with patch('locale.getlocale', return_value=('en_US', 'UTF-8')):
        date = format_date("2024-01-15")
        assert date == "01/15/2024"
```

## Flaky Test Management Process

### Step 1: Detection

**Automated detection:**
- Track test results over time
- Calculate flakiness scores
- Flag tests with inconsistent results
- Monitor retry frequency

**Manual detection:**
- Developer reports suspicious test
- Test passes on retry
- Intermittent failures in CI

### Step 2: Quarantine

**Immediately isolate flaky test:**
```python
import pytest

@pytest.mark.skip(reason="Flaky test - JIRA-1234")
def test_flaky_feature():
    # Test code
    pass
```

**Or move to separate suite:**
```python
@pytest.mark.flaky
def test_suspicious():
    # Test code
    pass
```

**Run flaky tests separately:**
```bash
# Run only stable tests
pytest -m "not flaky"

# Run flaky tests separately with retries
pytest -m flaky --reruns 3
```

### Step 3: Investigation

**Gather information:**
- Test failure logs
- Screenshots or videos
- Test environment details
- Recent code changes
- Test execution history

**Reproduce locally:**
```bash
# Run test multiple times
for i in {1..20}; do
    pytest test_flaky.py::test_suspicious -v
done
```

**Analyze patterns:**
- When does it fail?
- What's the failure message?
- Does it fail at specific times?
- Does it fail on specific machines?

### Step 4: Root Cause Analysis

**Common root causes checklist:**
- [ ] Timing issues (race conditions, insufficient waits)
- [ ] Resource leaks (database connections, file handles)
- [ ] Test order dependencies
- [ ] External service dependencies
- [ ] Shared test data
- [ ] Environment differences
- [ ] Parallel execution conflicts
- [ ] Insufficient cleanup

**Debugging techniques:**
- Add logging
- Run in isolation
- Run multiple times
- Check test dependencies
- Review recent changes

### Step 5: Fix

**Apply appropriate fix:**

| Root Cause | Fix |
|-----------|-----|
| Timing | Use explicit waits |
| Resource leaks | Add cleanup in fixtures |
| Order dependency | Make tests independent |
| External dependencies | Mock or stub |
| Shared data | Use unique data per test |
| Environment | Control environment or test appropriately |

**Verify fix:**
```bash
# Run test 50 times to verify stability
for i in {1..50}; do
    pytest test_fixed.py::test_previously_flaky
done
```

### Step 6: Verification

**Monitor after fix:**
- Track test results for 1-2 weeks
- Ensure pass rate > 99%
- Check for new failure patterns

**Remove from quarantine:**
```python
# Remove skip marker
def test_now_stable():
    # Test code
    pass
```

### Step 7: Prevention

**Prevent future flaky tests:**
- Code review for test quality
- Run tests multiple times before merging
- Monitor test stability metrics
- Educate team on flaky test causes
- Use static analysis to detect common issues

## Flaky Test Dashboard

### Metrics to Track

| Metric | Target | Alert Threshold |
|--------|--------|----------------|
| **Flaky test count** | 0 | > 10 tests |
| **Flaky test rate** | < 1% | > 5% |
| **Average retry count** | < 0.1 | > 0.5 |
| **Time to fix** | < 2 days | > 7 days |
| **Quarantine duration** | < 1 week | > 2 weeks |

### Dashboard Components

**Flaky test list:**
- Test name
- Flakiness score
- Last failure date
- Owner
- Status (investigating, fixing, fixed)

**Trend graphs:**
- Flaky test count over time
- Flakiness rate by team
- Time to fix distribution

**Top offenders:**
- Most flaky tests
- Teams with most flaky tests
- Common root causes

## Flaky Test Prevention

### 1. Test Design Guidelines

**Write stable tests:**
- Use explicit waits, not sleep
- Make tests independent
- Clean up after tests
- Mock external dependencies
- Use unique test data
- Control test environment

### 2. Code Review Checklist

**Review tests for stability:**
- [ ] No hard-coded waits
- [ ] Tests are independent
- [ ] Proper cleanup in fixtures
- [ ] External dependencies mocked
- [ ] Unique test data
- [ ] Environment controlled
- [ ] No test order dependencies

### 3. Pre-Merge Testing

**Run tests multiple times:**
```bash
# Run new tests 10 times before merging
for i in {1..10}; do
    pytest tests/new_feature/ -v
done
```

**Check for flakiness:**
- All runs pass? Good to merge
- Any failures? Investigate before merging

### 4. Continuous Monitoring

**Track test health:**
- Monitor pass rates
- Alert on flaky tests
- Dashboard for visibility
- Regular reviews

## Tools and Techniques

### 1. Pytest Plugins

**pytest-rerunfailures:**
```bash
# Rerun failed tests
pytest --reruns 3 --reruns-delay 2
```

**pytest-flakefinder:**
```bash
# Run tests multiple times to find flaky tests
pytest --flake-finder --flake-runs=10
```

### 2. Buildkite Test Analytics

**Features:**
- Flaky test detection
- Test duration tracking
- Failure analysis
- Trend visualization

### 3. Custom Tracking

**Track test results in database:**
```python
def record_test_result(test_name, result, timestamp):
    db.insert({
        'test': test_name,
        'result': result,
        'timestamp': timestamp,
        'build': os.environ['BUILD_ID']
    })

def get_flaky_tests(days=7):
    query = """
    SELECT test, COUNT(*) as runs, 
           SUM(CASE WHEN result = 'pass' THEN 1 ELSE 0 END) as passes
    FROM test_results
    WHERE timestamp > NOW() - INTERVAL %s DAY
    GROUP BY test
    HAVING passes > 0 AND passes < COUNT(*)
    """
    return db.execute(query, (days,))
```

## Key Takeaways

1. **Flaky tests are expensive:** They waste time and erode trust
2. **Detect systematically:** Use retry analysis, historical data, statistical methods
3. **Quarantine immediately:** Don't let flaky tests block development
4. **Fix root causes:** Address timing, resources, dependencies, data
5. **Prevent future flakiness:** Code reviews, guidelines, monitoring

## Related Topics

- [[03_Maintainable_Tests]]: Writing stable tests
- [[04_CI_CD_Integration]]: Handling flaky tests in pipelines
- [[06_ROI_Analysis]]: Measuring cost of flaky tests

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/11_Test_Automation]]: Dealing with flaky tests

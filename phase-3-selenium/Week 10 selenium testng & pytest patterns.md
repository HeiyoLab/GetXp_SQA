# Week 10 — Selenium TestNG / Pytest Patterns

**Phase:** 3 — Selenium Mastery  
**Tool focus:** Selenium + TestNG (Java) or pytest (Python)  
**Estimated time:** 3 hours

---

## Overview

A test file full of methods is not a test suite — it's a script. This week you structure your Selenium tests with a proper framework: annotations/decorators, data-driven execution, grouping, and basic reporting. These patterns are what hiring managers expect to see on your CV and in your code.

---

## Topics

### 1. TestNG Annotations (Java)

```java
public class LoginTest {
  private WebDriver driver;

  @BeforeSuite
  public void globalSetup() { /* run once before all tests */ }

  @BeforeClass
  public void classSetup() { /* run once before this class */ }

  @BeforeMethod
  public void setup() {
    driver = new ChromeDriver();
    driver.get("https://www.saucedemo.com");
  }

  @Test(groups = {"smoke", "login"}, priority = 1)
  public void loginWithValidCredentials() { ... }

  @Test(groups = {"regression"}, dependsOnMethods = {"loginWithValidCredentials"})
  public void addToCart() { ... }

  @AfterMethod
  public void teardown() { driver.quit(); }

  @AfterSuite
  public void globalTeardown() { /* cleanup */ }
}
```

### 2. pytest Fixtures & Markers (Python)

```python
import pytest
from selenium import webdriver

@pytest.fixture(scope="function")
def driver():
    d = webdriver.Chrome()
    d.get("https://www.saucedemo.com")
    yield d
    d.quit()

@pytest.mark.smoke
def test_valid_login(driver):
    ...

@pytest.mark.parametrize("username,password,expected", [
    ("standard_user", "secret_sauce", "inventory"),
    ("locked_out_user", "secret_sauce", "error"),
    ("bad_user", "wrong", "error"),
])
def test_login_scenarios(driver, username, password, expected):
    ...
```

Run by marker: `pytest -m smoke`

### 3. Data-Driven Testing

**Java — TestNG `@DataProvider`:**

```java
@DataProvider(name = "loginData")
public Object[][] loginData() {
  return new Object[][] {
    {"standard_user", "secret_sauce", true},
    {"locked_out_user", "secret_sauce", false},
    {"bad_user", "wrong_pass", false},
  };
}

@Test(dataProvider = "loginData")
public void testLogin(String user, String pass, boolean shouldPass) { ... }
```

**From external JSON (Java):**

```java
// Read test data from src/test/resources/test-data/login-data.json
String json = new String(Files.readAllBytes(Paths.get("src/test/resources/test-data/login-data.json")));
```

**Python — reading from CSV:**

```python
import csv

def read_csv(path):
    with open(path) as f:
        return list(csv.DictReader(f))

@pytest.mark.parametrize("row", read_csv("test-data/login.csv"))
def test_login(driver, row):
    ...
```

### 4. Grouping & Filtering

**TestNG XML:**

```xml
<!-- testng.xml -->
<suite name="Regression">
  <test name="LoginTests">
    <groups>
      <run><include name="smoke"/></run>
    </groups>
    <classes>
      <class name="tests.LoginTest"/>
    </classes>
  </test>
</suite>
```

### 5. Basic Reporting

**TestNG — built-in HTML report** in `test-output/index.html` after run.

**Allure with TestNG:**

```xml
<dependency>
  <groupId>io.qameta.allure</groupId>
  <artifactId>allure-testng</artifactId>
  <version>2.x.x</version>
</dependency>
```

**Allure with pytest:**

```bash
pip install allure-pytest
pytest --alluredir=allure-results
allure serve allure-results
```

---

## Resources

- [TestNG docs](https://testng.org/doc/documentation-main.html)
- [pytest docs — fixtures](https://docs.pytest.org/en/stable/reference/fixtures.html)
- [Allure TestNG](https://allurereport.org/docs/testng/)
- [Allure pytest](https://allurereport.org/docs/pytest/)

---

## Deliverable

> Build a **data-driven test suite** that tests the login flow with multiple credential sets.

Requirements:

1. Test the login flow with at least **5 credential sets** (2 valid, 3 invalid/edge cases)
2. Data stored externally in JSON or CSV — not in the test method itself
3. Each test run clearly logs which credential set passed/failed
4. Tests grouped with `@smoke` and `@regression` markers/groups
5. Allure report generated and screenshot taken of the results summary

---

## Evaluation Criteria

- [ ] 5 data sets tested — all produce correct pass/fail
- [ ] Data is in an external file, not the test code
- [ ] Tests use `@DataProvider` or `@pytest.mark.parametrize`
- [ ] Groups/markers work — can filter to just smoke or regression
- [ ] Allure report generated with test results
- [ ] No `Thread.sleep()` or `time.sleep()` anywhere

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 10 deliverable."_

---

_← [Week 09](./week-09-selenium-setup-core.md) · Back to [README](../README.md) · Next: [Week 11 →](./week-11-selenium-advanced.md)_

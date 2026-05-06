# Week 09 — Selenium Setup & Core Concepts

**Phase:** 3 — Selenium Mastery  
**Tool focus:** Selenium (Java or Python)  
**Estimated time:** 3 hours

---

## Overview

Selenium is still the dominant tool in Malaysian enterprise and large corporate QA teams. Many job descriptions list it as a requirement. This week you set up Selenium and replicate 5 tests from your Playwright suite — giving you a direct comparison between the two tools and a foundation for the rest of Phase 3.

Choose **Java (TestNG)** or **Python (pytest)** — stick with one language for all of Phase 3.

---

## Topics

### 1. WebDriver Setup

**Java (Maven):**

```xml
<!-- pom.xml -->
<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-java</artifactId>
  <version>4.x.x</version>
</dependency>
<dependency>
  <groupId>io.github.bonigarcia</groupId>
  <artifactId>webdrivermanager</artifactId>
  <version>5.x.x</version>
</dependency>
```

```java
WebDriverManager.chromedriver().setup();
WebDriver driver = new ChromeDriver();
```

**Python (pip):**

```bash
pip install selenium webdriver-manager pytest
```

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
```

> Selenium 4+ includes Selenium Manager which handles driver binaries automatically.

### 2. Element Location Strategies

```java
// Java
driver.findElement(By.id("user-name"));
driver.findElement(By.name("password"));
driver.findElement(By.className("btn_action"));
driver.findElement(By.cssSelector("[data-test='login-button']"));
driver.findElement(By.xpath("//input[@placeholder='Password']"));
driver.findElement(By.linkText("About"));
driver.findElement(By.partialLinkText("Sauce"));
```

```python
# Python
driver.find_element(By.ID, "user-name")
driver.find_element(By.CSS_SELECTOR, "[data-test='login-button']")
driver.find_element(By.XPATH, "//button[text()='Login']")
```

### 3. WebDriverWait & Expected Conditions

**Never use `Thread.sleep()` or `time.sleep()` for synchronisation.** Always use explicit waits:

```java
// Java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement button = wait.until(ExpectedConditions.elementToBeClickable(By.id("login-button")));
button.click();

wait.until(ExpectedConditions.visibilityOfElementLocated(By.className("error-message")));
wait.until(ExpectedConditions.urlContains("inventory"));
```

```python
# Python
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

wait = WebDriverWait(driver, 10)
button = wait.until(EC.element_to_be_clickable((By.ID, "login-button")))
button.click()
```

### 4. Browser Options

```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--headless=new");
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");
options.addArguments("--window-size=1920,1080");
WebDriver driver = new ChromeDriver(options);
```

### 5. Actions Class

```java
Actions actions = new Actions(driver);

// Hover
actions.moveToElement(element).perform();

// Right-click
actions.contextClick(element).perform();

// Drag and drop
actions.dragAndDrop(source, target).perform();

// Click and hold
actions.clickAndHold(element).moveByOffset(100, 0).release().perform();
```

---

## Resources

- [Selenium docs](https://www.selenium.dev/documentation/)
- [WebDriverManager GitHub](https://github.com/bonigarcia/webdrivermanager)
- [Selenium with Java tutorial — Guru99](https://www.guru99.com/selenium-tutorial.html)
- [Selenium with Python — official docs](https://selenium-python.readthedocs.io/)

---

## Deliverable

> Replicate **5 tests from your Playwright suite** using Selenium on SauceDemo. Document the differences.

Required tests (same scenarios as Week 5):

1. Login with valid credentials
2. Login with invalid credentials — assert error message
3. Add item to cart — assert badge count
4. Sort products
5. Checkout flow — complete purchase

Also write a `COMPARISON.md` in your deliverables folder noting:

- What was easier in Playwright vs Selenium
- What was harder
- Any behavioural differences you noticed

**Structure:**

```
deliverables/week-09-selenium-basics/
├── src/test/java/tests/ (or tests/)
├── pom.xml (or requirements.txt)
└── COMPARISON.md
```

---

## Evaluation Criteria

- [ ] All 5 tests pass without `Thread.sleep()` or `time.sleep()` anywhere
- [ ] Explicit waits used consistently
- [ ] At least one test uses `Actions` class
- [ ] `ChromeOptions` configured for headless execution
- [ ] `COMPARISON.md` contains at least 3 specific observations per tool
- [ ] Tests are independent — no shared driver state between test methods

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your repo link and reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 9 deliverable. Please evaluate my Selenium setup and tests."_

---

_← [Week 08](../phase-2-playwright/week-08-playwright-cicd-reporting.md) · Back to [README](../README.md) · Next: [Week 10 →](./week-10-selenium-testng-pytest.md)_

# Week 11 — Selenium Advanced Interactions

**Phase:** 3 — Selenium Mastery  
**Tool focus:** Selenium (Java or Python)  
**Estimated time:** 3 hours

---

## Overview

Enterprise applications are full of complex UI patterns — iFrames, shadow DOM, file uploads, dynamically loaded tables, drag-and-drop. This week tackles the scenarios that trip up most automation engineers and make tests flaky when handled incorrectly.

---

## Topics

### 1. iFrames

```java
// Java — switch into iframe
driver.switchTo().frame("frameName");           // by name/ID
driver.switchTo().frame(0);                     // by index
driver.switchTo().frame(driver.findElement(By.tagName("iframe")));  // by element

// Interact with element inside iframe
driver.findElement(By.id("inside-frame-element")).click();

// Always switch back when done
driver.switchTo().defaultContent();
driver.switchTo().parentFrame();
```

```python
# Python
driver.switch_to.frame("frameName")
driver.find_element(By.ID, "inside-frame-element").click()
driver.switch_to.default_content()
```

### 2. Shadow DOM

```java
// Java — use JavaScript to pierce shadow root
WebElement shadowHost = driver.findElement(By.cssSelector("my-component"));
SearchContext shadowRoot = shadowHost.getShadowRoot();
WebElement shadowChild = shadowRoot.findElement(By.cssSelector("#inner-button"));
shadowChild.click();
```

Note: Selenium 4+ has native shadow root support. For older browsers/versions, use `JavascriptExecutor`.

### 3. File Upload & Download

```java
// File upload — sendKeys with absolute path
WebElement uploadInput = driver.findElement(By.cssSelector("input[type='file']"));
uploadInput.sendKeys("/absolute/path/to/file.pdf");

// File download — configure download directory
Map<String, Object> prefs = new HashMap<>();
prefs.put("download.default_directory", "/path/to/downloads");
ChromeOptions options = new ChromeOptions();
options.setExperimentalOption("prefs", prefs);
```

```python
# Python — file upload
upload = driver.find_element(By.CSS_SELECTOR, "input[type='file']")
upload.send_keys("/absolute/path/to/file.pdf")
```

### 4. JavaScript Executor

When Selenium's standard methods fall short:

```java
JavascriptExecutor js = (JavascriptExecutor) driver;

// Scroll into view
js.executeScript("arguments[0].scrollIntoView(true);", element);

// Click when standard click is blocked by overlay
js.executeScript("arguments[0].click();", element);

// Read hidden attribute
String value = (String) js.executeScript("return arguments[0].getAttribute('data-value');", element);

// Scroll to bottom of page
js.executeScript("window.scrollTo(0, document.body.scrollHeight);");
```

### 5. Fluent Wait

More flexible than `WebDriverWait` — configure polling interval and ignore specific exceptions:

```java
FluentWait<WebDriver> wait = new FluentWait<>(driver)
  .withTimeout(Duration.ofSeconds(30))
  .pollingEvery(Duration.ofMillis(500))
  .ignoring(NoSuchElementException.class)
  .ignoring(StaleElementReferenceException.class);

WebElement result = wait.until(driver ->
  driver.findElement(By.id("async-result"))
);
```

### 6. Dynamic Tables

```java
// Count rows dynamically
List<WebElement> rows = driver.findElements(By.cssSelector("table tbody tr"));
System.out.println("Row count: " + rows.size());

// Find cell by column header value
WebElement headerRow = driver.findElement(By.cssSelector("table thead tr"));
List<WebElement> headers = headerRow.findElements(By.tagName("th"));
int priceColIndex = 0;
for (int i = 0; i < headers.size(); i++) {
  if (headers.get(i).getText().equals("Price")) { priceColIndex = i; break; }
}
String price = rows.get(0).findElements(By.tagName("td")).get(priceColIndex).getText();
```

---

## Resources

- [Selenium docs — iFrames](https://www.selenium.dev/documentation/webdriver/interactions/frames/)
- [Selenium docs — Shadow DOM](https://www.selenium.dev/documentation/webdriver/shadow_dom/)
- [Selenium docs — Waits](https://www.selenium.dev/documentation/webdriver/waits/)
- Practice sites:
  - [The Internet (iframe, file upload)](https://the-internet.herokuapp.com/)
  - [DemoQA](https://demoqa.com/) — dynamic tables, drag-drop

---

## Deliverable

> Write **3 tests** each handling a distinct complex scenario:

1. **iFrame test** — interact with at least 2 elements inside an iframe, then assert state outside the iframe
2. **File upload test** — upload a real file and assert a success message or the file name appears in the UI
3. **Dynamic table test** — traverse a table, find a specific row by value, and assert a cell in that row

Requirements:

- No `Thread.sleep()` / `time.sleep()` — use `FluentWait` for at least 1 test
- Use `JavascriptExecutor` for at least one operation
- Each test is fully isolated — no dependencies on other tests
- Use [The Internet](https://the-internet.herokuapp.com/) or [DemoQA](https://demoqa.com/)

---

## Evaluation Criteria

- [ ] All 3 tests pass reliably on repeated runs
- [ ] No `Thread.sleep()` anywhere — explicit or fluent waits only
- [ ] `FluentWait` with polling and exception ignoring used in at least 1 test
- [ ] `JavascriptExecutor` used in at least 1 test
- [ ] iFrame test switches context in and out correctly
- [ ] File upload test asserts something after upload (not just that click worked)
- [ ] Table test finds a row dynamically — not by hardcoded row index

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 11 deliverable."_

---

_← [Week 10](./week-10-selenium-testng-pytest.md) · Back to [README](../README.md) · Next: [Week 12 →](./week-12-selenium-crossbrowser-grid.md)_

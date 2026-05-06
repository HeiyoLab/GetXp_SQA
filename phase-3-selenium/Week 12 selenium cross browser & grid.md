# Week 12 — Selenium Cross-Browser & Grid

**Phase:** 3 — Selenium Mastery  
**Tool focus:** Selenium Grid · Docker  
**Estimated time:** 3–4 hours

---

## Overview

Testing on one browser in isolation is not enough. This week you run your suite across Chrome and Firefox, document any differences, and set up Selenium Grid with Docker so you can run parallel cross-browser tests from a single machine. This is a common setup in Malaysian enterprise QA teams.

---

## Topics

### 1. Driver Factory Pattern

Abstract browser creation so your tests don't care which browser they're running on:

```java
// DriverFactory.java
public class DriverFactory {
  public static WebDriver createDriver(String browser) {
    return switch (browser.toLowerCase()) {
      case "chrome" -> {
        ChromeOptions opts = new ChromeOptions();
        opts.addArguments("--headless=new", "--no-sandbox");
        yield new ChromeDriver(opts);
      }
      case "firefox" -> {
        FirefoxOptions opts = new FirefoxOptions();
        opts.addArguments("-headless");
        yield new FirefoxDriver(opts);
      }
      default -> throw new IllegalArgumentException("Unknown browser: " + browser);
    };
  }
}
```

In TestNG:

```java
@Parameters("browser")
@BeforeMethod
public void setup(String browser) {
  driver = DriverFactory.createDriver(browser);
}
```

In `testng.xml`:

```xml
<suite name="CrossBrowser" parallel="tests" thread-count="2">
  <test name="Chrome">
    <parameter name="browser" value="chrome"/>
    <classes><class name="tests.LoginTest"/></classes>
  </test>
  <test name="Firefox">
    <parameter name="browser" value="firefox"/>
    <classes><class name="tests.LoginTest"/></classes>
  </test>
</suite>
```

### 2. Remote WebDriver

Used to connect to Selenium Grid or cloud providers:

```java
URL gridUrl = new URL("http://localhost:4444/wd/hub");
ChromeOptions options = new ChromeOptions();
RemoteWebDriver driver = new RemoteWebDriver(gridUrl, options);
```

### 3. Selenium Grid with Docker

```yaml
# docker-compose.yml
version: "3"
services:
  selenium-hub:
    image: selenium/hub:latest
    ports:
      - "4444:4444"

  chrome:
    image: selenium/node-chrome:latest
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443

  firefox:
    image: selenium/node-firefox:latest
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
```

Start with:

```bash
docker-compose up -d
```

Grid console: `http://localhost:4444`

### 4. Cross-Browser Strategy

Not every test needs to run on every browser. Classify your tests:

- **All browsers** — critical business flows, checkout, login
- **Chrome only** — fast feedback during development
- **Full matrix** — regression suite before release

### 5. BrowserStack / LambdaTest (Cloud Alternative)

For cloud-based cross-browser testing:

```java
// BrowserStack example
String username = System.getenv("BROWSERSTACK_USER");
String accessKey = System.getenv("BROWSERSTACK_KEY");
URL url = new URL("https://" + username + ":" + accessKey + "@hub.browserstack.com/wd/hub");

ChromeOptions capabilities = new ChromeOptions();
HashMap<String, Object> bsOptions = new HashMap<>();
bsOptions.put("os", "Windows");
bsOptions.put("osVersion", "10");
bsOptions.put("browserVersion", "latest");
capabilities.setCapability("bstack:options", bsOptions);

WebDriver driver = new RemoteWebDriver(url, capabilities);
```

---

## Resources

- [Selenium Grid docs](https://www.selenium.dev/documentation/grid/)
- [Docker Hub — Selenium images](https://hub.docker.com/u/selenium)
- [BrowserStack Selenium](https://www.browserstack.com/selenium)

---

## Deliverable

> Run your Selenium test suite across **Chrome and Firefox** and produce a cross-browser test report.

Requirements:

1. `DriverFactory` class implemented — browser passed as parameter/env var
2. 5 tests run on both Chrome and Firefox (10 total runs)
3. Report distinguishes results by browser
4. Document any browser-specific differences in `CROSS-BROWSER-NOTES.md`
5. **Optional but recommended:** Set up Selenium Grid using Docker and run at least 1 test via RemoteWebDriver

---

## Evaluation Criteria

- [ ] `DriverFactory` pattern used — no browser-specific code in test classes
- [ ] Tests run on Chrome and Firefox
- [ ] Allure/HTML report shows browser breakdown
- [ ] `CROSS-BROWSER-NOTES.md` documents at least 2 differences observed (or confirms consistent behaviour)
- [ ] Tests run headless in both browsers
- [ ] Optional: Docker Grid running and RemoteWebDriver connecting to it

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 12 deliverable."_

---

_← [Week 11](./week-11-selenium-advanced.md) · Back to [README](../README.md) · Next: [Week 13 →](../phase-4-framework-architecture/week-13-framework-design.md)_

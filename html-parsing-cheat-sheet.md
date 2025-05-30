---

## 1. **Headless Browsers & Automation Frameworks**

### **Puppeteer** (Node.js)
- Headless Chrome/Chromium automation.
- Excellent for rendering JS-heavy pages.
- [GitHub](https://github.com/puppeteer/puppeteer)

### **Selenium** (Multiple languages)
- Automation for all major browsers.
- Can be run headless.
- [Website](https://www.selenium.dev/)

### **Playwright** (Node.js, Python, Java, .NET)
- Multi-browser support (Chromium, WebKit, Firefox).
- Feature-rich, modern alternative to Puppeteer.
- [GitHub](https://github.com/microsoft/playwright)

### **Nightmare.js** (Node.js)
- Headless automation for Electron.
- Simpler than Puppeteer/Playwright, but less maintained.
- [GitHub](https://github.com/segmentio/nightmare)

### **Cypress** (JavaScript)
- Primarily for end-to-end testing, but can be used to extract rendered HTML.
- [Website](https://www.cypress.io/)

Here is a list of popular **console (text-based) browsers** with the ability to dump the screen or page content:

---
## 2. **Console Browsers **

### 1. **w3m**
- **Description:** Text-based web browser with support for tables, frames, SSL, and images (in terminals with graphics support).
- **Dump screen:**  
  ```bash
  w3m -dump URL_or_file.html
  ```
- **Homepage:** [w3m](https://w3m.sourceforge.net/)

### 2. **lynx**
- **Description:** One of the oldest and most well-known text-based browsers. Highly configurable.
- **Dump screen:**  
  ```bash
  lynx -dump URL_or_file.html
  ```
- **Homepage:** [lynx](https://lynx.invisible-island.net/)

### 3. **links**
- **Description:** Text browser with support for HTML tables and frames. Has both text and graphics mode.
- **Dump screen:**  
  ```bash
  links -dump URL_or_file.html
  ```
- **Homepage:** [links](http://links.twibright.com/)

### 4. **elinks**
- **Description:** An advanced fork of links with more features and scripting support.
- **Dump screen:**  
  ```bash
  elinks -dump URL_or_file.html
  ```
- **Homepage:** [elinks](http://elinks.or.cz/)

### 5. **browsh**
- **Description:** Modern text-based browser that uses Firefox in the background. Renders complex JS-heavy pages as text/graphics in the terminal.
- **Dump screen (screenshot as text):**  
  ```bash
  browsh https://example.com
  ```
  *(No explicit "-dump" option, use with terminal output capture: e.g., `script` or `tee`)*
- **Homepage:** [brow.sh](https://www.brow.sh/)

### **Summary Table**

| Browser  | Dump Command Example                | Notes                     |
|----------|-------------------------------------|---------------------------|
| w3m      | `w3m -dump URL_or_file.html`        | Good for quick dumps      |
| lynx     | `lynx -dump URL_or_file.html`       | Highly scriptable         |
| links    | `links -dump URL_or_file.html`      | Simple and effective      |
| elinks   | `elinks -dump URL_or_file.html`     | More features than links  |
| browsh   | `browsh URL` (capture terminal out) | Best for JS-heavy pages   |

---

## 2. **Command Line Utilities**

### **htmlunit** (Java)
- Headless browser written in Java.
- Good for Java-based scraping and automation.
- [Website](https://htmlunit.sourceforge.io/)

### **PhantomJS** (JavaScript - DEPRECATED)
- Headless WebKit.
- No longer maintained, but still used.
- [Website](https://phantomjs.org/)

### **CasperJS** (JavaScript - DEPRECATED)
- Wrapper for PhantomJS for easier scripting.
- [Website](https://casperjs.org/)

### **Splash** (Python/Lua)
- Headless browser with HTTP API, built on Chromium.
- Excellent for use with Scrapy.
- [GitHub](https://github.com/scrapinghub/splash)

---

## 3. **Browser Automation via Scripting**

### **AppleScript + Safari/Chrome** (macOS only)
- Can script browsers on macOS to dump HTML.

### **AutoHotkey / AutoIt** (Windows)
- Windows scripting to automate browser actions.

---

## 4. **Other Notable Libraries/Tools**

### **BeautifulSoup + Selenium** (Python)
- Use Selenium to render, then BeautifulSoup to parse.

### **WebDriverIO** (Node.js)
- Selenium-based browser automation for Node.js.
- [Website](https://webdriver.io/)

### **TestCafe** (Node.js)
- Automation and testing for web apps.
- [Website](https://testcafe.io/)

### **Rod** (Go)
- DevTools driver for Chrome, written in Go.
- [GitHub](https://github.com/go-rod/rod)

### **chromedp** (Go)
- High-level Chrome DevTools Protocol client for Go.
- [GitHub](https://github.com/chromedp/chromedp)

### **Pyppeteer** (Python)
- Python port of Puppeteer.
- [GitHub](https://github.com/pyppeteer/pyppeteer)

### **undetected-chromedriver** (Python)
- Python package to bypass anti-bot detection.
- [GitHub](https://github.com/ultrafunkamsterdam/undetected-chromedriver)

---

## 5. **Cloud/Remote APIs**

### **Browserless**
- Hosted headless browser service (Puppeteer compatible).
- [Website](https://www.browserless.io/)

### **ScrapingBee, ScraperAPI, etc.**
- API services that can return rendered HTML.

---

## **Summary Table**

| Tool/Library            | Language(s)             | Headless | Maintained | JS Rendering | Link                                      |
|-------------------------|-------------------------|----------|------------|--------------|--------------------------------------------|
| Puppeteer               | Node.js                 | Yes      | ✓          | ✓            | https://github.com/puppeteer/puppeteer     |
| Selenium                | Many                    | Yes      | ✓          | ✓            | https://www.selenium.dev/                  |
| Playwright              | Node.js, Python, Java   | Yes      | ✓          | ✓            | https://github.com/microsoft/playwright    |
| Nightmare.js            | Node.js                 | Yes      | ~          | ✓            | https://github.com/segmentio/nightmare     |
| Cypress                 | JavaScript              | Yes      | ✓          | ✓            | https://www.cypress.io/                    |
| htmlunit                | Java                    | Yes      | ✓          | ✓            | https://htmlunit.sourceforge.io/           |
| PhantomJS               | JavaScript              | Yes      | ✗          | ✓            | https://phantomjs.org/                     |
| CasperJS                | JavaScript              | Yes      | ✗          | ✓            | https://casperjs.org/                      |
| Splash                  | Python/Lua              | Yes      | ✓          | ✓            | https://github.com/scrapinghub/splash      |
| WebDriverIO             | Node.js                 | Yes      | ✓          | ✓            | https://webdriver.io/                      |
| TestCafe                | Node.js                 | Yes      | ✓          | ✓            | https://testcafe.io/                       |
| Rod                     | Go                      | Yes      | ✓          | ✓            | https://github.com/go-rod/rod              |
| chromedp                | Go                      | Yes      | ✓          | ✓            | https://github.com/chromedp/chromedp       |
| Pyppeteer               | Python                  | Yes      | ✓          | ✓            | https://github.com/pyppeteer/pyppeteer     |
| undetected-chromedriver | Python                  | Yes      | ✓          | ✓            | https://github.com/ultrafunkamsterdam/undetected-chromedriver |
| Browserless/API         | Any (via HTTP)          | Yes      | ✓          | ✓            | https://www.browserless.io/                |

---

# [Playwright](https://playwright.dev) Cheat Sheet

## Installation

### Node (JavaScript / TypeScript)
```sh
npm install -D playwright
yarn add -D playwright
```
Install browsers
```sh
npx playwright install
npx playwright@latest install --with-deps

npx playwright init
```

### Python
```sh
pip3 install --break-system-packages playwright

# Install browsers (Chromium, Firefox)
playwright install

# install dependencies
playwright install-deps   # sudo apt-get install libavif16
```

## Minimal Project
### JavaScript
```js:scrape.js
import { chromium } from 'playwright';

(async () => {
  const browser = await chromium.launch(); // use { headless: false } to see UI
  const context = await browser.newContext();
  const page = await context.newPage();
  await page.goto('https://example.com');
  // ... actions ...
  await browser.close();
})();
```
emulate user environment
```js
const context = await browser.newContext({
  userAgent: 'MyBot/1.0',           // user-agent
  viewport: { width: 1280, height: 800 },
  locale: 'en-US',
  timezoneId: 'America/Los_Angeles',
  geolocation: { latitude: 37.7749, longitude: -122.4194 },
  permissions: ['geolocation'],     // allowed permissions for pages in this context
  extraHTTPHeaders: {               // custom request headers
    'x-my-header': 'some-value'
  },
  colorScheme: 'light',             // 'dark' | 'light' | 'no-preference'
  storageState: 'state.json'        // load cookies / localStorage from file
});
```


### Python
```py
from playwright.sync_api import sync_playwright
 
# Recommended way with a context manager (cleans up automatically)
# with sync_playwright() as p:
#     browser = p.chromium.launch(headless=False, slow_mo=50)
#     # ...
 
# Manual / inline way
p = sync_playwright().start()
browser = p.chromium.launch(headless=False, slow_mo=50)
context = browser.new_context()
page = context.new_page()
page.goto("https://example.com")
# ... actions ...
browser.close()
p.stop() # Important: you must manually stop playwright
```
emulate user environment 
```py
context = browser.new_context(
    user_agent="MyBot/1.0",
    viewport={"width": 1280, "height": 800},
    locale="en-US",
    timezone_id="America/Los_Angeles",
    geolocation={"latitude": 37.7749, "longitude": -122.4194},
    permissions=["geolocation"],
    extra_http_headers={"x-my-header": "some-value"},
    color_scheme="light",
    storage_state="state.json",
)
```
add cookies
```py
context.add_cookies([{
  "name": "sessionid",
  "value": "abc123",
  "domain": "example.com",
  "path": "/",
  "httpOnly": True,
  "secure": True
}])
```
or save object to 'state.json' 
```py
context.storage_state(path="state.json")
```

### Selectors quick reference
- CSS: `'#id'`, `'.class'`, `'div > a'`
- Text: `text=Login` (matches by visible text)
- Role: `role=button[name="Submit"]`
- XPath: `xpath=//button[text()="OK"]`
- nth match: `locator('a').nth(2)`

Prefer `locator()` API for reliability and auto-waiting.

### read data
```py
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    context = browser.new_context()
    page = context.new_page()
    page.goto("https://example.com")
    # Using page.text_content
    title = page.text_content("h1")
    print("h1 text:", title)
    # Using locator
    title_loc = page.locator("h1")
    print("h1 text (locator):", title_loc.text_content())
    browser.close()
```

### find button and click it
```py
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    context = browser.new_context()
    page = context.new_page()
    page.goto("https://example.com")
    # Simple click
    page.click("button#submit")
    # Or with locator
    btn = page.locator("button", has_text="Submit")
    btn.click()    
    # Use `page.wait_for_response(...)`, `page.wait_for_selector(...)`, or `locator.wait_for()` when asserting post-click behavior.
    page.wait_for_load_state("networkidle")
    browser.close()
```

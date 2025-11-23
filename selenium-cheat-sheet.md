j# Selenium cheat sheet
## links
* [selenium pip](https://www.selenium.dev/selenium/docs/api/py/)
* [selenium enter point](https://www.selenium.dev/)
* [selenium with virtual screen example ](http://github.com/cherkavi/python-utilitites/tree/master/selenium/)
* [selenium python scripts with additional parameters](http://github.com/cherkavi/python-utilitites/tree/master/selenium/README.md)

## installation

### Selenium with Firefox
#### Firefox 
```sh
# !!! do not install firefox via SNAP !!!
x-www-browser https://www.mozilla.org/en-US/firefox/linux/?utm_medium=referral&utm_source=support.mozilla.org
cd ~/Downloads
tar xjf firefox-*.tar.bz2

sudo mv firefox /opt
sudo ln -s /opt/firefox/firefox /usr/local/bin/firefox
```

#### geko driver for Firefox 
```sh
function gecko-driver-update(){
    GIT_ACCOUNT=mozilla
    GIT_PROJECT=geckodriver
    download_url=`curl -s https://api.github.com/repos/$GIT_ACCOUNT/$GIT_PROJECT/releases/latest | jq -r '.[]' | grep browser_download | grep "linux64" | grep -v "asc" | awk -F ": " '{print $2}' | sed s/\"//g` 
    wget $download_url -O gekodriver.tar.gz
    tar -xf gekodriver.tar.gz -C "${HOME_SOFT}/selenium_driver"
    rm gekodriver.tar.gz
}

if [ ! -f $HOME_SOFT"/selenium_driver/geckodriver" ]; then
    echo "download gecko driver from last release"
    mkdir "${HOME_SOFT}/selenium_driver"
    gecko-driver-update
    $GECKO_DRIVER --version
fi
```

### Selenium with Chrome
#### [install chrome](https://www.google.com/chrome/)

#### [chrome driver for Chrome](https://googlechromelabs.github.io/chrome-for-testing)
installed chrome and chrome driver must have the same version !!!
```
google-chrome --new-widnow "chrome://settings/help"  
google-chrome https://googlechromelabs.github.io/chrome-for-testing/#stable  
```


### [selenium](https://www.selenium.dev/downloads/)
```sh
pip3 install selenium
pip3 list | grep selenium

# update selenium
pip3 install -U --break-system-packages selenium
```


## [minimal example](https://www.selenium.dev/documentation/webdriver/troubleshooting/upgrade_to_selenium_4/)
```sh
python3
```
```python
# connect to driver
from selenium import webdriver
from selenium.webdriver import DesiredCapabilities
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.common.by import By

## pip install webdriver_manager # auto install of the driver, if not has found
# from webdriver_manager.firefox import GeckoDriverManager
# service = Service(executable_path=GeckoDriverManager().install())


PATH_TO_FIREFOX_DRIVER="/home/soft/selenium-drivers/gekodriver-mozilla"

options = webdriver.FirefoxOptions()
options.log.level = "trace"
# !!! don't use it - hangs forever
# options.add_argument("--profile=/tmp/my-profile")

# options.add_argument("-v")
# options.add_argument("-headless")   # options.headless = True
# options.add_argument('--no-sandbox')
# options.add_argument('--disable-dev-shm-usage')
# options.binary_location='/opt/firefox/firefox'

## https://www.browserstack.com/docs/automate/selenium/firefox-profile
# options.set_preference("browser.shell.checkDefaultBrowser", False)      

service = Service(executable_path=PATH_TO_FIREFOX_DRIVER)
driver = webdriver.Firefox(service=service, options=options)  # pgrep -laf firefox
# print(driver.capabilities["browserVersion"])

# open html page
page=driver.get('https://www.web2pdfconvert.com')

# find input element on the page
# https://www.selenium.dev/documentation/webdriver/elements/interactions/

element=driver.find_element(By.CLASS_NAME,"js-url-input");
# enter text into the element
element.send_keys("http://");
# click on element
driver.find_element(By.CLASS_NAME,"js-convert-btn").click()

driver.find_element(By.CLASS_NAME,"js-download-btn").click()
driver.quit()
```

## shadow dom example
```python
from selenium import webdriver
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import sys
import time


url='https://your domain'
webdriver_path='/home/soft/selenium_driver/geckodriver'

driver = webdriver.Firefox(service=Service(webdriver_path))

driver.get(url)

## direct detection will not work 
# button = driver.find_element(By.XPATH, "//button[text()='Alle erlauben']")
button.click()
driver.execute_script('return arguments[0].click()', button)

## algorithm
# 1. Get the host element
host_element = driver.find_element(By.ID, "usercentrics-root")

# 2. Get the shadow_root
shadow_root = driver.execute_script('return arguments[0].shadowRoot', host_element)   # host_element/shadowRoot
# driver.execute_script('return arguments[0].shadowRoot.innerHTML', host_element)

# 3. Find the element within the shadow root and interact with it
#    Warning: Shadow DOMs do not support XPath queries
button_inside_shadow = shadow_root.find_element(By.CSS_SELECTOR, '[data-testid="uc-accept-all-button"]')

# 4. usage of the element after detection 
button_inside_shadow.click()
```
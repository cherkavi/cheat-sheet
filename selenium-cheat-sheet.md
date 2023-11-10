# Selenium cheat sheet
## links
* [pip](https://www.selenium.dev/selenium/docs/api/py/)
* [enter point](https://www.selenium.dev/)
* [virtual screen example: ](http://github.com/cherkavi/python-utilitites/tree/master/selenium/)

## installation
### Firefox
```sh
# !!! do not install firefox via SNAP !!!
x-www-browser https://www.mozilla.org/en-US/firefox/linux/?utm_medium=referral&utm_source=support.mozilla.org
cd ~/Downloads
tar xjf firefox-*.tar.bz2

sudo mv firefox /opt
sudo ln -s /opt/firefox/firefox /usr/local/bin/firefox

```
### [selenium](https://www.selenium.dev/downloads/)
```sh
pip3 install selenium
pip3 list | grep selenium
```
### geko driver
```sh

GIT_ACCOUNT=mozilla
GIT_PROJECT=geckodriver
RELEASE_FILE_BEFORE_TAG="geckodriver-"
RELEASE_FILE_AFTER_TAG="-linux64.tar.gz"

release_url=https://github.com/${GIT_ACCOUNT}/${GIT_PROJECT}/releases/latest
latest_release_url=$(curl -s -I -L -o /dev/null -w '%{url_effective}' $release_url)
release_tag=`echo $latest_release_url | awk -F '/' '{print $NF}'`
release_file=$RELEASE_FILE_BEFORE_TAG$release_tag$RELEASE_FILE_AFTER_TAG

release_download="https://github.com/${GIT_ACCOUNT}/${GIT_PROJECT}/releases/download/${release_tag}/$release_file"

# Create a directory for downloads
output_dir="/home/soft/selenium-drivers"
file_name=gekodriver-mozilla
# Create the output directory if it doesn't exist
mkdir -p "$output_dir"
# Download the latest release
curl -L -o $output_dir/${file_name}.tar.gz "$release_download"

tar -xzf $output_dir/${file_name}.tar.gz -C $output_dir
mv $output_dir/geckodriver $output_dir/$file_name
chmod +x $output_dir/$file_name
$output_dir/$file_name --version
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
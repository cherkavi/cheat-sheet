# firefox browser

## firefox marionette
```sh
firefox --marionette
```

```python
# pip3 install marionette-driver

from marionette_driver.marionette import Marionette

marionette = Marionette(host='localhost', port=2828)
marionette.start_session()

# Open a webpage
marionette.navigate('https://amazon.de')

# | ID                    | marionette.find_element('id', 'element_id')                      |
# | Name                  | marionette.find_element('name', 'element_name')                  |
# | Class Name            | marionette.find_element('class name', 'class_name')              |
# | Tag Name              | marionette.find_element('tag name', 'div')                       |
# | CSS Selector          | marionette.find_element('css selector', 'div.class_name')        |
# | XPath                 | marionette.find_element('xpath', '//div[@class="class_name"]')   |
# | Link Text             | marionette.find_element('link text', 'Click Here')               |
# | Partial Link Text     | marionette.find_element('partial link text', 'Click')            |
# | Accessibility ID      | marionette.find_element('accessibility id', 'accessibility_id')  |
element = marionette.find_element('id', 'twotabsearchtextbox')
element.send_keys("hello")
print(element.text)

# Close the session
marionette.delete_session()
```

## firefox profile list
```sh
cat $HOME/.mozilla/firefox/profiles.ini
```

## firefox profile activate
```sh
plugin_path=$(cat $HOME/.mozilla/firefox/profiles.ini | grep Path | head -n 1 | awk -F '=' '{print $2}')
firefox --profile $plugin_path
```

## prevent to open in private mode
```sh
sudo mkdir -p /etc/firefox/policies
echo '
{
  "policies": {
    "DisablePrivateBrowsing": true
  }
}
' >> /etc/firefox/policies/policies.json
```

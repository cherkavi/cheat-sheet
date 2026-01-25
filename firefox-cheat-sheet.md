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


## firefox create extension

### firefox developer edition
1. Install [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/).
2. Type `about:config` in the address bar and click through the warning.
3. Search for: `xpinstall.signatures.required`.
4. Double-click it to set it to **false**.
5. Now, any local `.xpi` file you install will remain active after a restart.

### minimal set of files

```js:content.js
(function() {
    const box = document.createElement('div');
    box.innerHTML = "<b>hello</b>";
    document.body.appendChild();
})
```

```sh
# generate id 
uuidgen

# 0ee3ef96-92c9-4486-ae32-bf42c2e814b4
```

```json:manifest.json
{
  "manifest_version": 2,
  "name": "Some info",
  "version": "1.0",
  "applications":{
    "gecko": {
        "id": "{0ee3ef96-92c9-4486-ae32-bf42c2e814b4}"
    }
  },
  "description": "Finds and displays phrases like 'baujahr 1985' on the page.",
  "permissions": [],
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"],
      "run_at": "document_idle"
    }
  ]
}
```
create XPI file extension (for firefox installation) as a development package
```sh
zip 1.zip *
mv 1.zip '{0ee3ef96-92c9-4486-ae32-bf42c2e814b4}.xpi'
```

### plugin installation
#### temporary
```sh
firefox about:debugging#/runtime/this-firefox
```
#### via UI
```sh
firefox about:config
```
```properties
xpinstall.signatures.required=false
```
```sh
about:addons
```
#### copy to folder
```sh
locate firefox | grep '.mozilla' | grep extensions$

# $HOME/snap/firefox/common/.mozilla/firefox/{profile-id}.{profile-name}/extensions/
cp '{0ee3ef96-92c9-4486-ae32-bf42c2e814b4}.xpi' ${FIREFOX_EXTENSIONS_FOLDER}
```
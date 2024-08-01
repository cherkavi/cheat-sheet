# google chrome cheat sheet

## how to install 
```sh
x-www-browser https://www.google.com/chrome/thank-you.html?brand=CHBD&statcb=0&installdataindex=empty&defaultbrowser=0
# remove chrome cache: 
rm -rf ~/.cache/google-chrome
rm -rf ~/.config/google-chrome
sudo dpkg -i ~/Downloads/google-chrome-stable_current_amd64.deb
```

## [internal links](chrome://chrome-urls/)
> firefox system links `about:about`
* [system settings ](chrome://system/)  
* [applications](chrome://apps/)
* [extensions](chrome://extensions-internals/)

## extensions folder
```sh
# chrome
cd $HOME/.config/google-chrome/Default/Extensions/
# chromium
cd $HOME/snap/chromium/common/chromium/Default/Extensions/
```

find names of all installed extensions with path to them
```sh
EXT_PATH=$HOME/snap/chromium/common/chromium/Default/Extensions/
for each_file in `find $EXT_PATH -name "manifest.json"`; do
    echo $each_file
    cat $each_file | grep '"name": '
done
```

alternative way of finding names of all installed plugins/extensions
```sh
CHROME_CONFIG=$HOME/.config/google-chrome
IFS=$'\n'
for each_file in `find $CHROME_CONFIG | grep -i Extensions | grep manifest.json$`; do
    echo $each_file
    cat $each_file | grep '"name": '
done
```
## application 
```sh
## folder
ll "$HOME/snap/chromium/common/chromium/Default/Web Applications/Manifest Resources/"

## start application
/snap/bin/chromium --profile-directory=Default --app-id=cifhbcnohmdccbgoicgdjpfamggdegmo
```

## link anchor, link to text, highlight text on the page, find text on the page, [text fragments](https://developer.mozilla.org/en-US/docs/Web/Text_fragments)
```sh
x-www-browser https://github.com/cherkavi/cheat-sheet/blob/master/architecture-cheat-sheet.md#:~:text=Architecture cheat sheet&text=Useful links

# also possible to say prefix before the text
x-www-browser https://github.com/cherkavi/cheat-sheet/blob/master/architecture-cheat-sheet.md#:~:text=Postponing,%20about

# aslo possible to say previx and suffix around the destination text

```

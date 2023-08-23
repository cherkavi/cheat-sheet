# google chrome

## how to install 
```sh
x-www-browser https://www.google.com/chrome/thank-you.html?brand=CHBD&statcb=0&installdataindex=empty&defaultbrowser=0
# remove chrome cache: 
rm -rf ~/.cache/google-chrome
rm -rf ~/.config/google-chrome
sudo dpkg -i ~/Downloads/google-chrome-stable_current_amd64.deb
```

## [internal links](chrome://chrome-urls/)
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

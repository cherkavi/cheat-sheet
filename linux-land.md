# Linux-Android

## Firefox 

### Firefox installation 
```sh
sudo apt update && sudo apt install -y software-properties-common gnupg vim
sudo add-apt-repository ppa:mozillateam/ppa
```
skip snap during the installation
```sh
sudo vim /etc/apt/preferences.d/mozillateamppa
```
```sh
Package: firefox*
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001
```
```sh
sudo apt update 
sudo apt remove firefox
sudo apt install -y firefox
# sudo apt install -y firefox-esr # highly stable version
```
### Firefox usage
firefox-run.sh
```sh
MOZ_FORCE_DISABLE_E10S=1 MOZ_DISABLE_CONTENT_SANDBOX=1 firefox 2>/dev/null &
```
firefox-profile-run.sh
```sh
MOZ_FORCE_DISABLE_E10S=1 MOZ_DISABLE_CONTENT_SANDBOX=1 firefox -P 2>/dev/null &
```
firefox-kill.sh
```sh
ps aux | grep firefox | awk '{print $2}' | xargs kill -9
```
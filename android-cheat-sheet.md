# Android development cheat sheet

## [android OS architecture - layers](https://lineageos.org/engineering/HowTo-Debugging/)

## [Run Android OS without phone](https://github.com/cherkavi/solutions/blob/master/android-os-without-phone/README.md)

## connect phone to a computer ( Linux )
> for reading links like: mtp://motorola_moto_g52_xxxxxx
Settings -> Connected devices -> USB
  * Use USB for: File Transfer
  * Optional: USB controlled by connected device

## PC software for managing Android device
### Odin
#### Odin for Linux
1. install [heimdall](https://glassechidna.com.au/heimdall/)
    ```sh
    ubuntu_version=$(lsb_release -rs | cut -d. -f1)
    if [ "$ubuntu_version" -ge 22 ]; then 
        sudo apt install heimdall-flash heimdall-flash-frontend
        heimdall print-pit
    fi
    ```
2. download release of [JOdin3](https://github.com/GameTheory-/jodin3) and run JOdin3CASUAL
    ```sh
    soft; mkdir odin3; cd odin3
    wget https://github.com/GameTheory-/jodin3/releases/download/v1.0/Jodin3.zip
    unzip Jodin3.zip
    ls -la Jodin3/JOdin3CASUAL
    ```
### [ADB](https://developer.android.com/tools/adb) 
> Android Debug Bridge
> creates a connection (bridge) between the device and computer.
**should be activated:**
1. developer mode
   Settings -> About phone -> build number -> type 5 times on it 
2. USB debug
   Settings -> find word "develop" -> activate "USB debugging"
#### [download from google](https://dl.google.com/android/repository/platform-tools-latest-linux.zip)
#### adb install via Debian APT
```sh
sudo apt -y install adb
adb version
adb devices
```

#### [list of some adb all commands](https://www.getdroidtips.com/basic-adb-command/): 
copy files 
```sh
adb push test.apk /sdcard
adb pull /sdcard/demo.mp4 e:\
```
restart device in recovery mode 
```sh
adb reboot
adb reboot recovery
```

list of all applications/packages
```sh
adb shell pm list packages -f
# list of all installed applications with name of packages
adb shell pm list packages -f | awk -F '.apk=' '{printf "%-60s | %s\n", $2, $1}' | sort
# list of all non-system/installed applications
# | grep -v "package:/system" | grep -v "package:/vendor" | grep -v "package:/product" | grep -v "package:/apex"
adb shell pm list packages -f | awk -F '.apk=' '{printf "%-60s | %s\n", $2, $1}' | grep "package:/data/app/" | sort

# package_name=org.mozilla.firefox
# x-www-browser https://play.google.com/store/search?q=${package_name}&c=apps

# adb shell; 
# pm list packages -f
```
permission by package
```sh
PACKAGE_NAME=com.google.android.youtube
adb shell dumpsys package $PACKAGE_NAME | grep -i permission | grep -i granted=true
```
list of all permissions
```sh
adb shell pm list permissions
```

list of all features
```sh
adb shell pm list features
```
```sh
# print one file from OS
adb shell cat /proc/cpuinfo

# https://source.android.com/docs/core/architecture/bootloader/locking_unlocking
adb shell getprop | grep oem
adb shell getprop sys.oem_unlock_allowed
adb shell setprop sys.oem_unlock_allowed 1
# dmesg -wH
```

list of all settings
```sh
adb shell service list
adb shell settings list --user current secure 
adb shell settings get secure location_providers_allowed
adb shell settings get secure enabled_accessibility_services
```
 
 send keyboard event
```sh
adb shell input keyevent KEYCODE_HOME

# KEYCODE_A: A
# KEYCODE_B: B
# KEYCODE_ENTER: Enter
# KEYCODE_SPACE: Space
# KEYCODE_BACK: Back button
# KEYCODE_HOME: Home button
```

### [Fastboot](https://source.android.com/docs/setup/build/running)
> works only in "bootloader" or "fastboot" or "download" mode.
> boot your Android device into the bootloader mode
#### [download from google](https://dl.google.com/android/repository/platform-tools-latest-linux.zip)
#### fastboot install via Debian APT
```sh
sudo apt -y install fastboot
fastboot version
```

### check connected phone 
```sh
sudo apt install adb 

adb devices

lsusb
# Bus 001 Device 013: ID 04e8:6860 Samsung Electronics Co., Ltd Galaxy A5 (MTP)
```


### fix issue with root:root usb access 
```sh
ls -l /dev/bus/usb/001/013
# crw-rw-r--+ 1 root plugdev 189, 10 Mai 21 13:50 /dev/bus/usb/001/011

# if group is root, not a plugdev
sudo vim /etc/udev/rules.d/51-android.rules
SUBSYSTEM=="usb", ATTR{idVendor}=="04e8", ATTR{idProduct}=="6860", MODE="0660", GROUP="plugdev", SYMLINK+="android%n"

sudo service udev status
```

## decompilation, opening Android apk 
### [jadx](https://github.com/skylot/jadx)
#### download latest release
```sh
GIT_ACCOUNT=skylot
GIT_PROJECT=jadx
version=`wget -v https://github.com/${GIT_ACCOUNT}/${GIT_PROJECT}/releases/latest/download/$GIT_RELEASE_ARTIFACT 2>&1 | grep following | awk '{print $2}' | awk -F '/' '{print $8}'`
GIT_RELEASE_ARTIFACT=jadx-${version:1}.zip
wget -v https://github.com/${GIT_ACCOUNT}/${GIT_PROJECT}/releases/latest/download/$GIT_RELEASE_ARTIFACT
```
#### start gui 
```sh
./bin/jadx-gui
```

## adb usage
### print all applications with full set of granted permissions 
```sh
for each_app in `adb shell pm list packages -f | grep 'package:/data/app' | awk -F 'base.apk=' '{print $2}' | sort `; do
    echo ">>> $each_app"
    # each_app=com.google.android.youtube
    # adb shell pm dump $each_app | clipboard
    adb shell pm dump $each_app | awk '/runtime permissions:/,/disabledComponents:/ { if ($0 ~ /disabledComponents:/ || $0 ~ /enabledComponents:/) exit; else print }' | grep "granted=true"
    echo ""
done
```

## Android applications
### WhatsApp
internal storage for documents 
```sh
mtp://${YOUR_PHONE}/Internal%20shared%20storage/Android/media/com.whatsapp/WhatsApp/Media/WhatsApp%20Documents
```
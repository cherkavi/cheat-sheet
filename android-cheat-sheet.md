# Android development cheat sheet

## [android OS architecture - layers](https://lineageos.org/engineering/HowTo-Debugging/)

## [Run Android OS without phone](https://github.com/cherkavi/solutions/blob/master/android-os-without-phone/README.md)

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
2. USB debug
#### [download from google](https://dl.google.com/android/repository/platform-tools-latest-linux.zip)
#### adb install via Debian APT
```sh
sudo apt -y install adb
adb version
adb devices
```

#### [list of some adb all commands](https://www.getdroidtips.com/basic-adb-command/): 
```sh
adb push test.apk /sdcard
adb pull /sdcard/demo.mp4 e:\

adb reboot
adb reboot recovery
```
```sh
adb shell pm list packages -f
```

```sh
adb shell

pm list packages
pm list packages -f
pm list features
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

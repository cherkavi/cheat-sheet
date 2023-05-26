# Android development

## check connected phone 
```sh
sudo apt install adb 

adb devices

lsusb
# Bus 001 Device 013: ID 04e8:6860 Samsung Electronics Co., Ltd Galaxy A5 (MTP)
```


## fix issue with root:root usb access 
```sh
ls -l /dev/bus/usb/001/013
# crw-rw-r--+ 1 root plugdev 189, 10 Mai 21 13:50 /dev/bus/usb/001/011

# if group is root, not a plugdev
sudo vim /etc/udev/rules.d/51-android.rules
SUBSYSTEM=="usb", ATTR{idVendor}=="04e8", ATTR{idProduct}=="6860", MODE="0660", GROUP="plugdev", SYMLINK+="android%n"

sudo service udev status
```

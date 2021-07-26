# Linux devices

### install drivers, update drivers ubuntu
```
sudo ubuntu-drivers autoinstall
```

### zbook nvidia hp zbook hp nvidia
```
sudo prime select query
# should be nvidia
sudo ubuntu-drivers devices
# sudo ubuntu-drivers autoinstall - don't use it
sudo apt install nvidia driver-455
# or 390, 415, .... and restart
```

### apple keyboard, alternative 
```sh
echo 'options hid_apple fnmode=2 iso_layout=0 swap_opt_cmd=0' | sudo tee /etc/modprobe.d/hid_apple.conf
sudo update-initramfs -u -k all
```


## video camera, camera settings, [webcam setup](https://wiki.archlinux.org/index.php/Webcam_setup)
```sh
# camera utils installation
sudo apt install v4l-utils
sudo apt install qv4l2
# list of devices
v4l2-ctl --list-devices
# list of settings
v4l2-ctl -d /dev/video0 --list-ctrls
```
camera settings example
```sh
# /etc/udev/rules.d/99-logitech-default-zoom.rules
SUBSYSTEM=="video4linux", KERNEL=="video[0-9]*", ATTRS{product}=="HD Pro Webcam C920", ATTRS{serial}=="BBBBFFFF", RUN="/usr/bin/v4l2-ctl -d $devnode --set-ctrl=zoom_absolute=170"
```

## wacom tablet, wacom graphical tablet, map wacom, map tablet, tablet to display
> your wacom device has two modes - PC/Android, for switching between them - press and keep for 3-4 sec two outermost buttons.
```sh
# detect your monitors and select one of the output like 'HDMI-1'
xrandr --listmonitors

# detect all wacom devices
# xsetwacom --list devices
xinput | grep -i wacom | awk -F 'id=' '{print $2}' | awk '{print $1}' | while read each_input_device
do
	# xsetwacom set 21 MapToOutput 2560x1440+1080+0
	xinput map-to-output $each_input_device HDMI-1
done
```

## external monitor settings, external screen, external display
```monitor.sh
#!/bin/sh
xrandr --output $1
xrandr --output $2 --auto --right-of $1
xrandr --output $3 --auto --right-of $2
```
```
xrandr | grep " connected" | awk '{print $1}'
 ./monitor.sh "DP-4" "DP-1-3" "eDP-1-1"
```

or just install 'arandr' and generate bash script 
```
sudo apt install arandr
```

## bluetooth
```sh
# connect and disconnect headphones
bluetoothctl connect 00:18:09:EC:BE:FD
bluetoothctl disconnect 00:18:09:EC:BE:FD
# for manual 
```
```sh
sudo apt install bluez-tools
bt-device --list
bt-device --disconnect 00:18:09:EC:BE:FD
bt-device --connect 00:18:09:EC:BE:FD
```

## output audio device, sound card, headphones
```
# list of all outputs
pacmd list-sinks | grep -A 1 index
# set default output as
pacmd set-default-sink 16
pacmd set-default-sink bluez_sink.00_18_09_EC_BE_FD.a2dp_sink

# list of input devices
pacmd list-sources | grep -A 1 index
# set default input device
pacmd set-default-source 6
pacmd set-default-source alsa_input.pci-0000_00_1f.3-platform-skl_hda_dsp_generic.HiFi__hw_sofhdadsp_6__source

# mute microphone mute source
pacmd set-source-mute alsa_input.pci-0000_00_1f.3-platform-skl_hda_dsp_generic.HiFi__hw_sofhdadsp_6__source true
# unmute microphone unmute source
pacmd set-source-mute alsa_input.pci-0000_00_1f.3-platform-skl_hda_dsp_generic.HiFi__hw_sofhdadsp_6__source false
```

## control mouse from keyboard
```
sudo apt-get install keynav
killall keynav 
cp /usr/share/doc/keynav/keynavrc ~/.keynavrc
keynav ~/.keynavrc
```
example of custom configfile
```
clear
daemonize
Super+j start,cursorzoom 400 400
Escape end
shift+j cut-left
shift+k cut-down
shift+i cut-up
shift+l cut-right
j move-left
k move-down
i move-up
l move-right
space warp,click 1,end
Return warp,click 1,end
1 click 1
2 click 2
3 click 3
w windowzoom
c cursorzoom 400 400
a history-back
Left move-left 10
Down move-down 10
Up move-up 10
Right move-right 10
```


## Touch screen
### calibration
tool installation
```
sudo apt install xinput-calibrator
```
configuration
```
xinput_calibration
```
list of all devices, device list, list of devices
```
xinput --list
cat /proc/bus/input/devices
```
permanent applying
```
vi /usr/share/X11/xorg.conf.d/80-touch.conf
```
disable device
```
xinput --disable {number from command --list}
```

## Keyboard Lenovo
### middle button
```bash
# check input source - use name(s) for next command
xinput
# create file and add content
sudo vim /usr/share/X11/xorg.conf.d/50-thinkpad.conf
```
```
Section "InputClass"
    Identifier  "Trackpoint Wheel Emulation"
    MatchProduct    "Lenovo ThinkPad Compact USB Keyboard with TrackPoint|ThinkPad Extra Buttons"
    MatchDevicePath "/dev/input/event*"
    Option      "EmulateWheel"      "true"
    Option      "EmulateWheelButton"    "2"
    Option      "Emulate3Buttons"   "false"
    Option      "XAxisMapping"      "6 7"
    Option      "YAxisMapping"      "4 5"
EndSection
```

### recover usb drive
```
sudo fdisk -l
sudo lsblk
sudo fsck /dev/sdb
e2fsck -b 32768 /dev/sdb
sudo e2fsck -b 32768 /dev/sdb
sudo dd if=/dev/zero of=/dev/sdb
sudo fdisk /dev/sdb
sudo partprobe -s
sudo mkfs.vfat -F 32 /dev/sdb
sudo dd if=/dev/zero of=/dev/sdb bs=512 count=1
sudo fdisk /dev/sdb
```

## home automation
### DTMF generator
```
sox -n dtmf-1.wav synth 0.1 sine 697 sine 1209 channels 1
sox -n dtmf-2.wav synth 0.1 sine 697 sine 1336 channels 1
sox -n dtmf-3.wav synth 0.1 sine 697 sine 1477 channels 1
```


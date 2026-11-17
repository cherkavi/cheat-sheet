# ZigBee devices 

## Zigbee coordinator

### check connection 
```sh
sudo apt update
sudo apt install -y usbutils

sudo usermod -aG dialout "$USER"

## -------------------------
## Check USB device
lsusb | grep -i -E 'Texas|Sonoff|Silicon Labs'
# Bus 001 Device 006: ID 10c4:ea60 Silicon Labs CP210x UART Bridge

## -------------------------
## Check serial devices
ls -l /dev/serial/by-id/
# lrwxrwxrwx 1 root root 13 Nov 17 12:03 usb-ITead_Sonoff_Zigbee_3.0_USB_Dongle_Plus_22726ce.......-if00-port0 -> ../../ttyUSB0

## -------------------------
## Check kernel messages
sudo dmesg | grep -i -E 'ttyUSB|ttyACM|cp210|ch34'
# [ 2362.309053] usbcore: registered new interface driver cp210x
# [ 2362.309067] usbserial: USB Serial support registered for cp210x
# [ 2362.309088] cp210x 1-13:1.0: cp210x converter detected
# [ 2362.310840] usb 1-13: cp210x converter now attached to ttyUSB0


## -------------------------
## Check permissions
groups
```

### initialize
```sh
export ZIGBEE_PORT="$(find /dev/serial/by-id -maxdepth 1 -type l | head -n1)"

echo "$ZIGBEE_PORT"
readlink -f "$ZIGBEE_PORT"
```

## [Zigbee solutions](https://github.com/cherkavi/solutions/zigbee/controller-sensor-window-door/README.md)

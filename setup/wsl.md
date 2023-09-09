# setup with wsl

## use usb port

get usbuipd - https://github.com/dorssel/usbipd-win

```bash
# connect usb device and attach 
usbipd wsl list
BUSID  VID:PID    DEVICE                                                        STATE
1-3    0bda:b85c  Realtek Wireless Bluetooth Adapter                            Not attached
1-4    04f3:0c7e  ELAN WBF Fingerprint Sensor                                   Not attached
2-2    0403:6001  USB Serial Converter                                          Not attached

usbipd wsl attach --busid=2-2
...
```

```bash
# add device in linux wsl
lsusb 
...
# see and restart colrado, normal uses /dev/ttyUSB0
```

## install

Installation with ansible scripts 
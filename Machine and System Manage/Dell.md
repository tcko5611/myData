# Dell

# Restore the system of windows for E7440

1.  goto dell web and save the windows to usb :girl:
2.  using dell F12 key to enter new entrance for usb
3.  start build the new system

# while close the top, still have screen to work

1.  goto the system control panel about power
2.  turn off when up close and into sleep

# add wi-fi printer

1.  get the driver from the printer web
2.  from wifi router get the ip of the printer
3.  fill the ip to the driver

# needed software

1.  Java
2.  librepilot
3.  dropbox

# install E6420 in ubuntu

## show the network

```bash
lshw -c network
lsspci -nnk | grep -iA2 net
rfkill list all

```

## install the driver for wifi and start it

```bash
sudo apt install bcmwl-kernel-source
sudo modprobe wl

```
## Kernel Module

Keychron keyboard uses `hid_apple` kernel module on Linux system. You can use `modinfo` command to check this.

`modinfo hid_apple`

If you cannot see hid_apple loaded, you can use moprobe to add module.

`modeprobe hid_apple`

You can also use `lsmod` and `modinfo` to verify if `hid_apple` module has been added.

`lsmod | grep "hid_apple"`

`modinfo hid_apple`


## Bluetooth
Go to `/etc/bluetooth/main.conf` and add or uncomment following settings:

To verify the existing config
`cat /etc/bluetooth/main.conf`

To change the config use nano
`nano /etc/bluetooth/main.conf`

Configuration:
```
FastConnectable =true

AutoEnable=true

UserspaceHID=true

IdleTimeout = 0
```

Restart bluetooth service using `systemctl restart bluetooth`
Enable bluetooth automatically on start up using `systemctl enable bluetooth`

## Function Key

### Test function key

The default function mode change be changed if you like as per below using value 0 or 1 or 2.
```
0 = disabled

1 = normally media keys, switchable to function keys by holding Fn key (Default)

2 = normally function keys, switchable to media keys by holding Fn key
```
To test the config you can use  `echo <value> >> /sys/module/hid_apple/parameters/fnmode`
Replace the <value> with 0 or 1 or 2

For me function keys works best so,

```
echo 2 >> /sys/module/hid_apple/parameters/fnmode
```

### Make permanent change in fnmode:

Go to `/etc/modprobe.d/hid_apple.conf` to make permanent change.

For some reason `hid_apple.conf` file did not exit for me, had to create myself.

To verify the existing config:
`cat /etc/modprobe.d/hid_apple.conf`

To edit the config use nano:
`nano /etc/modprobe.d/hid_apple.conf`

`options hid_apple fnmode=2`

Now reload kernel module:

`modprobe -r hid_apple && modprobe hid_apple`

## Stay awake
Go to `/etc/modprobe.d/btusb.conf` and add `options btusb enable_autosuspend=0`

Use `cat /etc/modprobe.d/btusb.conf` to verify the exisiting config

Use `nano /etc/modprobe.d/btusb.conf` to edit it and insert:

`options btusb enable_autosuspend=0`

## Power Percentage

Check battery level for keyboard using `upower --dump`

`upower --dump | grep -i keyboard -A 10`

or 

`upower --dump | grep -i keyboard -A 10 | grep percentage`

Here 10 is the number of lines after the grep command visible in output.


## WakeUp with external keyboard while lid is closed (wire plugged)

First of all find all the USB available

`grep . /sys/bus/usb/devices/*/power/wakeup`

Now, root it ... haha

`sudo su`

Enable each of the USB for power wakeup mine looks like

```
echo enabled > /sys/bus/usb/devices/2-10/power/wakeup
echo enabled > /sys/bus/usb/devices/2-5/power/wakeup
echo enabled > /sys/bus/usb/devices/2-8/power/wakeup
echo enabled > /sys/bus/usb/devices/2-9/power/wakeup
echo enabled > /sys/bus/usb/devices/3-1/power/wakeup
echo enabled > /sys/bus/usb/devices/usb1/power/wakeup
echo enabled > /sys/bus/usb/devices/usb2/power/wakeup
echo enabled > /sys/bus/usb/devices/usb3/power/wakeup
echo enabled > /sys/bus/usb/devices/usb4/power/wakeup
```

Test it.

Now to go to 
`sudo nano /etc/rc.local`
and copy the code from above to the text editor.



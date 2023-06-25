## Kernel Module

Keychron keyboard uses `hid_apple` kernel module on Linux system. You can use `modinfo` command to check this.

`modinfo hid_apple`

If you cannot see hid_apple loaded, you can use moprobe to add module.

`modeprobe hid_apple`

You can also use `lsmod` and `modinfo` to verify if `hid_apple` module has been added.

`lsmod | grep "hid_apple"`


## Bluetooth
Go to `cat /etc/bluetooth/main.conf` and add or uncomment following settings:

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

For me function keys works best so,

```
echo 2 >> /sys/module/hid_apple/parameters/fnmode
```

### Make permanent change in fnmode:

Go to cat /etc/modprobe.d/hid_apple.conf to make permanent change.

`options hid_apple fnmode=2`

Now reload kernel module:

`modprobe -r hid_apple && modprobe hid_apple`

## other

Check battery level for keyboard using `upower --dump`

`upower --dump | grep -i keyboard -A 10`

or 

`upower --dump | grep -i keyboard -A 7 | grep percentage`



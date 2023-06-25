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

## Function Key

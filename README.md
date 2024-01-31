# ble_icee

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


# NOTES:
* BLE API limitation on non https connection

## BLuetooth on raspbian - Instructions

* https://www.instructables.com/Control-Bluetooth-LE-Devices-From-A-Raspberry-Pi/

* Install version 5.50 following instruction on the above link

* Before make add include <linux/sockios.h> to files ```l2test.c``` and ```rctest.c```

## BLE App based on python project
* https://github.com/Douglas6/cputemp
* *  sudo nano /etc/systemd/system/dbus-org.bluez.service add the line 
```
	ExecStart=/usr/lib/bluetooth/bluetoothd -E
```
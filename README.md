## lampify
This is a simple CLI application you can use to control your Bluetooth Low Energy (BLE) lamp.

## Features
The project offers the following functionality:
- Turning the lamp on / off
- Controlling lamp brightness
- Controlling lamp temperature
- Sending initial setup signal
- Sending desktop notifications

## Lamp compatibility
lampify was tested with the lamp **Victoria Lighting Losange/PL500**, hovewer a lot of chinese lamps have common control algorithm, so lampify should be compatible with all BLE lamps that can be controlled via Android applications from the list below (the list may be incomplete).

  - [LampSmart Pro](https://play.google.com/store/apps/details?id=com.jingyuan.lamp)

Also tested on "Jaycomey Dimmable Ceiling Light,4 Squares Modern LED Ceiling Lamps with Remote Control,80W Acrylic Flush Mount Ceiling Light Fixture for Living Dining Room Bedroom Kitchen(Black)" as available on Amazon

## Dependencies
This is the list of packages providing the files required to compile lampify. Be aware that package names usually do vary across different linux distributions.

- Debian 12
  - libbluetooth-dev
  - libnotify-dev
  - make
  - gcc
  - bluez
  - git

## Compilation
The compilation process is pretty simple:
```
git clone https://github.com/Justin8428/lampify.git
cd lampify
make
```
To install (places executable in /usr/local/bin and sets up permissions):
```
sudo make install
```

To uninstall (removes executable from /usr/local/bin):
```
sudo make uninstall
```

## Usage
### General syntax
The syntax of provided CLI interface can be retrieved by running lampify without arguments:
```
[ user@archlinux ~ ] $ lampify                                                                               
Lampify 1.0.0 by MasterDevX
Made in Ukraine

Usage:
lampify <mode> <action> [parameter]

Available modes:
q               quiet (without notifications)
v               verbose (with notifications)

Available actions:
setup           connect to the lamp
on              turn the lamp on
off             turn the lamp off
cold  <0..9>    set cold brightness
warm  <0..9>    set warm brightness
dual  <0..9>    set dual brightness

```

### Initial setup
Before you can control your lamp, you have to perform an initial setup so the lamp will remember the unique ID of your device generated by lampify. The ID is being generated based on your machine's hostname.

To perform the initial setup:
- Turn the lamp on using the power switch
- Within a few seconds after powering the lamp on, send a setup signal from your device:

  ```
  sudo lampify v setup
  ```
- If you see the lamp flashing, the connection is established

**sudo permissions are required to access the ble stack on modern linux**

### Command examples
Turn the lamp on (without notification):
```
lampify q on
```

Turn the lamp off (with notification):
```
lampify v off
```

Set warm temperature and brightness to 5 (without notification):
```
lampify q warm 5
```

Set cold temperature and brightness to 2 (with notification):
```
lampify v cold 2
```

## lampify-updated
This is a simple CLI application you can use to control your Bluetooth Low Energy (BLE) lamp.
Many thanks to MasterDevX for the initial codebase.

## Features
The project offers the following functionality:
- Turning the lamp on / off
- Controlling lamp brightness
- Controlling lamp temperature
- Sending initial setup signal
- **Multiple lamp support** via manually specifying unique id
- Sending desktop notifications (GNOME3 only; notification support to be removed soon)

## Lamp compatibility
lampify-updated should work with at least some lamps (that are non-RGB, dimmable, cool/warm only) that use the following app

- [LampSmart Pro](https://play.google.com/store/apps/details?id=com.jingyuan.lamp)

Lamps tested to work include

- Victoria Lighting Losange/PL500
- "Jaycomey Dimmable Ceiling Light,4 Squares Modern LED Ceiling Lamps with Remote Control,80W Acrylic Flush Mount Ceiling Light Fixture for Living Dining Room Bedroom Kitchen(Black)" as available on Amazon


## Requirements

### OS and packages
This is the list of packages providing the files required to compile lampify. Be aware that package names usually do vary across different linux distributions.
- Debian 12
  - libbluetooth-dev
  - libnotify-dev
  - make
  - gcc
  - bluez
  - git

It may be a good idea to set up a separate minimal Debian install in a VM, as lampify requires `sudo` permissions to work (sending HCI commands in Linux requires sudo permissions).

Ubuntu (20.04 and 22.04) have been tested to not work.

### Hardware
Your bluetooth card needs to support at least Bluetooth v4.0 LE and have working drivers / firmware for Linux.

Depending on your distro / hardware you may need to install drivers / firmware for your bluetooth card. (Older) Intel cards seem to work out of the box. e.g Broadcom users see https://github.com/winterheart/broadcom-bt-firmware.

Working cards:

 - Intel AC-8265
 - Broadcome BCM20702A0
 - Qualcomm Atheros QCA9377
   
The Intel AX210 does not appear to be working.


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
[ user@debian ~ ] $ lampify                                                                               
Lampify 2.0.0 by MasterDevX, Justin8428
2020, 2023

Usage:
./lampify <mode> <uniqueid> <action> [parameter]

Available modes:
q               quiet (without notifications)
v               verbose (with notifications)

Allowable uniqueid:
Unique id can be any string (up to 1024 characters)
that is made up of capital letters and numbers without spaces.
Example: LAMP01

Available actions:
setup           connect to the lamp
on              turn the lamp on
off             turn the lamp off
cold  <0..9>    set cold brightness
warm  <0..9>    set warm brightness
dual  <0..9>    set dual brightness

```

### Initial setup
Before you can control your lamp, you have to perform an initial setup so the lamp will remember a unique ID that you specify. This unique ID should be made up of capital letters and numbers without spaces. Once you have setup this unique ID, you do not need to re-setup the lamp as long as you remember what this ID is!

To setup more than one lamp, setup each lamp individually by assigning a new unique ID to each lamp.

To perform the initial setup:
- Think up of a unique ID. In this example we will use LAMP01TEST.
- Turn the lamp on using the power switch
- Within a few seconds after powering the lamp on, send a setup signal from your device:

  ```
  sudo lampify v LAMP01TEST setup
  ```
- If you see the lamp flashing, the connection is established

To reiterate **sudo permissions are required to access the ble stack on modern linux**

### Command examples
Turn the lamp on (without notification):
```
sudo lampify q LAMP01TEST on
```

Turn the lamp off (with notification):
```
sudo lampify v LAMP01TEST off
```

Set warm temperature and brightness to 5 (without notification):
```
sudo lampify q LAMP01TEST warm 5
```

Set cold temperature and brightness to 2 (with notification):
```
sudo lampify v LAMP01TEST cold 2
```

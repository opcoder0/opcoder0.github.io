---
layout: post
author: opcoder0
---

## Configuring USB 3G Modem on Linux

I was in the process of configuring my USB 3G broadband modem with my Linux boxes and was interested to find out all about Linux’s USB serial interface and drivers etc. So I’ve kind of assimilated all the information below along with the links for those of you who want to read them.

Its fairly simple to configure any (or most) of the USB modems in Linux. But you’ll need to know some details about it first. You would need to find out

– Username (this could be your SIM card’s phone number)
– Password (this could be your SIM card’s phone number)
– Phone Number (Which number to dial. Example: #777)

Before you connect you will need to write `/etc/wvdial.conf` You can read the man page of `wvdial.conf` and write everything by hand or run `sudo wvdialconf` on your computer. Linux probes for details of the device and generates `wvdial.conf` with some information. It could look like something below —

```
[Dialer Defaults]
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
Modem Type = Analog Modem
; Phone = <Target Phone Number>
ISDN = 0
; Username = <Your Login Name>
Init1 = ATZ
; Password = <Your Password>
Modem = /dev/ttyUSB0
Baud = 9600
```

You will then need to fill up the rest of the details such as (a sample) –

```
[Dialer Defaults]
Modem = /dev/ttyUSB0
Init1 = ATZ
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
CBaud = 460800
Stupid Mode = 1
Modem Type = Analog Modem
Phone = *99#
New PPPD = yes
ISDN = 0
Username = <Your Username>
Password = <Your Password>
Baud = 9600

```

Save and quit the above file and then run “sudo wvdial” with the device plugged and you should be online after that.

Some internal information –

USB modem [http://en.wikipedia.org/wiki/Modem](http://en.wikipedia.org/wiki/Modem)

USB wireless modem use a USB port on the laptop instead of a PC card or ExpressCard slot. Most of the wireless modems come with an integrated SIM cardholder (Huawei E220, Sierra 881, etc.) and some models also provided a microSD memory slot and/or jack for additional external antenna such as Huawei E1762 and Sierra Wireless Compass 885.

USB modems work in Linux with kernel versions 2.6.20 and greater. Its development started in 2.5. This article on Linux Journal explains about USB Serial implementation in the linux kernel in detail.

It needs support for `/dev/ttyUSB*` devices. See this link in wikipedia for more information [https://en.wikipedia.org/wiki/Huawei_E220](https://en.wikipedia.org/wiki/Huawei_E220). You can use wvdial to connect via this USB modem.

`wvdial` (See wikipedia link [https://en.wikipedia.org/wiki/Wvdial](https://en.wikipedia.org/wiki/Wvdial) is a dial-up utility available for Linux OSes.

`wvdial` initializes the modem and executes the dial up instructions based on what is specified in `wvdial.conf` (See `man wvdial.conf`) to find out what to write in that file. Or you can plugin the USB modem and run wvdialconf (as root or sudo) and that detects and generates the `/etc/wvdial.conf` file.

### RESOURCES

[https://en.wikipedia.org/wiki/Modem](https://en.wikipedia.org/wiki/Modem)
[https://forum.openwrt.org/viewtopic.php?id=19171](https://forum.openwrt.org/viewtopic.php?id=19171)
[https://en.wikipedia.org/wiki/Wvdial#cite_note-0](https://en.wikipedia.org/wiki/Wvdial#cite_note-0)
[https://wiki.archlinux.org/index.php/USB_3G_Modem](https://wiki.archlinux.org/index.php/USB_3G_Modem)
[https://www.mjmwired.net/kernel/Documentation/usb/usb-serial.txt](https://www.mjmwired.net/kernel/Documentation/usb/usb-serial.txt)
[https://www.linux-usb.org/](https://www.linux-usb.org/)
[https://www.linuxjournal.com/article/6434?page=0,2](https://www.linuxjournal.com/article/6434?page=0,2)
[https://www.linux-usb.org/devices.html](https://www.linux-usb.org/devices.html)
[https://cateee.net/lkddb/web-lkddb/USB_SERIAL.html](https://cateee.net/lkddb/web-lkddb/USB_SERIAL.html)

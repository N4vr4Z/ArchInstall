# ARCH INSTALLATION GUIDE

## DOWNLOADING ISO AND MAKING A LIVE-USB

Download [Arch Linux ISO](https://archlinux.org/download/) and use it to create a live-USB using rufus (windows), woeUSB (linux) , balenaetcher .

## INSTALLING ISO FROM LIVE-USB 

> Note: Set Boot priority to USB in BIOS settings beforehand 

###### STEP 1 : BOOTLOADER
After you are greated with Arch linux boot loader , Select **Boot Arch Linux (x86_64)
You will be greated by a terminal ( you'll be logged in as root )

###### STEP2 : CONFIGURING INTERNET CONNECTION 
- **Ethernet** — plug in the cable.
- **Wi-Fi** — authenticate to the wireless network using [iwctl](https://wiki.archlinux.org/title/Iwctl) .
- **Mobile broadband modem** — connect to the mobile network with the [mmcli](https://wiki.archlinux.org/title/Mobile_broadband_modem#ModemManager) utility.
`Ping google.com` to check if your internet connection is working 
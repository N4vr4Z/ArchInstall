# ARCH INSTALLATION GUIDE



## DOWNLOADING ISO AND MAKING A LIVE-USB

Download [Arch Linux ISO](https://archlinux.org/download/) and use it to create a live-USB using rufus (windows), woeUSB (linux) , balenaetcher .



## INSTALLING ISO FROM LIVE-USB 

> Note: Set Boot priority to USB in BIOS settings beforehand 



###### STEP 1 : BOOTLOADER

After you are greeted with Arch linux boot loader , Select **Boot Arch Linux (x86_64)**

You will be greeted by a terminal ( you'll be logged in as root )



###### STEP2 : CONFIGURING INTERNET CONNECTION 

- **Ethernet** — plug in the cable.

- **Wi-Fi** — authenticate to the wireless network using [iwctl](https://wiki.archlinux.org/title/Iwctl) .

**IWCTL** Quick guide  -

`iwctl`- starts iwd interactive prompt 

`[iwd]# device list ` - lists all network devices

`[iwd]# station *device* scan` - replace device with your networking device ( for example wlan0)

`[iwd]# station *device* get-networks` - shows all visible wifi networks 

`[iwd]# station *device* connect *SSID* ` - If network is password protected it will ask for a passphrase 

Ctrl+C / exit to leave iwd prompt 

Use command `Ping google.com` to check if your internet connection is working 



###### STEP3 : PARTITIONING DRIVERS 
Before proceding to partitioning drives , Lets set correct time to avoid any possible netwoks errors using command `timedatectl set-ntp true`

Now we'll continue to partition our drives

Lets list our current storage devices using the command `lsblk`

Now , to start partitioning run `cfdisk /dev/device_to_create_partition_on`

We'll be greeted by four options , choose the correct option according to your computer :

**gpt** : If you have a relatively newer computer and have a storage device with storage capacity >2 TB , use this option 

**dos** : If you have a old computer / storage device with storage capacity <2 TB , use this option 

Create a 512 MB bootable partition . Partition can be made bootable by pressing B on the partition . 

I will create a root and home partition .

Create another partitions according to your will . Now quit the partition utility and format the partitions we just made using :

`mkfs.ext4 /dev/boot_partition` - formats our boot partition which will be assigned to boot loader

`mkfs.ext4 /dev/root_partition` - formats the partition which will be assigned to / 

`mkfs.ext4 /dev/home_partition` - formats the partition which will be assigned to /home
 
 Swap is like a virtual-RAM that system can use when required . My advice will be to skip a swap partition if you are using a HDD . Slow writing speed of HDD will barely help to improve performance . If you wanna make a swap partition anyways , you can do so by making a partition with partition type "Linux SWAP" . Now initialise the swap partition using `mkswap /dev/swap_partition`
 
 Now we will mount the partitions we just created . 

`mount /dev/root_partition /mnt` - mounts root partition to /mnt 

`mkdir /mnt/boot` - makes a directory called boot in /mnt 

`mount /dev/boot_partition /mnt/boot` - mounts boot partition to /mnt/boot . This is the directory where the boot loader will be installed 

`mkdir /mnt/home` - makes a directory called home in /mnt 

`mount /dev/home_partition /mnt/home` - mounts home partition to /mnt/home

If you made a SWAP partition , you'll also have to mount the swap partition using `swapon /dev/swap_partition`

After creating partitions sucessfully you can cross-check them using `lsblk` .



###### STEP4 : INSTALLING SYSTEM FILES 

If you use ethernet :

`pacstrap /mnt linux linux-firmware base base-devel vim networkmanager `

If you use wifi :

When using iwctl , I prefer to use dhcpcd instead of NetworkManager . 

`pacstrap /mnt linux linux-firmware base base-devel vim iwctl dhcpcd `

> NOTE : You may also need to install ntfs-3g , so that you can read windows partitions . This will be necessary in case of dual booting windows 

Now generate an fstab using :

`genfstab -U /mnt >> /mnt/etc/fstab`

###### STEP4: FINISHING THE INSTALLATION

`arch-chroot /mnt` - this will chroot us into the distro we just installed .

During previous step , if you installed networkmanager , use:

`systemctl enable NetworkManager` - enables network manager 

if you installed dhcpcd , use :

`systemctl enable dhcpcd`

Now lets set up a bootloader , to do so , we'll use the arch package handler pacman . 

`pacman -S grub `

`grub-install /dev/device_to_which_you_installed_arch ` - installs grub 

`grub-mkconfig -o /boot/grub/grub.cfg` - configures grub for use

> Configuring grub to detect windows 

`pacman -S ntfs-3g os-prober`  

then edit /etc/default/grub and add/uncomment: "GRUB_DISABLE_OS_PROBER=false"

`mkdir /mnt/boot/windows`

`mount /dev/windows_partition /mnt/boot/windows `

`grub-mkconfig -o /boot/grub/grub.cfg`

###### STEP5: SETTING UP THE INSTALLED DISTRO 

`passwd` - to set up  a password

`vim /etc/locale.gen` - select the language you want to use by uncommenting it . ( for example , en_US UTF and en_US ISO )

`locale-gen` - generate the locale you uncommented during previous command 

`vim /etc/locale.conf` and write LANG=en_US.UTF-8

`vim /etc/hostname` - set your hostname ( the name which is used in shell )

`ln -sf /usr/share/zoneinfo/use_tab_key /etc/localtime ` - set the timezone , your computer will use 

everything is finished , lets reboot 

`exit`

`umount -R /mnt`

 `reboot`
 
 INSTALLATION FINISHED :)

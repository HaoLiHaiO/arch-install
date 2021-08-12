# My Arch Install - UEFI and network (August 2021)

## Disclaimer

This is my personal Arch Linux installation process. It is a personal document,
It does not aim to replace the excellent official installation guide which can 
be found at [Arch Linux Official Installation Guide](https://wiki.archlinux.org/title/installation_guide) . It is based on my personal preferences. Besides, 
it is not comprehensive and I am not including the parts which, for me, 
are straightforward (e.g. make a live USB, use iwctl to connect to the 
network etc.).

## Partitioning

- Either use cfdisk /dev/*diskname* [//]: # /dev/sdX or /dev/nvme
- Swap partition can be made later
- New syntax for EFI formatting: mkfs.fat -F 32 /dev/*bootpartition*
- Root formatting: mkfs.ext4 /dev/*rootpartition*

## Mounting

- Mount root on mount: mount /dev/*rootpartition* /mnt
- Create EFI directory: mkdir /mnt/efi
- Mount boot partition: mount /dev/*bootpartition* /mnt/efi

## Install base packages

- pacstrap /mnt base linux linux-firmware

## Generate UUID for the disk

- genfstab -U /mnt >> /mnt/etc/fstab

## CHROOT

- arch-chroot /mnt

## New Arch root

### Wireless network

- pacman -Syu iwd

### Wired network

- pacman -Syu netctl dialog dhcpcd wpa_supplicant ifplugd (with openresolv as
default instead of systemd-resolvconf)

## User add and sudo

- useradd -G wheel,audio,video -m *username*
- passwd *username*
- passwd (without anything for root)
- pacman -Syu sudo
- pacman -Syu vim nano && EDITOR=vim visudo and uncomment wheel group

## Bootloader - GRUB

- pacman -Syu grub efibootmgr
- grub-install --target=x86_64-efi --efi-directory=/efi/ --bootloader-id=Arch
- grub-mkconfig -o /boot/grub/grub.cfg

## Exit chroot and reboot

- log in as user and then sudo su to check if root login works

### Locale

- vim /etc/locale.gen and uncomment the right option
- run locale.gen
- vim /etc/locale.conf and: LANG=en_US-UTF8

### Network - hostname and hosts

- archlinux is default hostname, to change it:
  vim /etc/hostname and: *yourhostname* (e.g. *Alex*)
- vim /etc/hosts:
  127.0.0.1  localhost
  ::1        localhost
  127.0.0.1  *yourhostname*.localdomain *yourhostname*

#### Enabling and starting the network using iwd which we installed

- systemctl enable iwd.service
- systemctl start iwd.service
- systemctl enable systemd-resolved.service
- systemctl start systemd-resolved.service
- vim /etc/iwd/main.conf
    [General]
    EnableNetworkConfiguration=true
    
    [Network]
    NameResolvingService systemd

- now run iwctl, same process as before
- test with ping

## Package installation

- sudo pacman -Syu openssh && ssh-keygen -t ed25519 -C *github email*



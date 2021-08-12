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


---
{"dg-publish":true,"permalink":"/course/3rd-year-2024-2025/1st-sem/system-administration-and-maintenance-1/installation/archlinux/","noteIcon":""}
---

#### Step 1: Disk Partitioning

**Partition using cfdisk**
```bash
lsblk
cfdisk
# gpt
# efi 512M
# swap 4G
# root + home 30G
```

**Formatting**
```bash
mkfs.fat -F 32 -n "BOOT" /dev/sda1
mkfs.ext4 -L "Arch" /dev/sda3
swapon /dev/sda2
```

**Mounting**
```bash
mount /dev/sda3 /mnt
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
swapon /dev/sda2

# OPTIONAL if you have separate home partition you should mount it to /mnt/home
# mount /dev/sdaX /mnt/home 
```

#### Step 2: Installation
**Initialize and Generate keys**
```bash
pacman-key --init
pacman-key --populate
```
**Wait for a minute or do some checking**

**Check internet**
```bash
iplink
ping archlinux.org
```

```
loadkeys us
```

**Install base system**
```
pacstrap -K /mnt base linux linux-headers linux-firmware base-devel intel-ucode sudo vim neofetch networkmanager grub efibootmgr dosfstools
```

**Generate `fstab`**
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```


#### Step 4: OS Configuration using `chroot`

```bash
arch-chroot /mnt /bin/bash

ln -sf /usr/share/zoneinfo/Asia/Manila /etc/localtime
hwclock --systohc

vim /etc/locale.gen

locale-gen

echo "LANG=en_US.UTF-8" >> /etc/locale.conf
cat /etc/locale.conf

echo "dejarch" >> /etc/hostname
cat /etc/hostname

vim /etc/hosts
# 127.0.0.1   localhost
# ::1         localhost
# 127.0.1.1   dejarch.localdomain    dejarch


mkinitcpio -P
```


**User**
```bash
# change root passwd
passwd

# add user
useradd dejames
passwd dejames
usermod -aG wheel,video,audio dejames

visudo
# uncomment wheel
# add dejames ALL=(ALL:ALL) ALL

```

#### Step 5: installing Grub Bootloader

```bash
pacman -S grub
```

Setup GRUB
```
grub-install --target=x86_64 --efi-directory=/boot/efi --bootloader-id="dejarch-grub"
grub-mkconfig -o /boot/grub/grub.cfg
```

**Finalizing and running first boot**
```bash
exit 
shutdown -P now
# remove iso file
# run vm
# login to root
# create a home dir for dejames user
# mkdir /home/dejames
# chown -R dejames:dejames /home/dejames
# cp /etc/skel/ /home/dejames
```
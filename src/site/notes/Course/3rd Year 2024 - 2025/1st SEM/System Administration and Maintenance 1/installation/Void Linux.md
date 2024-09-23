---
{"dg-publish":true,"permalink":"/course/3rd-year-2024-2025/1st-sem/system-administration-and-maintenance-1/installation/void-linux/","noteIcon":""}
---

### Installation
**Start by using bash.**
```
bash
```
#### Step 1: Disk Partitioning
```bash
lsblk
cfdisk 
# choose gpt or choose a partition
# sdaX 30G - with efi 
```

##### Encrypt Drive
```bash
cryptsetup luksFormat --type luks1 -y /dev/sdaX  # sda5
# open
cryptsetup open /dev/sda5 cryptvoid # name of encrypted disk
```

##### Formatting Partition
```bash
# if efi has not yet setup 
# mkfs.fat -F 32 -n "BOOT" /dev/sdaX #sda1
mkfs.btrfs -L "Void" /dev/mapper/cryptvoid #sda5

```

##### Mounting partition
**Create OPTS variable for general mounting options**
```bash
OPTS="rw,noatime,compress=zstd,discard=async"
```

**Creating sub volumes for BTRFS**
```bash
mount -o $OPTS /dev/mapper/cryptvoid /mnt
# subvolumes
btrfs su cr /mnt/@
btrfs su cr /mnt/@home
btrfs su cr /mnt/@snapshots
umount /mnt
```

**Mounting subvolumes**
```bash

# mount ROOT
mount -o $OPTS,subvol=@ /dev/mapper/cryptvoid /mnt

# mkdir for sub directories
mkdir /mnt/{efi,home,.snapshots}

# mount HOME
mount -o $OPTS,subvol=@home /dev/mapper/cryptvoid /mnt/home

# mount SNAPSHOTS
mount -o $OPTS,subvol=@snapshots /dev/mapper/cryptvoid /mnt/.snapshots

# mount EFI
mount -o rw,noatime /dev/sdaX /mnt/efi # sda1

```

**Additional Directories**
```bash
mkdir -p /mnt/var/cache
btrfs su cr /mnt/var/cache/xbps
btrfs su cr /mnt/var/tmp
btrfs su cr /mnt/srv
```

#### Step 2: Installation
**Create general variables**
```bash
REPO=https://repo-fastly.voidlinux.org/current
ARCH=x86_64
```

**Setup installation database and keys**
```bash
mkdir -p /mnt/var/db/xbps/keys
cp /var/db/xbps/keys* /mnt/var/db/xbps/keys
```

**Install command**
```bash
XBPS_ARCH=$ARCH xbps-install -S -R "$REPO" -r /mnt base-system linux-firmware btrfs-progs cryptsetup vim neofetch 
```

#### Step 3: OS Configuration
**Copy file structure**
```bash
for dir in dev proc sys run; do mount --rbind /$dir /mnt/$dir; mount --make-rslave /mnt/$dir; done

cp /etc/resolv.conf /mnt/etc
```

**change root to /mnt to setup OS configs**
```bash
OPTS=$OPTS PS1='(chroot)# ' chroot /mnt /bin/bash
```

```bash
vim /etc/rc.conf # uncomment keymap

ln -sf /usr/share/zoneinfo/Asia/Manila /etc/localtime

vim /etc/default/libc-locales.conf # uncomment en_US

xbps-reconfigure -f glibc-locales

echo "dejvoid" > /etc/hostname
vim /etc/hosts
# 127.0.1.1       dejvoid.localdomin    dejvoid

```

**New user**
```bash
# change root password
passwd

# add user
useradd dejames
passwd dejames
usermod -aG wheel,video,audio dejames
```

**Edit sudoers file**
```bash
chsh -s /bin/bash root
EDITOR=vim visudo

# uncomment the wheel 
# add: 
# dejames ALL=(ALL:ALL) ALL

```

**Adding repositories**
```bash
xbps-install -S
xbps-install -S void-repo-nonfree
xbps-install -S void-repo-multilib
xbps-install -S



```

#### Step 4: Making File System Table (`FSTAB`)
setup `UUID`'s and variables
```bash
EFI_UUID=$(blkid -s UUID -o /dev/sdaX) #sda1
ROOT_UUID=$(blkid -s UUID -o /dev/mapper/cryptvoid) #sda5
LUKS_UUID=$(blkid -s UUID -o /dev/sdaX) #sda5
```

**Append new table to `fstab` config**
```bash
cat << EOF >> /etc/fstab
UUID=$ROOT_UUID / btrfs $OPTS,subvol=@ 0 1
UUID=$ROOT_UUID /home btrfs $OPTS,subvol=@home 0 2
UUID=$ROOT_UUID /.snapshots btrfs $OPTS,subvol=@snapshots 0 2
UUID=$EFI_ID /efi vfat defaults,noatime 0 2
EOF
```

**Check if table is ok**
```
vim /etc/fstab
```

#### Step 5: GRUB Bootloader Configurtion
**Install GRUB**
```
xbps-install grub-x86_64-efi
```

**Edit grub config**
```bash
vim /etc/default/grub
# append
# GRUB_ENABLE_CRYPTODISK=y
# append to string GRUB_CMDLINE_LINUX_DEFAULT
# rd.auto=1 rd.luks.allow-discards


# OPTIONAL: 
# Uncomment background
# Enable OSPROBER
# GRUB_DISABLE_OS_PROBER=false
```

**Install new grub config to the EFI**
```bash
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=dejvoid_grub
```

**ADD KEYFILE TO BOOT to disable double password entering**
```bash
dd bs=515 count=4 if=/dev/urandom of=/boot/keyfile.bin
```

**add luks key**
```sh
cryptsetup -v luksAddKey /dev/sda5 /boot/keyfile.bin
```

**Disable and modify privileges**
```bash
chmod 000 /boot/keyfile.bin
chmod -R g-rwx,o-rwx /boot
```

**Modify crypttab**
```bash
cat << EOF >> /etc/crypttab
cryptvoid UUID=$LUKS_UUID /boot/keyfile.bin luks
EOF
```
**Modify dracut config**
```bash
echo 'install_items+=" /boot/keyfile.bin /etc/crypttab " ' > /etc/dracut.conf.d/10-crypt.conf
ln -s /etc/sv/dhc /etc/runit/runsvdir/default
ln -s /etc/sv/dhcpd-eth0 /etc/runit/runsvdir/default
ln -s /etc/sv/dhcpd /etc/runit/runsvdir/default
```


#### Finalizing
**Install addition packages**
```
xbps-install -S
xbps-install cmatrix figlet cowsay NetworkManager

```

**Link server files and network**
```bash
ln -s /etc/sv/dhcpcd-eth0 /var/service
ln -s /etc/sv/dhcpcd /var/service
ln -s /etc/sv/NetworkManager /var/service

xbps-reconfigure -fa
```

```bash
exit
shutdown -P now
# remove iso
# reopen machine
# Login
# DONE
```

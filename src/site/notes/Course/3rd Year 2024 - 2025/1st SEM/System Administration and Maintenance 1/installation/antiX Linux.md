---
{"dg-publish":true,"permalink":"/course/3rd-year-2024-2025/1st-sem/system-administration-and-maintenance-1/installation/anti-x-linux/","noteIcon":""}
---


> [!NOTE] Installation
> Just follow the `cli-installer` command and ***Beware of wrong partition***.
> 

**Changing Sources*4 
```bash
vim /etc/apt/sources.list/debian.list
```

**Fix dbus or broken error**
```bash
apt satisfy dbus
```

**Installing XFCE** 
6/10 
good starter
```bash
apt-get install xserver-xorg x11-xserver-utils xfonts-base x11-utils lightdm lightdm-gtk-greeter xfce4 xfce4-goodies
```

```
reboot
```

**Installing LXDE**
2/10 yuck
need shit ton of customization, ugly as f*ck
```bash
	apt install xorg lxde synaptic
```

**Installing IceWM** 
4/10
need more customization but has different themes
```bash
	apt install icewm
```

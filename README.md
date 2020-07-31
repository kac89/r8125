# Archlinux/Manjaro Ethernet Driver for Odroid H2 Plus RTL8125B chipset

I made the necessary changes base on https://aur.archlinux.org/packages/r8125/

# Manjaro Minimal (preparation)
```
$ sudo pacman -Syyu
$ sudo pacman -Sy base-devel
$ sudo pacman -S $(pacman -Qsq "^linux" | grep "^linux[0-9]*[-rt]*$" | awk '{print $1"-headers"}' ORS=' ')
$ reboot
```

# Installation

```
$ git clone https://github.com/kac89/r8125.git
$ cd r8125
$ makepkg
$ ls
$ sudo pacman -U r8125-dkms-9.003.05-0-x86_64.pkg.tar.gz
```
(for the -U parameter use the r8125-dkms-X.XXX.XX-X-x86_64.pkg.tar.gz filename that the ls command has displayed, it can be different)


now blacklist the r8169 module (it wil not be loaded anymore)
```
$ sudo bash -c 'echo "blacklist r8169" > /etc/modprobe.d/r8169.conf'
```

reboot now, the module r8125 should be loaded automatically.
to check it :
```
$ lsmod | grep r8125
```
it should return something like : r8125                 176128  0

if the module was not loaded automatically, you can force it to be loaded on boot:
```
$ sudo bash -c 'echo "r8125" > /etc/modules-load.d/r8125.cfg'
```
now reboot and check if the module is loaded.

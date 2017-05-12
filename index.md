---
layout: default
---
<br>
<iframe src="https://ghbtns.com/github-btn.html?user=avivace&repo=debian-quickstart&type=star&count=true&size=large" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>

## Prepare the installation
### Which ISO?
Even if we plan to install the testing version, we start by installing the stable, and later upgrade.

[firmware-8.8.0-amd64-netinst.iso](http://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/amd64/iso-cd/?C=S;O=D)

If you can't access the network during the install, download the DVD ISO, [debian-8.8.0-amd64-DVD-1.iso](https://cdimage.debian.org/cdimage/release/current/amd64/iso-dvd/).

### Bootable USB key
On Windows, use [win32diskimager](http://sf.net/projects/win32diskimager/).

On Linux, run  
```
sudo dd if=debian.iso of=/dev/sdX
```
Assuming `debian.iso` is our downloaded file and `sdX` is your USB key.

If you downloaded the DVD ISO in the previous phase, extract [firmware.zip](https://cdimage.debian.org/cdimage/unofficial/non-free/firmware/stable/current/) file into the `firmware` folder of the USB key.

Now reboot, disable Secure Boot in BIOS settings and select the USB key as boot device.

## Installation wizard

This part should be easy if you ever installed a Linux distro, but you can take a look at the following links to see detailed explanation of every step:
- [Official Debian GNU/Linux Installation Guide](https://www.debian.org/releases/stable/amd64/ch06s03.html.en#di-setup)
- [Installing Debian](https://github.com/konklone/debian/blob/master/installing.md#installing-debian)
- [Debian Linux Desktop](https://wiki.comprofix.com/index.php?title=Debian_Linux_Desktop#Installation)

## Upgrade to testing
You should now be logged in your shiny new Debian. Open a terminal (the actual name depends on the Desktop Environment you chose, it's *Konsole* on KDE, *MATE Terminal* on MATE, ...) and start typing these things.

Give yourself sudo (replace "\<username>" with your username).
```powershell
su
apt install sudo
adduser <username> sudo
```

Log out and log in to get this to take effect.

Run `sudo nano /etc/apt/sources.list` and replace the contents of the file with the following content:

```powershell
## Official Debian Main Repos
deb http://deb.debian.org/debian/ testing main contrib non-free
deb-src http://deb.debian.org/debian/ testing main contrib non-free

deb http://deb.debian.org/debian/ testing-updates main contrib non-free
deb-src http://deb.debian.org/debian/ testing-updates main contrib non-free

deb http://deb.debian.org/debian-security testing/updates main
deb-src http://deb.debian.org/debian-security testing/updates main
```

Press `CTRL`+`O` and `ENTER` to save and quit.

Now run
```powershell
sudo dpkg --add-architecture i386
sudo apt update
sudo apt full-upgrade -y
sudo apt clean
sudo apt autoremove -y
```
And let `apt` upgrade your system. This will take a while, depending on your connection speed.

### VirtualBox Guest Additions
If you're trying things on a Virtual Machine, mount the Guest Addition drive from the menu and run:
```
cp /media/cdrom0/* vbox/ -r
sudo apt install build-essential dkms linux-headers-$(uname -r) -y
sudo ./vbox/VBoxLinuxAdditions.run
sudo rm -rf vbox
sudo reboot
```
Otherwise, ignore this step.

## Human defaults

Some standard utilities, fonts, a firewall:
```powershell
apt install wget vim-nox git curl pk-update-icon apt-transport-https gdebi build-essential linux-headers-$(uname -r) fonts-freefont-otf otf-freefont fonts-hack-otf ttf-mscorefonts-installer ttf-bitstream-vera ttf-dejavu ttf-liberation fonts-octicons ufw gufw ifconfig tlp neofetch tmux
```

Now we will apply some default configurations files
- [User .bashrc](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.bashrc)
- [Root .bashrc](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.rootbashrc)
- [.fonts.config](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.fonts.conf)
- [tmux](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.tmux.conf)

Running this commands will automatically download and apply them for you:
```powershell
sudo wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.rootbashrc -O ~/root/.bashrc
wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.bashrc -O ~/.bashrc
wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.fonts.conf -O ~/.fonts.conf
wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.tmux.conf -O ~/.fonts.conf
```

Additionaly, `apt install apt-listbugs apt-listchanges` in order to be made aware of grave bugs or important changes when you install new packages or during an upgrade.

## Resources
- [/r/debian threads]()
- [Debian Wiki]()
- [Official Debian GNU/Linux Installation Guide](https://www.debian.org/releases/stable/amd64/ch06s03.html.en#di-setup)
- [Installation Notes #1](https://github.com/konklone/debian/blob/master/installing.md#installing-debian)
- [Installation Notes #2]()
- [Installation Notes #3]()
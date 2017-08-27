---
layout: default
---


## Preamble
This project aims to provide a rolling Debian environment featuring recent packages, stability and confortable defaults for casual users, developers and geeks.

> I'm nowhere near to define myself as a *debian expert*, whatever it could mean, so everything you do following this guide is your and only your responsiblity.

Every type of feedback, comment or suggestion is welcome, please **do contribute**, this page and every suggested configuration is published on [this repository](https://github.com/avivace/debian-quickstart), under a GPLv3 license.

> A big and exciting part of using a Linux environment is about learning, going mad because everything broke and learning more by fixing things. Blindly copy-pasting will make things way worse later on, so \*pretty please\* take a moment to review what we are doing in every step and check the configuration files we are applying, knowledge is just one query away.

There are some important concept and practices you should be aware of:

### Debian releases and versions

#### Stable
The "stable" distribution contains the latest officially released distribution of Debian.

The current "stable" distribution of Debian is version 9, codenamed `stretch`.

#### Testing

The "testing" distribution contains packages that haven't been accepted into a "stable" release yet, but they are in the queue for that. The main advantage of using this distribution is that it has more recent versions of software.

The current "testing" distribution is `buster`.

#### Unstable

The "unstable" distribution is where active development of Debian occurs. Generally, this distribution is run by developers and those who like to live on the edge.

The "unstable" distribution is always called sid.

Please note that "unstable" refers to the versions of the software shipped, not to the stability of the distribution itself.

### Debian testing is not stable

We will be on debian **testing** which is basically another distribution, compared to the stable release.

Being on debian testing will give more bleeding edge software and recent versions. Testing is generally very stable - compared to other distributions - and rarely breaks. E.g. Ubuntu is based on Debian unstable and its LTS version on Debian testing. Anyway, reading [What are some best practices for testing/sid users?](https://wiki.debian.org/ DebianUnstable#What_are_some_best_practices_for_testing.2Fsid_users.3F) will give some useful general informations.

You should also pay attention running `apt full-upgrade` on testing and unstable, because during migrations or major upgrades, a lot of packages maybe unistalled, leaving you with a broken system. Always read what `apt` wants to do, before confirming.

### APT Pinning
A very common practice allowing the mixing of sources and repositories on Debian. In this page we will add `testing` and `unstable` repositories in the APT sources but we will give more priority to the `testing` one, meaning we will install and upgrade everything from that, and only for specific packages (or not available in the `testing` one) apt will consider the `unstable` source.
Firefox stable is available only on the `unstable` distribution, so we pinned the firefox package to that source.

In the "Upgrade to testing" paragraph, check the `sources.list` and `preferences` file we are applying to get this result.

- [A short introduction to APT pinning](https://www.howtoforge.com/a-short-introduction-to-apt-pinning)
- [APT pinning for beginners](http://jaqque.sbih.org/kplug/apt-pinning.html)


### --only-upgrade
```
apt-get install --only-upgrade <packagename>
```
This command will upgrade only that single package - if it is installed - and won't install any other package.


## Prepare the installation
You may want to try things on a Virtual Machine, first.

### Download the ISO
We will start by installing debian stable, and later upgrade if you need more recent package versions.

[firmware-9.1.0-amd64-netinst.iso](http://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/amd64/iso-cd/firmware-9.1.0-amd64-netinst.iso)

If you can't access the network during the install, download [this](http://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/amd64/iso-dvd/firmware-9.0.0-amd64-DVD-1.iso) image instead.

> Debian has these divisions in the repositories:
> 
> 1) main: All free software that follows the DFSG ([Debian Free Software Guidelines](https://en.wikipedia.org/wiki/Debian_Free_Software_Guidelines))
>
>2) contrib: Free software that follows DFSG but depends on software in non-free.
>
>3) non-free: All kinds of non-free software that doesn't follow the DFSG.
>
>We are using the unofficial "non-free" ISO of debian, which includes non-free  and contrib software and guarantees compatibility with a lot of hardware. 

### Bootable USB key
On Windows, use [win32diskimager](http://sf.net/projects/win32diskimager/).

On Linux, run  
```
sudo dd if=debian.iso of=/dev/sdX
```
Assuming `debian.iso` is our downloaded file and `sdX` is your USB key.

Now **reboot**, disable Secure Boot in BIOS settings and select the USB key as boot device.

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
exit
```

**Log out** and log in to get this to take effect.

We will now add our apt [sources.list](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/sources.list) with the testing repositories, add x86 as supported architecture and upgrade the system:

Run
```powershell
sudo rm /etc/apt/sources.list
sudo wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/sources.list -O /etc/apt/sources.list
sudo wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/preferences -O /etc/apt/preferences
sudo dpkg --add-architecture i386
sudo apt update
sudo apt full-upgrade -y
sudo apt clean
sudo apt autoremove -y
```

This can take a while, depending on your connection speed.

### VirtualBox Guest Additions
If you're trying things on a Virtual Machine, mount the Guest Addition drive from the menu and run `mkdir vbox && cp /media/<CDROM>/* vbox/ -r` (replace <CDROM> with the correct drive, should be `cdrom0` or `cdrom`), then:
```
sudo apt install build-essential dkms linux-headers-$(uname -r) -y
sudo ./vbox/VBoxLinuxAdditions.run
sudo rm -rf vbox
sudo reboot
```
Otherwise, ignore this step.

## Human defaults

Some standard utilities, fonts, a firewall:
```powershell
sudo apt install wget vim-nox git curl pk-update-icon apt-transport-https gdebi build-essential linux-headers-$(uname -r) fonts-freefont-otf otf-freefont fonts-hack-otf ttf-mscorefonts-installer ttf-bitstream-vera ttf-dejavu ttf-liberation fonts-octicons ufw gufw tlp neofetch tmux debian-keyring
```

Now we will apply some default configurations files
- [User .bashrc](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.bashrc)
- [Root .bashrc](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.rootbashrc)
- [.fonts.config](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.fonts.conf)
- [tmux](https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.tmux.conf)

Running these commands will automatically download and apply them for you:
```powershell
sudo wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.rootbashrc -O ~/root/.bashrc
wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.bashrc -O ~/.bashrc
wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.fonts.conf -O ~/.fonts.conf
wget https://raw.githubusercontent.com/avivace/debian-quickstart/master/defaults/.tmux.conf -O ~/.tmux.conf
```

Additionaly, `apt install apt-listbugs apt-listchanges` in order to be made aware of grave bugs or important changes when you install new packages or during an upgrade.

### Chromium
```
sudo apt install chromium chromium-l10n chromium-widevine
```

### Firefox
```
sudo apt install firefox
```


## We did it
<center>
<blockquote><i><p style="font-family: 'Libre Baskerville' "> Enjoy the feeling of running a beautiful, powerful computer maintained by a global community of dedicated, passionate people that believe in a world of free software.</p></i> </blockquote></center>

## FAQ
### Debian releases
> Is *jessie* the stable? If I have "stable" in my sources.list what release am I tracking?

Read [SourcesList](https://wiki.debian.org/SourcesList) and [DebianReleases](https://wiki.debian.org/DebianReleases).


## Community & further readings
You're now on your own. Or not. There are plenty of places to ask for help and learn a lot about debian:

- [Debian irc](https://wiki.debian.org/GettingHelpOnIrc)
- [Debian subreddit](https://reddit.com/r/debian)
- [Stack Exchange tag "debian"](https://unix.stackexchange.com/questions/tagged/debian)
- [Don't break debian](https://wiki.debian.org/DontBreakDebian)
- [DebianTesting](https://wiki.debian.org/DebianTesting)
- [Debian Wiki](https://wiki.debian.org/)

### Sources
- [/r/debian](https://www.reddit.com/r/debian)
- [Official Debian GNU/Linux Installation Guide](https://www.debian.org/releases/stable/amd64/ch06s03.html.en#di-setup)
- [Installation Notes](https://github.com/konklone/debian/blob/master/installing.md#installing-debian)

### Related distributions
These supported distributions are based on Debian and provide updated software out of box:

- [SparkyLinux](https://sparkylinux.org/) A distro based on the testing branch of debian, providing a rolling release and a repository of additional applications.

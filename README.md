# Pinephone

Example Pinephone install, apps and usage.

There are of course many distributions and desktop environments to choose from.
Here I used Mobian (Bookworm) with Phosh since most things just worked (like sms, camera), I had some problems with other distros but was a while ago now.

Note possible differences between releases HW https://wiki.pine64.org/wiki/PinePhone#Hardware_revisions, e.g. v1.1 discharges even when turned off (https://wiki.pine64.org/wiki/PinePhone_1.1_VBUS_power_usage_Hardware_Fix)

# Install OS and bootloader

Mobian (Bookworm) with Phosh (wayland), https://wiki.mobian-project.org/doku.php?id=install-linux

Boot options, something like this (?);

```
power+volume up -> bootloader
power+volume down -> SD card
power -> eMMC
```

1. Flash bootloader if not present, Tow-boot: https://wiki.mobian-project.org/doku.php?id=tow-boot

2. Flash OS. I think this is the best way;

Flash OS image onto SD card. Then boot SD card with power+volume down. This will boot installation image from SD card, with presents different options and e.g. possiblity of disk encryption.

OS Image, https://images.mobian-project.org/pinephone/installer/weekly/
Some images where broken for me, so I decided on this https://images.mobian-project.org/pinephone/installer/weekly/mobian-installer-pinephone-phosh-20220522.img.gz
(Still, on this image release gpg key is outdated and need to be updated to be able to run apt update in OS. Do a simple search on the error when it comes.)

Remember to apt update and upgrade!



# System apps

## General tips

https://forum.pine64.org/showthread.php?tid=13861


## Powersave

IDK! Not checked. Long battery as long as it is in suspend (=no internet but will wake on sms and phone calls)

custom kernel https://forum.pine64.org/showthread.php?tid=17339

## Internet, texts, calls, mms

Sms, calls, and 4G worked out of the box for me. I think I tried with Tre and Telia carrier networks in Sweden. MMS needed manual configuration and didn't work when tested, at least for Telia.

### Calls and SMS

Absolutely no complains, or issues, the few times I tested on 4G

#### Audio
To increase call receive volume, /usr/share/alsa/ucm2/PinePhone/VoiceCall.conf

### MMS

Edit:  "Currently there is no easy solution for sending or receiving MMS if carrier has two APNs (one for data and for MMS). There are some workarounds but most likely best one is to not use MMS at all before things get"
        The issue is here https://gitlab.com/kop316/mmsd/-/issues/5

General: https://wiki.mobian-project.org/doku.php?id=mms https://forum.pine64.org/showthread.php?tid=16754

In general is supported, go to chatty (default sms app phosh) and set settings e.g. (Telia which doesnt work due to different APNs, APN: mms.telia.se MMSC: http://mmss
MMS-proxy: 193.209.134.132:80)

```
### Debug
systemctl --user status mmsd-tng

### Modify settings (or do it directly in Chatty)
systemctl --user stop mmsd-tng
vim $HOME/.mms/modemmanager/mms
systemctl --user start mmsd-tng
```

## Alarm app (that works in suspend)

Default alarm app does not work in suspend (lol!), so try this one instead.

Birdie: Go here on phone and just build: https://github.com/Dejvino/birdie (https://wiki.mobian.org/doku.php?id=birdie).

(At least it seems to be a popular app for Pinephone, sudo install and hope I won't get hacked :P)

```
# Build instructions 2022-10-17
git clone https://github.com/Dejvino/birdie.git
cd birdie
# Mobian/Debian specific
sudo apt install gcc make checkinstall python3-psutil python3-pip
pip3 install -r requirements.txt
sudo make install-deb
# Now launchable via desktop shortcut
```

## Screenshot

Need to install appropriate app and fix key binding or whatever. https://wiki.mobian-project.org/doku.php?id=tweaks#taking-screenshots

scrot did not work for me (black image), a similar suggestion is gnome-screenshot

```
sudo apt install gnome-screenshot
scale-to-fit gnome-screenshot 
```

TODO how to add as a reasonable shortcut?




## Apps

Note that nothing (except sms, mms?, and phone calls) will be sent or received when phone is in suspend (default after 5min). This greatly increases battery life however.
To wake phone up periodically try some 'sleepwalk' script, like https://github.com/milky-sway/pinephone-scripts, and that way get any notifications.


### Signal-desktop

Build from source, can be difficult to compile, but latest beta (2022-10-11) was easy to build (I built on another arm64 device).
Still I did NOT manage to build a .deb file, only the binary directly (TODO) ... but it works (need to create shortcut manually TODO).

NOTE: if Signal-desktop crashes on your device make sure that you have an updated system. It failed to launch on my older, outdated, Mobian Bullseye.

NOTE: if installation does not work have a look on my complete notes <https://github.com/Eliot-Roxbergh/pinephone_notes/blob/main/extras_build_signal-desktop.md>
 
#### Prerequisites

```
sudo apt install -y git git-lfs npm curl build-essential gcc make
# Install NVM (don't trust me that this is secure)
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.bashrc
nvm install 16.15.0 #this might change ofc, otherwise run nvm use in signal-desktop
```
#### Build Signal-Desktop arm64
```
git clone https://github.com/signalapp/Signal-Desktop.git 
cd Signal-Desktop
git checkout v5.63.0-beta.3 #lastest beta
git-lfs install
nvm use

yarn install --frozen-lockfile --arch=arm64 --network-timeout 600000

SIGNAL_ENV=production yarn build:electron --arm64 --linux --dir --config.directories.output=release #(add deb after Linux for .deb, but I get errors on it right now.)
# OR # yarn build:dev && yarn build:release --arm64 --linux #deb 

```


### Telegram-desktop

sudo apt install telegram-desktop

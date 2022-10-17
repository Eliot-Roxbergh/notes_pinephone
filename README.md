# Pinephone

Example Pinephone install, apps and usage.

There are of course many distributions and desktop environments to choose from.
Here I used Mobian (Bookworm) with Phosh since most things just worked (like sms, camera), I had some problems with other distros but was a while ago now.

Note possible differences between releases HW https://wiki.pine64.org/wiki/PinePhone#Hardware_revisions, e.g. v1.1 discharges even when turned off (https://wiki.pine64.org/wiki/PinePhone_1.1_VBUS_power_usage_Hardware_Fix)

Development status of Pinephone etc. https://xnux.eu/devices/pine64-pinephone.html

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

## Camera

Photos works OK. Front camera is not very good and very yellow hue. Back camera is good if you compare to a phone from like 2012 (I would say it's better than Samsung S3 for instance), so it is acceptable perhaps (and flash works). However, AFAIK taking videos is not supported!!

Signal-desktop did not find the camera at all (but it seems like camera starts focus considering the sound? idk).

## Flashlight
Part of system UI, works.


## Internet, texts, calls, mms

Sms, calls, and 2G 3G 4G worked out of the box for me. I (quickly) tried with Tre and Telia carrier networks in Sweden (2022-10). MMS needed manual configuration and data/MMS APN needs to be the same (so e.g. Telia doesn't work), MMS worked on Tre carrier.

### General 

Telia and Tre carriers (although Tre has no 2G support on their side ofc);
Everything seems to work with the carriers tested, with three exceptions. 1. Calls doesn't work on 4G. 2. 2G coverage is much worse than other phones it looks like (2G calls and data is supported). 3. MMS works, except for Telia which had two APNs (no support!).

Compared to other phones, 4G 3G coverage is equivalent it looks like, however 2G coverage is quite a bit worse.

(TODO fill in new carriers on wiki pages)

https://wiki.pine64.org/wiki/PinePhone_Carrier_Support

https://wiki.pine64.org/wiki/PinePhone_APN_Settings


### Calls and SMS

tl;dr When tested, calls works on 2G and 3G. Texing works on all networks.

Applies to carriers Tre (altough they have no 2G ofc) and Telia:
Calls work very well on 3G. But it seems like it doesn't want to connect on 4G (peculiar considering data works), either a text is received for the missed call (Telia) or it says for caller that you're "busy" (!) (Tre). 2G works, although there is some (not too loud) constant interference on the speakers, also in case of bad coverage (or maybe it was a coincidence?) there might be bad interference getting transmitted to the other party.

Comment: When I tested 2G, hot swapping headset, I managed to crash mic? Regardless it did not work in calls, with headset or not before restart.

Received and transmitted sound is generally good.
It is possible to plug-in and remove headset during conversation, etc. All working well, (except for that one time during call that all mics stopped working... remained even after connecting a headset and also after redialing).


#### Firmware

For custom modem firmware look here, https://github.com/the-modem-distro/pinephone_modem_sdk

#### Audio
To increase call receive volume, /usr/share/alsa/ucm2/PinePhone/VoiceCall.conf

### MMS

tl;dr In general is supported, go to chatty (default sms app phosh) and set settings manually. If it doesn't work make sure Mobile Data APN is same as MMS APN (and supported by carrier).

Works on most carriers, but need to set settings manually. For instance, works in Sweden with Tre, does not work with Telia.

Telia does not work, why? Well because -> _"Currently there is no easy solution for sending or receiving MMS if carrier has two APNs (one for data and for MMS). There are some workarounds but most likely best one is to not use MMS at all before things get"_
        The issue is here https://gitlab.com/kop316/mmsd/-/issues/5

General: https://wiki.mobian-project.org/doku.php?id=mms https://forum.pine64.org/showthread.php?tid=16754


Swedish Tre-based carriers does work, data APN (add in settings->Mobile Network->Access Point Names) is same as mms. In case of Tre, APN in both cases is data.tre.se.
Tre MMS settings -> APN: data.tre.se, MMSC: http://mms.tre.se, MMS proxy: mmsproxy.tre.se:8799


```
### Debug
systemctl --user status mmsd-tng

### Modify settings (or do it directly in Chatty)
systemctl --user stop mmsd-tng
vim $HOME/.mms/modemmanager/mms
systemctl --user start mmsd-tng
```

## Alarm app (that works in suspend)

Default alarm app does not work in suspend (lol!), so use this one instead. It did work when I tried it, but UI is slow.

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

## Terminal

System comes with Console (terminal app). It works. To paste text, long press (on touch screen) and _at the same time_, with the other hand, press 'paste' on the menu that appears. Ctrl+v on virtual keyboard doesn't seem to work for some reason, maybe it wants ctrl+shift+v which keyboard doesn't want to send?



## Apps

Note that nothing (except sms, mms?, and phone calls) will be sent or received when phone is in suspend (default after 5min). This greatly increases battery life however.
To wake phone up periodically try some 'sleepwalk' script, like https://github.com/milky-sway/pinephone-scripts, and that way get any notifications.


### Signal-desktop

Signal-desktop works ok, but not made for small screen: so zoom out in the menu and it _should_ be possible to contract the contact menu by dragging (maybe if you plug in mouse, it's literally 1 px).

Build from source, can be difficult to compile, but latest beta (2022-10-11) was easy to build (I built on another arm64 device).
Still I did NOT manage to build a .deb file, only the binary directly (TODO) ... but it works (need to create shortcut manually TODO). TODO try if it's easier on x86 (might not be though), considering build .deb file uses x86_32 dependency for some reason (First Docker link gave some sort of .deb, so should work on arm64!) .

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

Telegram works well, no complains after quick test.

```
sudo apt install -y telegram-desktop
```

### Browsing

Firefox is OK, but takes like 5 seconds to start. Video performance is bad, if you're lucky you can run 360p video smoothly. HW acceleration working?
Sometimes menus don't show and only flicker, but generally it is usable and e.g. possible to go into settings, clear cookies etc.

# Security

Pinephone has hw switches to disable mic / camera 1 / camera 2 / wifi / cellular.

I do not think firmware and required apps is 100% open-source (cellular is not ofc but more?). And for instance, regarding Pinephone Pro "_ppp-cam app itself will stay closed source_" (https://xnux.eu/log/#toc-2022-06-23-further-pinephone-pro-camera-development)

Note that, in general, stock Linux does not have the same application sandboxing as e.g. Android. 

"_USB-OTG is very permissive . The MTP service currently has no security whatsoever; if the phone is plugged into a computer, even with disk encryption, the computer will have full R/W access to your /home dir, and full read access to /. See Services on how to disable and enable only when needed._" - https://wiki.mobian-project.org/doku.php?id=pinephone

Offtopic: it is possible to change phone IMEI apparently, https://forum.pine64.org/showthread.php?tid=14743


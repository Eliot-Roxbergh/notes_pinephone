# Pinephone

Some notes on Pinephone installation, apps and use. But I have not used this device a lot!

There are of course many distributions and desktop environments to choose from.
Here I used Mobian (Bookworm) with Phosh since most things just worked (like sms and camera), I had some problems with other distros but was a while ago now.

For development status of Pinephone etc. see [1]

There is also a more powerful version Pinephone Pro, however now ~Q4 2022 it is not usable as a daily phone [2].


[1] - https://xnux.eu/devices/pine64-pinephone.html \
[2] - https://wiki.pine64.org/wiki/PinePhone_Pro#State_of_the_software 

## TL;DR

**How to buy**

The phone works and only costs 199$ (total to Sweden 270$ incl. VAT) (quad core, 3GB ram) [1]. There are also accessories like an attachable keyboard [2] as well as replacement parts and batteries.

Only purchase if you have some Linux experience.

If you buy used or on sale, make sure to get the latest board revision (currently 1.2b).
Note the differences between releases HW [3], e.g. v1.1 discharges even when turned off ([4]) and I think it cannot use USB-C hubs? In case you have an older version you probably want to buy the most recent mainboard in the pine64 store [5], to replace it and get rid of these problems (and to get 3GB ram from 2GB).

[1] - https://pine64.com/product/pinephone-beta-edition-with-convergence-package/ \
[2] - https://pine64.com/product/pinephone-pinephone-pro-keyboard-case/ \
[3] - https://wiki.pine64.org/wiki/PinePhone#Hardware_revisions \
[4] - https://wiki.pine64.org/wiki/PinePhone_1.1_VBUS_power_usage_Hardware_Fix \
[5] - https://pine64.com/product/pinephone-community-edition-3gb-32gb-mainboard/


-----

**Does it work as a phone**

For me it worked with SMS, MMS, calls with minor configuration. (MMS only works with carriers who has same the data APN and MMS APN).

Battery time is good in suspend mode, and can take calls.

It is nice with hardware switches if one wishes to turn off mic, back or front camera, etc.

-----

**Does it work as a smartphone**

(Desktop) Apps often work OK but of course it is a bit cumbersome compared to a smartphone, most apps are not made for a phone.
It might be possible to run Android apps via software such as Waydroid or Anbox, I guess.

It is slow! Let's say performance similar to Samsung S3, with the downside that apps are not optimized for it. Still it works for most regular browsing, chat apps, etc. Video streaming is VERY SLOW however, fullscreen 360p Youtube videos in Firefox is lagging but playable (I'm not sure if this can be improved with HW acceleration or something). The successor, Pinephone Pro, has much better performance, which might be a good upgrade once it has matured.

Camera is badish and cannot take videos (?). Still it works as a camera.

Battery time is very bad (max 1-2h of heavy use?), but is quite good in suspend mode: in which it turns off data and only wakes on calls or sms/mms. So battery time is quite good if you utilize suspend and use it as a phone, but during suspend you won't get any other notifications, nor able play music etc.


-----

**Does it work as a computer**

TODO. It should work to directly attach (USB-C hub) keyboard, mouse, and even HDMI screen, to utilize the phone as a desktop computer as well. Might be sufficient for lite work, browsing, reading, and coding.


-----

# Install OS and bootloader

Mobian (Bookworm) with Phosh (wayland), [1].

Boot options, something like this (?);

```
power+volume up -> bootloader
power+volume down -> SD card
power -> eMMC (sometimes SD card)
```

1. Flash bootloader if not present, Tow-boot [2]

2. Flash OS. I think this is the best method;

Flash OS image onto SD card. Then boot SD card with power+volume down. This will boot installation image from SD card, with presents different options and e.g. possiblity of disk encryption.

OS Image, [3]
Some images where broken for me, so I decided on this for now [4].
(Still, on this image release gpg key is outdated and need to be updated to be able to run apt update in OS. Do a simple search on the error when it comes.)

Remember to apt update and upgrade!

[1] - https://wiki.mobian-project.org/doku.php?id=install-linux \
[2] - https://wiki.mobian-project.org/doku.php?id=tow-boot \
[3] - https://images.mobian-project.org/pinephone/installer/weekly/ \
[4] - https://images.mobian-project.org/pinephone/installer/weekly/mobian-installer-pinephone-phosh-20220522.img.gz

# System apps

## General tips

General: https://forum.pine64.org/showthread.php?tid=13861 


## Powersave and Charging

Contrary to popular belief, you can charge the phone while it's turned off; When plugged in the phone will automatically boot, but turn it off again and it will remain off and charging.

When on, the phone charges somewhat slowly. Get a poweful charger, _"The PinePhone should be charged using a 15W (5V 3A) USB -PD power adapter"_ (citation needed). Some comments on phone charging hardware here, [1].

I would say that the most convenient way of charging is to put the phone in suspend mode. That way you can still receive calls/sms and set alarms, while charing much faster (by using less power). Suspend when charging must be enabled in settings.

By the way, _"You can use PinePhone without the battery inserted if you can provide enough power over USB port. Modem and WiFi and flash light are connected to the battery directly (almost) so these will not work without the battery inserted."_ [1]

#### Powersaving

I have not looked into custom powersaving hacks. Out of the box it does have long battery life as long as it is in suspend (=no internet but will wake on sms and phone calls), otherwise during heavy use maybe 1-3h.

custom kernel [2].

[1] - https://xnux.eu/howtos/pine64-pinephone-getting-started.html \
[2] - https://forum.pine64.org/showthread.php?tid=17339 


## Camera

Photos works OK. Front camera is not very good and very yellow hue. Back camera is good if you compare to a phone from like 2012 (I would say it's maaybe better than Samsung S3 for instance), so it is acceptable perhaps (and flash works). However, AFAIK taking videos is not supported!!

Signal-desktop did not find the camera at all (but it seems like camera starts focus considering the sound? idk).

## Flashlight
Part of system UI, works.


## Internet, texts, calls, mms

Sms, calls, and 2G 3G 4G worked out of the box for me. I (quickly) tried with Tre and Telia carrier networks in Sweden (2022-10). MMS needed manual configuration and data/MMS APN needs to be the same (so e.g. Telia doesn't work), MMS worked on Tre carrier.
Calls over 4G (i.e. VoLTE) did not work out of the box for me, is there manual configuration for this needed?

### General 

Telia and Tre carriers (although Tre has no 2G support on their side ofc);
Everything seems to work with the carriers tested, with three exceptions. 1. Calls doesn't work on 4G. 2. 2G coverage is much worse than other phones it looks like (2G calls and data is supported). 3. MMS works, except for Telia which had two APNs (no support!).

Compared to other phones, 4G 3G coverage is equivalent it looks like, however 2G coverage is quite a bit worse.

Quick speedtest on 4G with moderate/poor coverage, 8mbit/s.

(TODO fill in new carriers on wiki pages)

https://wiki.pine64.org/wiki/PinePhone_Carrier_Support

https://wiki.pine64.org/wiki/PinePhone_APN_Settings


### Calls and SMS

tl;dr When tested, calls works on 2G and 3G. Texing works on all networks.

Applies to carriers Tre (altough they have no 2G ofc) and Telia:
Calls work very well on 3G. But it seems like it doesn't want to connect calls on 4G (VoLTE should, in general, be possible but might need to explore.. see [1] [2]), either a text is received for the missed call (Telia) or it says for caller that you're "busy" (!) (Tre). 2G works, although there is some (not too loud) constant interference on the speakers, also in case of bad coverage (or maybe it was a coincidence?) there might be bad interference getting transmitted to the other party.


Comment: When I tested 2G, hot swapping headset, I managed to crash mic? Regardless it did not work in calls, with headset or not before restart.

Received and transmitted sound is generally good.
It is possible to plug-in and remove headset during conversation, etc. All working well, (except for that one time during call that all mics stopped working... remained even after connecting a headset and also after redialing).

[1] - https://wiki.postmarketos.org/wiki/PINE64_PinePhone_(pine64-pinephone)#VoLTE \
[2] - https://wiki.mobian-project.org/doku.php?id=pinephone

#### Firmware

For custom modem firmware look here, [1].

[1] - https://github.com/the-modem-distro/pinephone_modem_sdk 

#### Audio
To increase call receive volume, /usr/share/alsa/ucm2/PinePhone/VoiceCall.conf

### MMS

tl;dr In general is supported, go to chatty (default sms app phosh) and set settings manually. If it doesn't work make sure Mobile Data APN is same as MMS APN (and supported by carrier).

Works on most carriers, but need to set settings manually. For instance, works in Sweden with Tre, does not work with Telia.

Telia does not work, why? Well because -> _"Currently there is no easy solution for sending or receiving MMS if carrier has two APNs (one for data and for MMS). There are some workarounds but most likely best one is to not use MMS at all before things get"_
        The issue is here https://gitlab.com/kop316/mmsd/-/issues/5


Swedish Tre-based carriers does work, data APN (add in settings->Mobile Network->Access Point Names) is same as mms. In case of Tre, APN in both cases is data.tre.se.
Tre MMS settings -> APN: data.tre.se, MMSC: http://mms.tre.se, MMS proxy: mmsproxy.tre.se:8799


MMS general links: https://wiki.mobian-project.org/doku.php?id=mms https://forum.pine64.org/showthread.php?tid=16754

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

Birdie: Go here on phone and just build: https://github.com/Dejvino/birdie [1].

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

[1] - https://wiki.mobian.org/doku.php?id=birdie 

## Screenshot

Need to install appropriate app and fix key binding or whatever. [1]

scrot did not work for me (black image), a similar suggestion is gnome-screenshot

```
sudo apt install gnome-screenshot
scale-to-fit gnome-screenshot 
```

TODO how to add as a reasonable shortcut?

[1] - https://wiki.mobian-project.org/doku.php?id=tweaks#taking-screenshots 

## Terminal

System comes with Console (terminal app). It works. To paste text, long press (on touch screen) and _at the same time_, with the other hand, press 'paste' on the menu that appears. Ctrl+v on virtual keyboard doesn't seem to work for some reason, maybe it wants ctrl+shift+v which keyboard doesn't want to send?



## Apps

Note that nothing (except sms, mms?, and phone calls) will be sent or received when phone is in suspend (default after 5min). This greatly increases battery life however.
To wake phone up periodically try some 'sleepwalk' script, like [1], and that way get any notifications.

[1] - https://github.com/milky-sway/pinephone-scripts 

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

Telegram works well, no complains after quick test. Note, this is desktop app so no possibility for end-to-end encryption (Telegram sucks)!!

```
sudo apt install -y telegram-desktop
```

### Browsing

Firefox is OK, but takes like 5 seconds to start. Video performance is bad, if you're lucky you can run 360p video smoothly. Is HW acceleration available or working? Heard here that video is currently done in SW (2020-08-20 https://youtu.be/jdl1x3DkMEg?t=1256).

Sometimes menus don't show and only flicker, but generally it is usable and e.g. possible to go into settings, clear cookies, install addons etc.

# Security

Pinephone has hw switches to disable mic / camera 1 / camera 2 / wifi / cellular.

I do not think firmware and required apps is 100% open-source (cellular is not ofc but more?). And for instance, regarding Pinephone Pro "_ppp-cam app itself will stay closed source_" [1]

Note that, in general, stock Linux does not have the same application sandboxing as e.g. Android. 

"_USB-OTG is very permissive . The MTP service currently has no security whatsoever; if the phone is plugged into a computer, even with disk encryption, the computer will have full R/W access to your /home dir, and full read access to /. See Services on how to disable and enable only when needed._" [2].

Offtopic: it is possible to change phone IMEI apparently [3].

[1] - https://xnux.eu/log/#toc-2022-06-23-further-pinephone-pro-camera-development \
[2] - https://wiki.mobian-project.org/doku.php?id=pinephone \
[3] - https://forum.pine64.org/showthread.php?tid=14743 

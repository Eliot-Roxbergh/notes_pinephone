# Pinephone

Some notes on Pinephone installation, apps and use. But I have not used this device a lot!

There are of course many distributions and desktop environments to choose from.
Here I used Mobian (Bookworm) with Phosh since most things just worked (like sms and camera), I had some problems with other distros but was a while ago now.

For development status of Pinephone etc. see [1]

There is also a more powerful version Pinephone Pro, however now ~Q4 2022 it is not usable as a daily phone [2].

I still have some questions and issues, see [questions_and_bugs.md](questions_and_bugs.md)

[1] - https://xnux.eu/devices/pine64-pinephone.html \
[2] - https://wiki.pine64.org/wiki/PinePhone_Pro#State_of_the_software 

## TL;DR

**Why Pinephone?**

- It should work as a phone (calls, sms, mms). Also internet, wifi, and a poor camera. Maybe even GPS works (it should work but might not be very precise).

- Regular free software, such as GNU/Linux on a phone. Easy to use with the same programs as on desktop Linux. You are root and have full control. Easy to script and customize (if you have time), make changes and push patches upstream. Regular features, such as full disk encryption is available.

- With USB-C it can be attached to external peripherals (e.g. keyboard, mouse), ethernet, and screen - making the phone also usable as a slower desktop, laptop or tablet computer (the graphics are very sluggish). In this manner (or via SSH) you can also more easily configure and debug the phone.

- Kill switches.

- It has 3.5mm jack

- Moderately cheap at 199$.

- _Mostly_ free-software system and drivers [1].

- It shows promise; Slow HW but future releases (such as Pinephone Pro) will be faster.
Except for the basic apps, the software is badly optimized for such a small screen. But maybe future software releases will be better (it has visably improved between 2020 and 2022).

[1] - https://www.pine64.org/2020/01/24/setting-the-record-straight-pinephone-misconceptions

**How to buy**

The phone works and only costs 199$ (total to Sweden 270$, incl. VAT and shipping) (quad core, 3GB ram) [1]. There are also accessories like an attachable keyboard [2] as well as replacement parts and batteries.

Only purchase if you have some Linux experience.

If you buy used or on sale, make sure to get the latest board revision (currently 1.2b).
Note the differences between releases HW [3], e.g. v1.1 discharges even when turned off ([4]) and it cannot use USB-C hubs w/o hardware hack. In case you have an older version you probably want to buy the most recent mainboard in the pine64 store [5], to replace it and get rid of these problems (and to get 3GB ram from 2GB).

[1] - https://pine64.com/product/pinephone-beta-edition-with-convergence-package/ \
[2] - https://pine64.com/product/pinephone-pinephone-pro-keyboard-case/ \
[3] - https://wiki.pine64.org/wiki/PinePhone#Hardware_revisions \
[4] - https://wiki.pine64.org/wiki/PinePhone_1.1_VBUS_power_usage_Hardware_Fix \
[5] - https://pine64.com/product/pinephone-community-edition-3gb-32gb-mainboard/


-----

**Does it work as a phone**

For me it worked with SMS, MMS and calls after with minor configuration. (MMS only works with carriers who has same the data APN and MMS APN).

Battery time is good in suspend mode, and wakes on sms or call.
In general it seems to accept calls, wake from suspend. Downside is somewhat bad 2G and 4G call quality when it comes to sound. Echo issues, but (3G and) 4G calls are OK with headphones.

It is nice with hardware switches if one wishes to turn off mic, back or front camera, etc.

I have had issues that sometimes the screen won't wake from suspend, or audio temporarily stops working which can be annoying for a phone.


| Swedish carrier | sms | 2G, 3G, and 4G internet | 2G and 3G calls | 4G calls (VoLTE) | mms |
| --------------- | --------------- | --------------- |--------------- |--------------- |--------------- |
| Fello (/Telia based carriers) | Y | Y | Y | Y [1]| NO! [2] |
| Hallon (/Tre based carriers) | Y | Y (carrier doesn't support 2G at all)| Y (carrier doesn't support 2G at all) | Y [1] | Y |
| Comviq (/Tele2 based carriers) | Y | Y | Y | Y [3][4] | Y |
| Telenor based carriers| ? | ? | ? | ? | NO! (should not work) [2] |

[1] - _Only_ after updating (ADSP) firmware and installing modem sdk (using firmware version 01.003.01.003) \
[2] - Because internet APN != mms APN \
[3] - ONLY tested /w modem sdk and newest firmware version 01.003.01.003 \
[4] - Seems to work well now. But first during tests, some calls were not connected (got text msg instead), or calls failed efter ~10s. Now after reboot it seems to work well. IDK, might be fine. \


-----

**Does it work as a smartphone**

(Desktop) Apps often work OK but of course it is a bit cumbersome compared to a smartphone, most apps are not made for a phone.
And some apps might work well in portrait mode but does not in landscape mode (screen orientation).
It might be possible to run Android apps (but probably too bothersome and slow!) via software such as Waydroid or Anbox.

It is slow! Let's say performance similar to Samsung S3, with the downside that apps are not optimized for it. Still it works for most regular browsing, chat apps, etc. Video streaming is VERY SLOW however, fullscreen 360p Youtube videos in Firefox playable (HW acceleration is not supported in browser yet, so it is not very effective), local files play fine in full 720p. The successor, Pinephone Pro, has much better performance, which _might_ be a good upgrade once it has matured.

Camera is badish and cannot take videos (?). Still it works as a camera.

Battery time is bad (1-2h of heavy use, 4-5h moderate use or listening to music), but is quite good in suspend mode: in which it turns off data and only wakes on calls or sms/mms. So battery time is quite good if you utilize suspend and use it as a phone, but during suspend you won't get any notifications from other apps like you would from a smartphone. Note, regular apps like timer and alarm won't work either in suspend: need to get special apps that wakes the phone from suspend with a timer (e.g. Birdie alarm app described later)!


-----

**Does it work as a computer**

It is possible to directly attach (via USB-C hub) keyboard, mouse, and HDMI screen (etc.), to utilize the phone as a desktop computer as well. The graphics were extremely slow in 1440p, perhaps a lighter desktop environment is required or HW acceleration. Can of course do simple things such as writing in a text editor.

I had some issues that after unplugging USB-C it would not redetect later, and required restart. (Have not debugged further)


-----

**Links**

Mobian FAQ, https://wiki.mobian.org/doku.php?id=faq \
Resources on Linux phones and software/distributions, https://linmob.net/resources/ \
Linux phone apps (with rating how well they fit on phone), https://linuxphoneapps.org/

# Install OS and bootloader

**If screen is blank:** If the phone has no OS or bootloader. It is normal that the screen is completely blank, until you plug in a bootable sd-card and hold power+volume up (is this correct?), i.e. flash Tow-boot as described in step 1 below. It is also possible that the phone cannot charge if it has no OS, this is also normal (it only supports regular simple chargers that "implements BC1.2 spec correctly" according to IRC chat).

Mobian (Bookworm) with Phosh (wayland), [1].

Boot options, I think it is like this;

```
power+volume up -> bootloader
power+volume down -> SD card
power -> eMMC (sometimes SD card?)
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

#### Keyboard

By using the attachable keyboard you will get another 200% more battery capacity.

[1] - https://pine64.com/product/pinephone-pinephone-pro-keyboard-case/

#### Battery life

I have not looked into custom powersaving hacks. Out of the box it does have long battery life as long as it is in suspend (=no internet but will wake on sms and phone calls), otherwise during heavy use maybe 1-3h.

**Battery example** (lasted 5h)

5h battery life from 100% to 0; \
i) Used it 2h on 4G (wifi hw switch off). Normal light use and a few reboots (which could draw more?), with at most 30min screen time but not much time in suspend -> 39% lost (100->61 %). \
ii) Used it 1h on wifi (and 4G). Normal use, browsing/chatting, 30min playing music, 30min screen on. 30% battery lost (~51->21 %). But I'm not sure since it has been stuck on 21% for the last 30 minutes. :P \
iii) 2h with wifi and 4G, screen off. Half the time listening to music. Suspend disabled! \
The phone hung at 3% battery.

#### Powersave

To decrease power use;

- Turn off wifi (if possible also turn off kill switch)
- Minimize screen brightness

- Disable man-db and apt timers (see: sudo systemctl list-timers)

- Enabling suspend makes huge difference (but while in suspend it works like a dump phone)
- It is possible to turn on powersave in power options (I guess it just lowers CPU clock?)

CPU frequency is automatically governed by schedutil in the kernel.

Can try custom kernel [2].

[1] - https://xnux.eu/howtos/pine64-pinephone-getting-started.html \
[2] - https://forum.pine64.org/showthread.php?tid=17339 


## Camera

#### Photos
Taking photos works OK. Front camera is OK, but has a _very_ yellow hue. Back camera is decent if you compare to a phone from like 2012 (I would say it's maybe on par with Samsung S3 for instance. It is better in bad lighting, _but_ sometimes it cannot focus at all also no zoom. And the resulting photo is slightly bigger than the default camera app lets on), so it is almost acceptable perhaps (and the flash works). For back camera, the image in camera app looks very unsaturated (weak colors) but the resulting photo is OK.

For discussion on the camera, and for examples, see for instance this blog series [1].

[1] - https://blog.brixit.nl/pinephone-camera-pt5/

#### Videos
Taking videos is basically _not_ supported in software.
What I've heard is that you can record videos in **ffmpeg** (terminal program).

They ([1]) mention streaming video in firefox is possible (although it is very slow with no hw video acc.).

Signal-desktop did not find the camera at all (but it seems like camera starts focus considering the sound? idk).

[1] - https://communityblog.fedoraproject.org/matrix-video-calls-pinephone/, 2021-03-11 

## Flashlight
Part of system UI, works.

## Wifi
Wifi was moderately fast at 4-5 MB/s (quick test), it works. However, wifi does not seem to be automatically discovered after toggling wifi HW switch while phone is running.

## Internet, texts, calls, mms

Sms, calls, and 2G 3G 4G worked out of the box for me. I (quickly) tried with Tre and Telia carrier networks in Sweden (2022-10). MMS needed manual configuration and data/MMS APN needs to be the same (so e.g. Telia doesn't work), MMS worked on Tre carrier.
Calls over 4G (i.e. VoLTE) did not work out of the box for me, but worked instantly after installing Pinephone Modem SDK (see later section Firmware).

### General 

Telia and Tre carriers (although Tre has no 2G support on their side ofc);
Everything seems to work with the carriers tested, with three exceptions. 1. To get VoLTE (calls on 4G) working (on Fello and Hallon carriers), it was needed to get (ADSP) firmware update and install modem sdk. 2. 2G coverage is much worse than other phones it looks like (but 2G calls and data is supported). 3. MMS works, except for Telia which had two APNs (i.e. no support!).

Compared to other phones, 4G 3G coverage is similar it looks like, however 2G coverage is quite a bit worse.

(4G only), I tried to do speed test in browser but it is too inconsitent, Firefox is too slow? And/or 4G is very variable. Should get around 10mbps up and down (~35ms) regardless. Specifically, down has registered between 4-31 mbps and up ~11 mbps.
To compare, Iphone SE got 58/15 mbit/s (44ms).

(TODO fill in carrier we tested on wiki pages below)

https://wiki.pine64.org/wiki/PinePhone_Carrier_Support

https://wiki.pine64.org/wiki/PinePhone_APN_Settings


### Calls and SMS

tl;dr When tested, calls works on 2G and 3G. Texing works on all networks. 4G calls should work (otherwise try newer firmware). Call quality is OK with headset on 3G and 4G, otherwise risk of echo.


Applies to carriers Tre (altough they have no 2G ofc) and Telia:
Calls work very well on 3G. ~~But it seems like it doesn't want to connect calls on 4G (VoLTE should, in general, be possible but might need to explore.. see [1] [2]), either a text is received for the missed call (Telia) or it says for caller that you're "busy" (!) (Tre).~~ 2G works, although there is some (not too loud) constant interference on the speakers, also in case of bad coverage (or maybe it was a coincidence?) there might be bad interference getting transmitted to the other party.

EDIT: VoLTE (4G calls) worked with Fello, Hallon and Comviq carriers afted installing modem SDK (see later section Firmware).
EDIT: I use VoLTE but echo seems reoccuring when using regular phone call (also calling with speaker phone is terrible), I use headset which works.

Comment: When I tested 2G, hot swapping headset, I managed to crash mic? Regardless it did not work in calls, with headset or not before restart. This happened only once.

Received and transmitted sound is generally good (3G).
It is possible to plug-in and remove headset during conversation, etc. All working well, (except for that one time during call that all mics stopped working... remained even after connecting a headset and also after redialing).

~~[1] - https://wiki.postmarketos.org/wiki/PINE64_PinePhone_(pine64-pinephone)#VoLTE \
[2] - https://wiki.mobian-project.org/doku.php?id=pinephone~~

The modem settings (call volume, date and time, and more) is configurable if using modem sdk, see [1].

[1] - https://github.com/the-modem-distro/pinephone_modem_sdk/blob/kirkstone/docs/SETTINGS.md

#### Firmware

I think it is recommended to install custom firmware SDK for the modem, see [2].

In my case, by updating firmware and installing SDK I instantly got 4G calls (VoLTE) working for Fello and Hallon (carriers) which was broken before. It is also more open source and has more features.

Basic steps as reference (what I did when testing):

1. Update firmware to [ADSP Version 01.002.01.002](https://github.com/Biktorgj/quectel_eg25_recovery/raw/EG25GGBR07A08M2G_01.002.01.002/update/NON-HLOS.ubi) or newer. I used the latest however ([Version 01.003.01.003.](https://github.com/Biktorgj/quectel_eg25_recovery/raw/EG25GGBR07A08M2G_01.003.01.003/update/NON-HLOS.ubi)) and it worked [1]. (Updating firmware is only really necessary if the installed firmware if very old, but I just installed it regardless)

2. Flash modem sdk

```
sudo apt install adb fastboot

## 1 ##
# Get latest firmware (01.003.01.003)
sudo su
echo -ne "AT+QFASTBOOT\r" > /dev/ttyUSB2
exit
wget https://github.com/Biktorgj/quectel_eg25_recovery/raw/EG25GGBR07A08M2G_01.003.01.003/update/NON-HLOS.ubi
sudo fastboot flash modem NON-HLOS.ubi && fastboot reboot

## 2 ##
wget https://github.com/the-modem-distro/pinephone_modem_sdk/releases/download/0.7.0/package.tar.gz #NOTE! Check if new versions available
mkdir package; tar -xvzf package.tar.gz -C package
cd package
./flashall

# Enter pin when prompted.
# Give it a few minutes before giving up and reflashing or trying other firmware. (yes it is possible to restore old firmware if necessary [2])
```

[1] - Comment:  There are different versions of firmware available but this one (01.002.01.002) should be the most likely to work (see [3]). Still, it is possible to use the latest image ([ADSP Version 01.003.01.003.](https://github.com/Biktorgj/quectel_eg25_recovery/raw/EG25GGBR07A08M2G_01.003.01.003/update/NON-HLOS.ubi)) to get some improvements (although in rare cases it introduces new issues [3]). I used the latest firmware version on Swedish carriers with no issues. \
[2] - https://github.com/the-modem-distro/pinephone_modem_sdk \
[3] - https://github.com/the-modem-distro/pinephone_modem_sdk/blob/kirkstone/docs/ADSP-CARRIERS.md

#### Audio

#### Increase call volume
**NOTE:** This was a bad idea for me since VoLTE calls already can echo a lot for the person on the other side! Also headphones are already quite loud.

To increase call receive volume, change in /usr/share/alsa/ucm2/PinePhone/VoiceCall.conf

```
# change in the file to (e.g.) the below (https://github.com/the-modem-distro/pinephone_modem_sdk/issues/7#issuecomment-888979024)
cset "name='DAC Playback Volume' 100%"
cset "name='AIF1 DA0 Playback Volume' 90%"
```

See also [1].

[1] - https://github.com/the-modem-distro/pinephone_modem_sdk/blob/kirkstone/docs/SETTINGS.md

### MMS

tl;dr In general is supported, go to chatty (default sms app phosh) and set settings manually. If it doesn't work make sure Mobile Data APN is same as MMS APN (and supported by carrier).

Works on most carriers, but need to set mms settings manually. For instance, works in Sweden with Tre, does not work with Telia. Also double check data APN so it is what it should.

Telia does not work, why? Well because -> _"Currently there is no easy solution for sending or receiving MMS if carrier has two APNs (one for data and for MMS). There are some workarounds but most likely best one is to not use MMS at all before things get"_
        The issue is here https://gitlab.com/kop316/mmsd/-/issues/5


Swedish Tre-based carriers does work, data APN (add in settings->Mobile Network->Access Point Names) is same as mms. In case of Tre, APN in both cases is data.tre.se.
Tre MMS settings -> APN: data.tre.se, MMSC: http://mms.tre.se, MMS proxy: mmsproxy.tre.se:8799


MMS general links: https://wiki.mobian-project.org/doku.php?id=mms https://forum.pine64.org/showthread.php?tid=16754

```
### Debug
systemctl --user status mmsd-tng
#(make sure data apn is correctly set as well)

### Modify settings (or do it directly in Chatty)
systemctl --user stop mmsd-tng
vim $HOME/.mms/modemmanager/mms
systemctl --user start mmsd-tng
```

## GPS

~~It started of being 50m wrong then it was 200m, 10km and now it's in dead.~~ It does give location in apps and firefox.
After updated firmware and modem-sdk, GPS works. ~~Note that it could take a while until GPS is moderately accurate.~~
When I used GPS, accuracy was very bad around 100m (50-150m according to OpenStreetMaps), it did not improve if I had it on for longer. But needs to be investigated more.


## Alarm app (that works in suspend)

Default alarm app does not work in suspend (lol!), so use this one instead. It did work when I tried it, but setting alarm takes a few seconds (have patience).
Remember to increase the volume so you can actually hear it and wake up!
Has worked well for me.

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

Signal-desktop does not have official arm64 support, but you can build it from source (see below).
Signal-desktop works ok, but not made for small screen: so zoom out via the menu and it _should_ be possible to contract the contact menu by dragging (maybe if you plug in mouse, it's literally 1 px). 
Voice calls on signal work, but it did not detect the camera. Remember that if the phone is in suspend, Signal messages or calls will not be received (of course) until phone wakes up again. 

Signal alerts are shown on lock screen, blue led flashing correctly. It is possible to send attachments such as images, usually it even works to modify (such as to crop or draw on) the picture before sending.

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
git checkout v5.63.0-beta.3 #I just took the latest beta 2022-10-11
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

### Music / Sound

Playing music works OK, can pause etc. on lock screen. In flight mode with music, it seemed to use approximately 15% battery an hour (with no other services running), so it is not terrible. It does not seem to enter suspend while music is playing, at least the music doesn't stop.

**VLC** works but UI not made for phone.

The distro comes with **Lollypop** which at first didn't find any music, but seems to work now. No major complaints. It is a bit odd, and not sure how different music categories work. What I do; Set active folder in settings and go to "Recently added albums", sometimes it doesn't refresh and you need to go in and out of this "Recently added albums".

#### PipeWire

If sound latency problems or issues, maybe try PipeWire. It did NOT help my sound issues, at least not a lot.
(that is, firstly, that sometimes no sound, especially when just rebooted, and need to restart PulseAudio/reboot. Secondly, music often stops when headphone cable is pulled a little .. ~~bad connector?~~ one boot it worked 100% for hours so unsure the reason, must be software or issue with hw on boot?)... regardless annoying issues.

It is easy to do; simply enable PipeWire, disable PulseAudio, and reboot the system (below).

Install and use PipeWire, Maybe this should be configured further (but then again PipeWire was installed by default, and used in some placed due to Wayland? It only needed to be enabled instead of PulseAudio globally as well.)


```
sudo apt install pipewire -y

## Then follow instructions from https://wiki.debian.org/PipeWire#For_PulseAudio (included below) ##

# Check for new service files with:
systemctl --user daemon-reload
# Disable and stop the PulseAudio service with:
systemctl --user --now disable pulseaudio.service pulseaudio.socket
# Enable and start the new pipewire-pulse service with:
systemctl --user --now enable pipewire pipewire-pulse

# You can check which server is in use by, as your regular user, running:
LANG=C pactl info | grep '^Server Name'
# Either it says e.g. "PulseAudio", and when enabled something like "PulseAudio (On PipeWire 0.3.59)"

# Please reboot the system
```


### Video

Hardware video acceleration not available in _any_ browser afaik. In general video decoding is done in SW [1], the issue (I think) is that Mali400 MP2 only supports GLES 2.0. More on HW acceleration see [2] [3].

I think that for video watching in browser, best I have seen is (almost smooth) 30fps@480p, which was on **Angelbrowser** (firefox might be on par if you have enough ram). Outside the browser, mpv was able basically handle 30fps@1080p.
 
For **Firefox** it will probably work better with 3GB ram instead of 2GB, as Firefox watching Youtube takes almost all 2GB I had on system. Moreover, someone said that maybe video works better after you've installed mpv (more likely from some gstreamer dependency), I'm not sure.

However! **mpv** has hardware acceleration [2], so you can e.g. stream/download Youtube videos with yt-dlp with good performance. Specifically, with mpv, 30fps@1080p (VP9 encoding) video is OK and I got only ~8% dropped frames! \
mpv: _"libva driver for the Allwinner system-on-a-chip, called “libva-request”. [..] MPV is already using this lib to decode FullHD MP4 movies on my PinePhone with next to zero frame drops. The amount of power consumption has dropped significantly, too. It’s possible to watch a full-length movie without running out of juice. The PinePhone is using 3.6 Watts when all CPU cores are running, which results in approximately two hours until the battery is drained. With the help of the GPU this is no longer the case, as the power consumption got down to around 2 Watts, doubling the life time of a full battery charge."_ - [2] 


[1] - Lecture: Is there hope for Linux on smartphones?, 2020-08-20, https://youtu.be/jdl1x3DkMEg?t=1256 \
[2] - https://communityblog.fedoraproject.org/matrix-video-calls-pinephone/, 2021-03-11 \
[3] - https://xnux.eu/devices/feature/cedrus-pp.html, (older blog post)

### Browsing

tl;dr video is laggy, stream and play locally if possible. Desktop browsers such as firefox are of course clunky to use.

There is no hw video acceleration in browsers, someone on IRC said this (I'm paraphrasing);
> Firefox will never have hardware acceleration unless we can convince the developers to (1.) lower webrender requirements and (2.) support the legacy openGL renderer. And libva-v4l2-request remains dead.

**Firefox** is "OK", but takes like 5 seconds to start. Video performance is bad, but you can run 360-480p moderately smoothly (maybe? check again with 3GB ram system). Some menus don't show and only flicker, but generally it is usable and e.g. possible to go into settings, clear cookies, install addons etc. Firefox is too hungry to run videos effectively on 2GB system, might be better on 3GB version.
How to handle bookmarks though, hard to understand/navigate?

**Angelbrowser** (`sudo apt install angelbrowser`), worked better in videos and works well on small screen. Video almost worked OK on 480p. Seems good, but I'm not too familiar with this browser.

# Security

Pinephone has hw switches to disable mic / headphones / camera 1 / camera 2 / wifi / cellular modem.

Note that, in general, stock Linux does not have the same application sandboxing as e.g. Android. 

If you have the pin code, you're root... ops!

"_USB-OTG is very permissive . The MTP service currently has no security whatsoever; if the phone is plugged into a computer, even with disk encryption, the computer will have full R/W access to your /home dir, and full read access to /. See Services on how to disable and enable only when needed._" [2].

Offtopic: it is possible to change phone IMEI apparently [3].

## Components and Firmware

Firmware and required apps are, mostly, but not 100% open-source. And for instance, regarding Pinephone Pro, "_ppp-cam app itself will stay closed source_" (?) [1].

To quote Wikipedia, "_The PinePhone aims to be fully open source in its drivers and bootloader. Despite this, due to the scarcity of open source components for cellular and wireless connectivity, the firmware for the Realtek RTL8723CS WiFi/Bluetooth, as well as the optional auto-focus firmware for the OmniVision OV5640 back camera, remain proprietary software. In order to mitigate potential threats to privacy, these components communicate with the rest of the system only over serial protocols, such as USB 2.0, I2S and SDIO, which do not allow direct memory access (DMA). Use of these protocols also permits them to be physically disconnected via kill switches_ [4]"

It is possible (and probably recommended) to flash FOSS modem SDK, in which case _"0 binary blobs in the userspace. Only closed source running on the modem are TZ Kernel and ADSP firmware"_ [5]

[1] - https://xnux.eu/log/#toc-2022-06-23-further-pinephone-pro-camera-development \
[2] - https://wiki.mobian-project.org/doku.php?id=pinephone \
[3] - https://forum.pine64.org/showthread.php?tid=14743 \
[4] - original source: https://www.pine64.org/2020/01/24/setting-the-record-straight-pinephone-misconceptions \
[5] - https://github.com/the-modem-distro/pinephone_modem_sdk

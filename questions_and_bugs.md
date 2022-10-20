# Bugs:

Note this is for my current test phone which is old revision 1.1 Braveheart!

1. Sometimes sound stops working (both speakers and headphones). Why? _Sometimes_ it is fixed by rebooting, _sometimes_ it is fixed by issuing `systemctl --user restart pulseaudio`. Hardware issue, bad connection? Also sometimes playing music is buggy, and pauses etc. related?

2. Sometimes when wake from suspend? And if you manually turn off mobile data or mobile network, often when turned on internet does not work. Sometimes no data, sometimes neither data or cellular. Setting Network gives error, "Cannot register modem: modem is c..." and changing Network Mode says "Transaction timed out".
At least once, the Mobile Network menu disappeared, and reappeared, in settings probably restarting service, but still no internet.
2b. (related or not?) Usually you can force 2G/3G/4G instead of using default, but sometimes not with error: "Setting allowed modes not supported.".

## Rare bugs

1. Very rarely on new boot the top drag-down menu doesn't work. Fixed by locking phone.


# Feature Questions/Requests

0. Sometimes blue light is blinking but I'm not sure why (I just think a lot of apps gives notifications that are not interesting)? And how can I configure it or remove notifications? Still it works, including for Signal-desktop notifications.
1. Import/export contacts from SIM card? (contacts can be handled via .vcf file instead)
2. How to get VoLTE to work, if it doesn't out of the box?
3. Add shortcut / key combination for screenshot?

4. Watch videos in browser without lag, HW acceleration? \
**Partial answer:** I think no hw acceleration in browsers since Mali400 MP2, it only supports GLES 2.0. Use local players if possible, like MPV. Edit, I think it's not as bad as I first thought, as long as there is enough ram, 360p video (incl. livestreams) should at least kind of work (with the downside that it uses a lot of battery due to no hw acc).

6. Record videos at all (ffmpeg should work at least)? Have video calls (probably no)?
7. How to improve GPS? (it almost worked but most often it seems completely off)
8. Is it possible to turn on wifi HW switch during runtime and connect to wifi without reboot? Restart network service or something maybe? Can it be made convenient?
9. How to reliably listen to music, especially while not draining battery too much? Playing music and pausing via lockscreen almost works (not too reliable? It pauses sometimes? See sound issue, maybe just hw issue).
10. How does suspend work?

## Apps (that have issues)

#### VLC
Works to play music, even while phone is locked/screen off, and controlling music via lockscreen works as well (but other apps also work). 
Probably give up and try to use "native" apps like Lollypop, vlc is made for desktop.

1. Bad UI, the media keys like play button doesn't work!

#### Signal-desktop

0. Very often (when adding attachments and other pop ups!) the keyboard no long writes into signal text box, restart the app fixes it. **Very annoying!**
1. Fix build so it is packaged as .deb instead of binary. Or at least add a shortcut so it is easy to launch said binary (how?). (See seprate file on building signal)
2. TODO try to collapse side menu with mouse?
3. It is possible to send and receive pictures, but they look very small and blurry. Need to save them on disk to see clearly.

#### Firefox

Bad UI for small screens, might reconsider.

1. How to change or list bookmarks? Such things are very unclear.

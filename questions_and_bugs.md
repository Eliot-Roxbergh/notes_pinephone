# Bugs:

Note this is for my current test phone which is old revision 1.1 Braveheart!

0. Sometimes sound stops working (both speakers and headphones). Why? I just rebooted and the issue remained, but fixed by issuing `systemctl --user restart pulseaudio`.

1. Very rarely on new boot the top drag-down menu doesn't work. Fixed by locking phone.

2. Sometimes when wake from suspend? And if you manually turn off mobile data or mobile network, when turned on internet does not work (only cellular no data)?
After a while the setting option disappears, and reappear, probably restarting service, but still no internet.
2b. (related or not?) Usually you can force 2G/3G/4G instead of using default, but sometimes not with error: "Setting allowed modes not supported."

# Feature Questions/Requests

0. Sometimes blue light is blinking but I'm not sure why (could be some random system notification or whatever)? And how can I configure it? Still it works, including for Signal-desktop notifications.
1. Import/export contacts from SIM card? (contacts can be handled via .vcf file instead)
2. How to get VoLTE to work, if it doesn't out of the box?
3. Add shortcut / key combination for screenshot?

4. Watch videos in browser without lag, HW acceleration? \
**Partial answer:** I think no hw acceleration in browsers since Mali400 MP2, it only supports GLES 2.0. Use local players if possible, like MPV. Edit, I think it's not as bad as I first thought, as long as there is enough ram, 360p video (incl. livestreams) should be no problem.


6. Record videos at all? Have video calls?
7. Does GPS work? (TODO test me)
8. Is it possible to turn on wifi HW switch during runtime and connect to wifi without reboot? Restart network service or something maybe? Can it be made convenient?
9. How to reliably listen to music, especially while not draining battery too much? Playing music and pausing via lockscreen almost works (not too reliable? It pauses sometimes?).


## Apps (that have issues)

#### VLC
Works to play music, even while phone is locked/screen off, and controlling music via lockscreen works as well. 
Probably give up and try to use "native" apps like Lollypop.

1. Bad UI, the media keys like play button doesn't work?

#### Signal-desktop

0. Very often (when adding attachments?) the keyboard no long writes into signal text box, restart the app fixes it.
1. Fix build so it is packaged as .deb instead of binary. Or at least add a shortcut so it is easy to launch said binary (how?). (See seprate file on building signal)
2. TODO try to collapse side menu with mouse?
3. Sometimes keyboard stops working in app, and need to restart it.
4. It is possible to send pictures etc, but they are very small and blurry. Need to save them on disk to see clearly.

#### Firefox

Bad UI for small screens, might reconsider.

1. How to change or list bookmarks? Such things are very unclear.

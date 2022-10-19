# Bugs:

Note this is for my current test phone which is old revision 1.1 Braveheart!

0. Sometimes sound stops working (both speakers and headphones). Why? I just rebooted and the issue remained, but fixed by issuing `systemctl --user restart pulseaudio`.

1. Very rarely on new boot the top drag-down menu doesn't work. Fixed by locking phone.

2. Sometimes when wake from suspend? And if you manually turn off mobile data or mobile network, when turned on internet does not work (only cellular no data)?
After a while the setting option disappears, and reappear, probably restarting service, but still no internet.
2b. (related or not?) Usually you can force 2G/3G/4G instead of using default, but sometimes not with error: "Setting allowed modes not supported."

# Feature Questions/Requests

0. WHY IS THE BLUE LED BLINKING? And how can I configure it?

1. Import/export contacts from SIM card? (contacts can be handled via .vcf file instead)
2. How to get VoLTE to work, if it doesn't out of the box?
3. Add shortcut / key combination for screenshot?
4. Watch videos in browser without lag, HW acceleration?
5. Record videos at all? Have video calls?
6. Does GPS work? (TODO test me)
7. Is it possible to turn on wifi HW switch during runtime and connect to wifi without reboot? Restart network service or something maybe? Can it be made convenient?

Apps:

1. Fix Signal-Desktop build so it is packaged as .deb instead of binary. Or at least add a shortcut so it is easy to launch said binary (how?). (See seprate file on building signal)

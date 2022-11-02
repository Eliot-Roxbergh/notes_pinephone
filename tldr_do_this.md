
I recommend to do this (**steps are usually explained in README.md**)


# What do I need to do to use PinePhone?

1. Modem: update firmware and get modem sdk. Without this VoLTE did not work for me. (Also more features, probably less energy used etc.)
2. Sound: I still have sound issues, but pipewire _may_ help.
3. MMS: Get a carrier that has data APN same as mms APN. Then set mms APN in the sms app's settings (and check data APN in mobile data settings).
4. Alarm: Get Birdie so alarms work in suspend (as the default app does not!).

## Extras

0. Other possible recommendations: https://github.com/the-modem-distro/pinephone_modem_sdk/blob/kirkstone/docs/SETTINGS.md
1. Signal-desktop is not officially available but can e.g. be built manually. It is not optimized for phone though. TODO improve?
2. ~~Increase call volume from speakers, it is moderately low.~~ Bad idea, the speakers already cause echo!!
3. Video decoding is kind of slow, install mpv and watch/stream locally. `sudo apt install -y mpv yt-dlp`
4. Enable suspend while charging to charge much faster, and/or get a PD 15W+ charger

## Bugs

For bugs and fixes see questions_and_bugs.md

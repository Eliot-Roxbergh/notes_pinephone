# Asorted on Modem

## Fello (Telia-based)

MMS no work since different APNs, prob. will not be supported. Maybe ugly hack possible.

VoLTE works with modem firmware, ADSP Version 01.002.01.002, and modem sdk.

### Works but some warnings (Fello, modem sdk, ADSP Version 01.002.01.002)

```sudo journalctl -u ModemManager```

Reports somethings, such as

```
info: Couldn't start network: QMI protocol error (14): 'CallFailed' \
      reason ... pdn-ipv6-call-disallowed

warn: couldn't load network timezone from the current network.
```

### Fail! (Fello, modem sdk, ADSP Version 01.003.01.003)

I think I can just flash this new image and try again right? In that case it does not work.

```sudo journalctl -u ModemManager```

```
warn: couldn't load operator code: ... MCC/MNC is still unknown
warn: couldn't load operator name: ... id is still unknown
```

## Hallon (Tre-based)

MMS works.

VoLTE has not worked so far, neither stock or modem sdk with ADSP Version 01.002.01.002

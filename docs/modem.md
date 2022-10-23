# Asorted on Modem

NOTE! If you change SIM cards and there is no internet, don't restart phone or restart modem. You need to turn off  "Mobile data" in settings, then turn it on again. Well this worked multiple times for me.

| Swedish carrier | Stock (unknown version ADSP and no modem sdk) | ADSP Version 01.002.01.002 | ADSP Version 01.003.01.003 |
| --------------- | --------------- | --------------- | --------------- |
| Fello (/Telia based carriers) | Y, no VoLTE | Y | Y | 
| Hallon (/Tre based carriers) | Y, no VoLTE | Y (VoLTE not tested) | Y |
| Tele2 based carriers| ? | ? | ? |
| Telenor based carriers| ? | ? |  ? |

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

### (Fello, modem sdk, ADSP Version 01.003.01.003)

Works.

Not checked logs for warnings or anything though.



## Hallon (Tre-based)

MMS works.

Both versions tested, and works.

Not checked logs for warnings or anything though.

Tested 01.003.01.003 and it works for VoLTE.

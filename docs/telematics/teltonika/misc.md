# misc

---

## simulate ignition ON/OFF

Code snippet below are sms command to send to the telematics device.

By default, ignition source is set to `DIN 1` (Id: 101 = 1):

```txt
<usr> <pwd> getparam 101
```

Check ignition status by reading corresponding I/O, AVL ID = 239:

```txt
<usr> <pwd> readio 239
```

Set ignition source as `Power Voltage`:

```txt
<usr> <pwd> setparam 101:4
```

[Power Voltage](https://wiki.teltonika-gps.com/view/FMM130_System_settings#Ignition_Source) - if voltage is between High Voltage Lever and Low Voltage Level (below Ignition Settings options) - ignition is ON; if voltage is higher than High Voltage Lever or lower than Low Voltage Level - ignition is OFF

So, by changing `Low Voltage (mV)` (Id: 105) threshold to a lower value than the default one (13200), it may simulate an ignition ON event:

```txt
<usr> <pwd> setparam 105:10000
```

Check it by reading ignition status:

```txt
<usr> <pwd> readio 239
```

Then, ignition OFF:

```txt
<usr> <pwd> setparam 105:13200
```

After tests, reset device configuration to previous parameter values:

```txt
<usr> <pwd> setparam 101:1;104:30000;105:13200
```

Check it by reading parameter values:

```txt
<usr> <pwd> getparam 101;104;105
```

---

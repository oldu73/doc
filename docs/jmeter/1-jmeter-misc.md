# JMeter - 01 - Misc

---

## Constant number of requests by second

[SRC](https://stackoverflow.com/questions/60293132/jmeter-how-to-send-a-constant-number-of-requests-every-second)

When launching a test, in up right corner, check for number of active units.

Limit at 100 messages / second for a total of 900k messages set the "Ramp Up" - time taken to bring up all the threads (Durée de montée en charge (s)" in french) to 90000.

---

## Log

You can view log messages directly in JMeter GUI, to do so:

- use menu Options → Log Viewer, a log console will appear at the bottom of the interface.  
- or click on the Warning icon in the upper right corner of GUI.

---

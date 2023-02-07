# CRC

---

## Dallas

On a dallas iButton there's a uniq ID like:

```txt
7D        14
000002CFCDCA
```

CRC = 7D

To test, browse to online app [crccalc](https://crccalc.com/) , set input to 'HEX', check for CRC-8/MAXIM value,
invert input as follow:

```txt
7D000002CFCDCA14 becomes 14CACDCF020000
```

Put `14CACDCF020000` in entry field then click on 'CRC-8' button and CRC result should be '0x7D'.

---

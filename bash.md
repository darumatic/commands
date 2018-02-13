# Generating a Random string

```bash
tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1

#Example output
$ tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1
DH3Ngkwh6hyigYIIPo2HI5SN0KOX7Z
```

# Generating a Random string

```bash
tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1
```

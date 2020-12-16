# Generating a Random string

```bash
tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1

#Example output
$ tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1
DH3Ngkwh6hyigYIIPo2HI5SN0KOX7Z
```
# Date Arithmetics 

```bash
#Today + 16 weeks
date --date='16 weeks'
#Today + 1 weeks
date --date='1 weeks'
#Today - 1 day
date --date='1 days ago' '+%a'
#One day ago with formatting
date --date='1 days ago' '+%Y%m%d'
#One day ago for an arbitrary day
date -d 'yesterday 2009-03-01'
```

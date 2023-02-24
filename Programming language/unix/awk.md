# awk
## remove duplicate line

```
awk '!seen[$0]++' filename

```
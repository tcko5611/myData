# Sed

## regular expression

-   \+: one or mode
-   \*: 0 or more
-   \(...\): bracket for contain

## change results to what we want

```sed
sed 's/.*Physical: \([0-9]*\) MB.*/\1/g'

```

## remove the first lines for sed

```
sed  '1,3d' < test.log

```
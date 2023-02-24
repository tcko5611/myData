# diff

only show the line that in old but not in new

```bash
diff --new-line-format="" --unchanged-line-format="" file1 file2
diff <(head -n 3 golden.fdb) <(head -n 3 T@000.fault/QA_TEST/fault_universe/out.fdb)
```

# tr

replace tail char '\n' to char ' '

```bash
tr '\\n' ''
tr -s ' '
cut -d ' ' -f 2,4
```

## find

find link files and delete them

```bash
find . -type l | tr '\\n' ' ' | xargs rm -rf
```

## groups

find groups

```bash
groups
```
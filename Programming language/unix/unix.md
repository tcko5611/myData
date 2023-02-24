# Commands

```bash
$> cat psim.runtime | tr -s ' ' | cut -d ' ' -f 3-4
tr -s : --squeeze-repeats
cut -d ' ' -f 3-4: cut field with character ' ' for fields 3 to 4 
```

command for grep sed cut and sort

```bash
bandgap]> ~/tools/data.py T@000.customfault.fdb | grep subckt_open | sed 's/,/ /g' | cut -d " " -f 6-7 | sort -k 2,2 | less
```
![[bash.png]]
```bash
cp -rL # to copy file with dereference
diff -qr dir1 dir2 # compare files in two directories
```
# KDE
* **dolphin**: file browser
# Gnome
* **eog**: image viewer
# Basic features
## time eclpase
```csh
set a=`date -u +%s`
sleep 5
set b=`date -u +%s`
@ c = $b - $a
echo $c
```
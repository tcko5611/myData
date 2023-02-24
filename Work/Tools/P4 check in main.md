---
date : 2022-04-28 09:03
aliases : []
priority : 5
---
# Metadata
Status :: #Status/Done 
Type :: #P4/info
Topics :: # Metadata
# Note
## procedure
* /depot/p4_utilities/viewtool mkview -ref cktsim_main_pci_01
* viewtool edcs
```
define DEVROOTS
customfault/src/cc_object/CcCfStacked.cpp
customfault/src/cc_object/Master.make
endef
```
* checkout shleved files
```
/depot/tools/CMT/p4unshelve -c 7596538
```
* resolve conflict
```
p4 sync //... ( or viewtool sync //...)
/depot/p4_utilities/p4resolve //...
```
* submit fixed
```
/depot/tools/CMT/p4commit -c 7596538
```
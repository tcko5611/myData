---
date : 2023-03-09 10:55
priority : 1
---
# Metadata
Status ::
Type ::
# Question
With subckt faults, CF will fail
# Answer
CSQA don't support subckt defects
# Note
## code change
### cf_sae_link.tcl
```tcl
# add enviroment variable "CF_DEBUG_REPLAY" to control CF replay function can be use or not
   if {[info exists ::env(CF_DEBUG_REPLAY)] && $::env(CF_DEBUG_REPLAY) == 1} {
     set isReplay_ 1
   }
```
### DefectModelEditor.cpp
```cpp
// add suckt defects with enable "subckt name" field
-    if (arg1 == "subckt_short" || arg1 == "subckt_open")
+    if (arg1 == "subckt_short" || arg1 == "subckt_open" ||
+        arg1 == "subckt_pinopen" || arg1 == "subckt_netopen" )
```
### TestGui.cpp
```cpp
// add all basic defect type for GUI to use
+  baseFaultModels_ << "mos_short" << "mos_open" << "mos_pinopen" << "mos_netopen"
```
### QAfData
```cpp
bool hasSubcktOpenOrShort(); // check configDoc_ has subckt defect or not for CSQA don't support subckt defect
void QAfData::buildCsqaOrigFaultListDoc() {
+  if (hasSubcktOpenOrShort()) return; // In CSQA, if has subckt defect, don't create csqa fault list
+ ...
+ }
```
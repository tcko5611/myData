---
date : 2022-06-08 13:10
aliases : []
priority : 1
---
# Metadata
Status :
Type :: #CF/GUI 
Topics :: # Metadata
# Note
## schName to netName
* using function *nl::mapPath* :
	the syntax is like -type \<NET|Instance\>
## Misc
```tcl
db::getPrefValue deTextEditor # for cc text editor
```
## get sa test suite and testbench, create run script
``` tcl
proc getSessionLCV {windID} {
    set session [sa::getSessions -window $windID]
    if {[::sa::_utils::isTestSuite $session]} {
        # MTB session
        set storage [sa::getTestSuiteStorage -storageType session -session $session]
        puts "Lib: [db::getAttr storage.library]"
        puts "Cell: [db::getAttr storage.cell]"
        puts "View: [db::getAttr storage.name]"
    } else {
        # STB session
        set testbench [sa::getTestbenches -of $session]
        puts "Lib: [db::getAttr testbench.openaccessState.libName]"
        puts "Cell: [db::getAttr testbench.openaccessState.cellName]"
        puts "View: [db::getAttr testbench.openaccessState.name]"
    }
}
sa::createRunScript -testbench $tb

```
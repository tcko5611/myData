---
date : 2023-03-29 10:59
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
Can not cross probing for res and cap that not in the de database.
# Answer
# Note
## code change
### QAfData.cpp
```cpp
// for old format to new format in load state, it will need to check the netlist for full path in order to take care of case_dir
void QAfData::oldToNewInConfigDoc()
{
	...
	// QString simCmdLine = getStringField("simulator") + " " + getStringField("sim_cmdline", "prefix") + " " +
    //  getStringField("top_netlist", "schematic") + " " + getStringField("sim_cmdline", "suffix");
    QString net = (getStringField("top_netlist", "schematic").at(0) == '/') ?
		getStringField("top_netlist", "schematic") :
	    getStringField("case_dir") + "/" +
	    getStringField("top_netlist", "schematic");
    QString simCmdLine = getStringField("simulator") + " " +
		getStringField("sim_cmdline", "prefix") + " " + net + " " +
	    getStringField("sim_cmdline", "suffix");
	...
}
```
### cf_sae_link.tcl
```tcl
proc crossProb { winId insts { type "instance" }} {
	...
    # set probe [descendProb $schFullName $type]
    if {[catch {set probe [descendProb $schFullName $type]} errmsg]} {
        removeAllProbe $winId
        set schFullName [string range $schFullName 0 [string last "/" $schFullName]-1]
        set schNameList [split $schFullName "/"]
        set probe [descendProb $schFullName $type]
    }
	...
}
```
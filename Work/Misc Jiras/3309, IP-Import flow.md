---
date : 2022-04-21 21:03
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Progress 
Type :: #CF/Cockpit 
Topics :: # Metadata
# Note
* insert a IP is a netlist that can run and get a fdb file of the ip
* create subckt of the IP and instantiate IP instances in top netlist
*  using the following setting in config.json
```json
   "fault_ip_import": [{
       "instance":["x1", "x2", "x3", "x4"],
       "fault_db_file": "./CF_xa/xa_addr.fdb"
   }]
```
* as usual run the json file for top netlist as usual
* in the fault universe direction, we can find 
	1. gen_fault use except_inst to eliminate fault in IP instance 
	2. some \*ip\*fdb files that comes from ip faults 
	3. CF will combine these file to generate a final fdb for CF to use 
## Need to do
* Code has some problems, the \*ip\*fdb files don't append the hiearchy names in top netlist correctly.
* in \_create\_ip\_reuse\_fdb(af\_import\_ipfdb.cpp), add ip's fdb instance name with top netlist ip instance name, also for ports. 
* The correct is we need a wrapper of ip in /slowfs/hs_scr1/tcko/customfault/jira/3308_IP_Import/addr_x.spi the net and node name must match.
* In \_create\_ip\_reuse\_fdb(af\_import\_ipfdb.cpp), replace the ip top instance name by real instance name in top netlist
* replace the port name if it has ip top instance name by real instance name in top netlist, or directly add instance name in top netlist.
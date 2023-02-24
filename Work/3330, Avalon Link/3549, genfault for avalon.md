---
date : 2022-07-16 13:11
aliases : []
priority : 1
---
# Metadata
Status :: #Status/Done 
Type ::  #CF/Cockpit 
Topics :: 
# Note
## description
Enhance gen_fault to read avalon instances name and match XA instance name
## code change
### XA
* add -avalon_inst (AVALON_INST) to fdsop_l.l
* in parser(fdop_y.y.c)
	* in \_report_faults add *token_list_t \*avalon_inst*
	* Add token *AVALON_INST*
	* in *fault_device_spec* add *AVALON_INST token_list      { \_report\_faults.avalon\_inst = $2; }*
	* in function *\_process\_device\_faults*, add avalon inst names(string) after ",#" (share with subckt m_param)
* in fdi_fault.c
	* create a hash tabe *avalon\_table\_* for store avalon->xa instane id
	* create function *addAvalonInstToTable\_* to chang avalon instances names to xa instnace id and store to *avalon\_table\_*
	* In *fdiProcessFault*
		* if avalon_inst given, then init *avalon\-table\_* and call *addAvalonInstToTable\_* to store xa instance id's
		* in *\_save\_device\_faults\_recurse*, if instance id in *avalon\_table\_*, mark it in scope and gen fault on it.
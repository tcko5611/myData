---
date : 2022-08-04 09:36
aliases : []
priority : 4
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
Topics :: 
# Note
## test case location
* /slowfs/hs_scr1/tcko/customfault/sae/jira/3615_msv/3616_testcase
## start procedure
* run.sh
* select L/C/V DesignLibrary/vco/VCS_AMS
* PWDE need to run simulation or cteate netlist in order not to get an empty *runSimulation* script in netlist directory. **maybe need to enhance**
* invoke CF/MFG, add my_mos_short , gen fault, run simulation
* save/load state
## library control
* change DesignLibrary's attribute "Technology Library" to "Technology Library"
* library's attribute "Technology Library" must same as "library name" or it will fail in \[oa::getAccess $lib \[oa::LibAccess *\$oa::oacWriteLibAccess*\]\]
## code change
* cf_sae_link.tcl:
	* generate correct PWDE L/C/V for test suite and testbench(saeState) [[CF App Tcl#get sa test suite and testbench create run script]]
	* remove block cosim flow
	* if PWDE just create netlist (will not generate runScript), use *sa::createRunScript -testbench $tb* to create runScript
	* if cosim, change netName by using *regsub* from "<" to "\[" in createWVExprMapFile
	* add *PrimeSimXAVCS* simulator
* MfgWidget.cpp:
	* in *updateGuiAfterLoadState*, check simv (extract from simv commnd and *case_dir*) file exist or not
		* if file exist, *model_->setCosimFields()*, not regenerate simv
		* if file not exist, on_actionCreateNetlist_triggered, *model_->parseCosimFields()*, generate simv
	* in *parseCosimScript* remove check expression exist or not.
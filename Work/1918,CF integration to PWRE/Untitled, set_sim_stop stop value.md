---
date : 2022-07-26 16:08
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type ::  #CF/GUI 
Topics :: 
# Note
## description
* for Mfg, set_stop stope value should be 1
* for fusa, set_stop stope value should be 0
## code change
* check entry point from PWDE, bind PW, update PW and update tb
	* in cf_sae_link.tcl
		* add cf::analysisType_, cf::setAnalysisType to set analysisTYpe
		* in cf::createWVExprMapFile (generate set_sim_stop commands), check analysis type and add stop=0(mfg) and stop=1(fusa) to set_sim_stop command.
	* PWDE entry point
		* invokeMfg : add cf::setAnalysisType mfg before cf::createFaultSimJson
		* invokeFuSa: add cf::setAnalysisType fusa before cf::createFaultSimJson
	* In StackedCcWidget.cpp
		* bindPw, check analysis type and execute TCL cf::setAnalysisType (for there may multiple CF Apps there)
		* updatePwTestSuite, check analysis type and execute TCL cf::setAnalysisType (for there may multiple CF Apps there)
		* updateTb, check analysis type and execute TCL cf::setAnalysisType (for there may multiple CF Apps there)
* set_sim_stop file create by CF App
	* for xa, will create with set_sim_stop tcl commands for xa  and use -c $measFile
	* for finesim/primesim, create netlist set_sim_stop commands and set af.setMeasFile($measFile) , and in CF we will use *\_write\_fault\_option* to write out set_sim_stop command and -fault_include to include the set_sim_stop commands.

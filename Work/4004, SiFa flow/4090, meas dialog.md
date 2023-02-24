---
date : 2023-02-14 08:07
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
How to let user input the meas(extract) - simStop statements into CF App SiFa flow and run fault campaign?
# Answer
* Need to add *tableWidgetUserDefineMeas* in widgetGlobalFaultUniverse for user to inpout/load their set_sim_stop statements.
* When run simulation, testbench should be able to invoke with user define set_sim_stop statements.
# Note
## code
* *SiFaWidget.ui*
	* new *tableWiddgetUderDefineMeas* in *widgetGlobalFaultUniverse*
		* fields : Name, Type (meas, extract, cck_signal, cck_soa), Expression, Min, Max, Logic("", Spec, Defect), Stop("", Yes, No)
* *SiFaWidget.cpp*
	* Delegate settings for *tableWiddgetUderDefineMeas*
		* ClickAddDelegate(7,...) for Name field
		* ComboBoxDelegate(\[\](){ return QStringList{"meas", "extrace", "cck_signal", "cck_soa"}; },...) for Type field
		* ComboBoxDelegate(\[\](){ return QStringList{"", "defect", "logic"}; }, ...) for Logic Field
		* ComboBoxDelegate(\[\](){ return QStringList{"", "Yes", "No"}; } for Stop field
	* model and GUI data exchange
		* in *modelToWidgetGlobalFaultUniverse* add *modelToTableWidgetUserDefineMeas*
		* in *widgetGlobalFaultUniverseToModel* add *tableWidgetUserDefineMeasToModel*
	* fuctions add
		* *on_checkBoxUserDefineMeas_toggled(bool checked)*
			* determine *tableWidgetUserDefineMeas* show/hide
		* *on_pushButtonLoadUserDefineMeas_clicked*
			* get exist meas file name
			* using *model_->loadUserDefineMeasFile(fileName)* to load user define meas into testbench
			* call *modelToTableWidgetUserDefineMeas*
		* *on_pushButtonSaveUserDefineMeas_clicked*
			* call *tableWidgetUserDefineMeasToModel*
			* get save meas file name
			* using *model_->saveUserDefineMeasFile(fileName)* to save user define meas to file
		* *tableWidgetUserDefineMeasToModel()*
			* build a Document from tableWidgetUserDeindMeas contents, an Array : contain row information in table
			* model_->setValueField("userDefineMeas", d);
		* *modelToTableWidgetUserDefineMeas*
			* get Value from model_->getValueField("userDefineMeas")
			* clear tableWidgetUserDefineMeas
			* put v into tableWidgetUserDefineMeas
* Model.cpp *loadUserDefineMeasFile*, *saveUserDefineMeasFile*
	* pass to currSession_
* QAfData.cpp
	* *loadUserDefineMeasFile*
		* using DiagSetting s parse meas file
		* remove "userDefineMeas" from configDoc_
		* prepare an empty Value array 
		* for each simstop in s
			* get fields from simStop and find correspoding expr in cck_\*, meas, extract
			* push fields into Value(array) values
			* push back values into violation\["userDefineMeas"\]
	* *saveUserDefineMeasFile*
		* v = configDoc_\["userDefineMeas"\]
		* open file stream out
		* for each vv in v:
			* grep fields into name, type, expr, min, max, logic, stop
			* if name ="" || type ="" then continue
			* if expr != ""
				* write correspoding "meas", "extract" and "cck" command
			* write set_sim_stop command
* af_defs.cpp
	* add UserDefineMeas in afdata to add user define meas file
* af_print_config.cpp
	* in *_write_faul_option* dump UserDefineMeas contain for "-fault_include" to use.
## test case
* /slowfs/hs_scr1/tcko/customfault/sae/jira/4090_meas
* xa -eldo test.cir
* create all fault
* load meas file
* run
## new problem
* rerun will fail
	* solved by calling *removeSimResultFromFaultList*(QAfData) in run(SiFaWidget)
* K2U problem
	* change QAfData K2U to store in configDoc_["nOfSim"] and inteact with Widgets
	* when checkout license, set the correct number of K2U for af data.
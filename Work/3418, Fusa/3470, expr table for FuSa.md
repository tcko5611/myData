---
date : 2022-06-24 07:59
aliases : []
priority : 1
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI 
Topics :: # Metadata
# Note
* DiagSetting:
	* Add a *buildDiagSettingOnly* function: contruct *DiagSetting* from *Document*
	* change *getValue* function to *const*: change DiagSetting to *configDoc\["diagnostic_coverage"\]*
* Model and FuSaModel:
	* Using *configDoc_* to store *FuSaModel* data
	* move *failureModes* data to *FuSaModel* (in *configDoc_*), and provide *failureModes* and *setFailureModes* set/get functions
	* provide *diagSetting* and *setDiagSetting* get/set functions for testbench *DiagSetting*
* FuSaWidget:
	*  move *ui->tableWidgetFailureMode* contruct from model to function *modelToWidgetGlobal*
	* move *modelToDiag* before *modelToTableWidgetExpr* in *modelToWidgetSetting* for *ui->tableWidgetExpr* need *DiagSetting*
	* In *diagToModel* add *model_->setDiagSetting* to construct testbench's *configDoc\[diagonostic_coverage\]*
	* in *buildTableExpr* use *model_->simStopDoc* to get *sim_stop* statemanets information.
* QAfData
	* add *diagSetting* and *setDiagSetting* get/set functions.
	* in *getFaultGenDir*, check if *af.InternalRunDir* is empty, then return. For it will setFaultUniverDirPath() and FuSa flow will check flist exists in first and check this this function.
	* **we need to check the flow clearly**
		* **should we create_dir for gen_fault and fault campaign?**
		* **for GUI, we can do multiple gen_fault and fault campaign, how could we control the create_dir?** 

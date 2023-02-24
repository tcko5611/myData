---
date : 2022-08-16 15:51
aliases : []
priority : 1
---
# Metadata
Status :: #Status/Done
Type ::  #CF/GUI 
Topics :: 
# Note
## Mfg Flow
1. StackedCcWidget
	1. create new actionOpenFdb disable first
	2. enable avctionOpenFdb_ when flow selected.
	3. openFdb function call StackedWidget->openFdb(fN)
2. StackedWidget : openFdb(fN)
	1. when Mfg call mfgWidget->openFdb(fN)
3. MfgWidget : 
	1. openFdb(fN)
		1. call model->loadFdb(fN)
		2. set stackedWidget to 2
		3. enable stackedWidget
		4. set delegate to result columns of tableViewFlist 
	2. pushButtonFdv_2
		1. check session to set fdb file name
		2. call model to dump fdb file
		3. check case_dir from model (empty when no testbench)
		4. using cbCfOpenFdv(fdb, caseDir, isSqa) <- send to StackedCcWidget to execute correspoding tcl function
4. Model:
	1. hasMember : return false if no testbench
	2. loadFdb:
		1. pasre fN into simulateFlistDoc_
		2. call flistTableModel_::loadFdbData()
5. InternalTableModel:
	1. create loadFdbDate:
		1. generate header from fdb tbs
		2. call updateFlistData()
	2. updateData ->old
		1. generate header from testbenches
		2. call updateFlistData()
6. FlistTableModel::loadFdbData()
	1. call model_->loadFdbData() <- InternalTableModel
	2. proxyModel_->sort(0)
## FuSa and SiFa
* Check in CL@=7764513, git @fe0dfc4
* For Fusa, 
	* we can find failure modes in fdb file -> tablemodelflist
	* load fdb and store fdb name and parse fdb file into flistdDoc_
	* rewrite faultListDocFile for work flow or result view
* For SiFa,
	* almost same with Mfg
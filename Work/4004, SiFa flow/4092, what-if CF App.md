---
date : 2023-02-23 09:15
priority : 1
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI 
# Question
How to integrate What-if flow to CF App
# Answer
# Note
## What-if flow for cockpit (different testbench name)
* in config file, we have a what_if_tag to specify the tb want to replace <br>
```json
"what_if" : { 
    "recalculate_coverage" : true,
    "what_if_tag" : "cfg61"
}
```
* CF has set af.RecalculateCoverge = *true*, af.WhatIfTag = *"xxx"*, af.LastRunDir= *"xxx"* to run new flow
## What-if flow in CF App
* set ui->pushButtonLoadFault->setVisible(false)
* copy from a old testbench
	* remove the fdb results for new testbench
	* set af.LastRunDir from old tb's fdb
	* set af.WhatIfTag from old tb's name
* when run what-if flow
	* set af.RecalculateCoverage = *true*
## code
* SiFaWidget
	* add pushButtonWhatIfRun
	* add function *on_pushButtonRunWhatIf_clicked*
		* model_->setRecalculateCoverage(true)
		* runSimulation()
	* modify function *on_pushButtonRun_clicked*
		* model_->setRecalculateCoverage(false)
		* runSimulation()
	* add new function *runSimulation*
		* remove previous run
		* collect selected fault ids
		* widgetsToModel
		* model_->runSimulation(idsSet)
	* Misc
		* turn *model_->getGlobalTemplateWeightFile* to *model_->getTemplateWeightFile*
		* remove *model_->...numOfSims*, now all use *model_->,,,K2U*
		* enable *pushButtonRunWhatIf* only for *!model_->getLastRunDir().isEmpty()* (is clone tb)
		* use widget to contain sample fields in orger to make visible work on layout
* Model
	* change function *cloneSesion* to virtual (SiFa flow need to do more thing in clone for what if flow)
	* add functions *setRecalculateCoverage*(control flow is what if or not) and *getLastRunDir*(check if the tb is a clone tb or not) to get/set data from QAfData
* SiFaModel
	* add function *cloneSession* override
		* Model::cloneSession()
		* currSession_->setSiFaWhatIfInfoInClone();
		* currSession_->removeSimResultFromFaultList();
* QAfData
	* Modify *setTemplateWeightFile*
		* = af.getTemplateWeightFileName()
	* Add *setSiFaWhatIfInfoInClone*; only use in clone procedure in SiFa flow
		* af.setLastRunDir(af.getInternalRunDir());  // set last run in copied tb
		* af.setWhatIfTag(af.getRunIdTag()); // set what if tag for copied tb
	* Add *getLastRunDir*
		* return af.getLastRunDir()
	* Add *setRecalculateCoverage(b)*
		* af.setRecalculateCoverage(b)

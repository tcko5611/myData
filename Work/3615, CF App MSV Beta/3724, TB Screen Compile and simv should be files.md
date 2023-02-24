---
date : 2022-09-06 10:27
priority : 3
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI 
Summary :: 
Topics :: 
# Note
## Code change
* GUI 
	* hide *lineEditSimvCommand*, *lineEditCosimScript*, *pushButtonCosimScript*
	* add *lineEditSimvFile*, *pushButtonSimv*, 
		* *lineEditSimvFile*: 
			* if file not exist
				* then warning, return
			* read the file content
			* if simv existed
				* then call *setCosimFields()*, simvExisted_= true
				* else simvExisted_= false
			* set content to *lineEditSimvCommand*
		* *pushButtonSimv*: 
			* if file not exist
				* then return
			* open simv file and edit
			* after finished edit/save,
				* set file name to  *lineEditSimvFile*
				* read the file content
				* if simv existed
					* then call *setCosimFields()*, set simvExisted_= true
					* else simvExisted_= false
				* set content to *lineEditSimvCommand*
	* add *pushButtonGenSimv*, 
		* *pushButtonGenSimv*:
			* create simv through function *on_actionCreateNetlist_triggered()*
	* modify *lineEditCompilation*, *pusButtonCompilation* behaviors
		* remove auto generate simv functionarity.
---
date : 2022-08-07 21:24
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
Topics :: 
# Note
## code change
* ItemDEelgates for *FailureModeDelegate*
	* *setEditorData*
		* last row not add row, just *insertState = true*
		* other's parent class *setEditorData* and *insertState = false*
	* *setModelData*
		* check duplicate or not
		* if ((empy && insertState) || duplicate) return // do nothing
		* if (empty) remove row
		* else setModelData
		* if last row != "click to add"
			* add "click to add" to last  row
---
date : 2022-06-27 09:16
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI
Topics :: [P10241015-3460](https://jira.internal.synopsys.com/browse/P10241015-3460), [[QPushButton]]
# Note
* code change: 
	* add a new button *Define From Template*
	* check template file exists or not(global for mfg)
	* copy to a new select file
	* start a process to edit the file(create a new function *editWeightFile* for this to share code)
* 2022/6/29
	* first need to use QFile::remove original file and use QFile::copy to copy weight file from template file
	* in af_selection.cpp, need to replace exit(-1) with throw exception
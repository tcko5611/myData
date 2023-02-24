---
date : 2022-04-21 15:10
aliases : []
---
# Metadata
Status :: #Status/Done
Type :: #CF/Cockpit
Topics :: # Metadata
# Note
* P4 check number: 7554668, 7586539
   don't change gmdMacro function in fmeum\_macro.c
   in \_subckt\_covert\_macron if  defect model contain only one element, before calling gmdMacro, retrun the gmd of that element.
* test case: 3286/test_xa, 
	* normal signal: a3
	* reference signal: delay_open
	* test point : e3
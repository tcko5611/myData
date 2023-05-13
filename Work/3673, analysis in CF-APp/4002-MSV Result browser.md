---
date : 2023-01-12 09:09
priority : 2
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI 
Summary :: 
Topics :: 
# Note
## test case: 
/slowfs/hs_scr1/tcko/customfault/sae/jira/3673_results/4002_mtb
## code modify
* af_main.cpp: write *modifyFilesInConfigToAbsolutePath_(Document &doc)* to change original config to absolue path
	* c++, using *realpath* [[Cpp utilities]] to get realpath of a related path
	* c++, using *ifstream.good()* to test file exist or not.
* MfgWidget.cpp: Need to check *item* exist or not in *QTableWidget*
* QAfData.cpp: insert *transformCosimSignalFile* into *valueToConfigDocAndAf* to save dwc information
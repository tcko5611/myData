---
date : 2023-01-14 10:40
priority : 1
---
# Metadata
Status :: #Status/Done 
Type :: #CF/GUI 
Summary :: 
Topics :: 
# Note
## case
/slowfs/hs_scr1/tcko/customfault/sae/jira/2849_sqa/4086_compile_only
## result not correct
* check results by call function 0 *QAfData::runCSqaRecalculate*
	* *GeneratesqaccfResult* will change the threshold result in flist(correct)
	* add *GenerateCsqaFdbFileFromPreviousResult* to generate final fdb file.
	* the main problem comes from *QFile::reanme* will not work on exist file.
* add *GenerateCsqaFdbFileFromPreviousResult* in *af_concurrent_fault.cpp* to generate final fdb
* in *GeneratesqaccReport* in recalculate also need to get run path *afdata_obj.getFaultUniverseDirPath()* in CF App.
* Change fuction *PrintJsonDocument(std::string filename, Document& d)* to *PrintJsonDocument(const std::string &filename, Document& d)* in *af_local.h*
* Need to check GUI fields for CSQA related operation
	* lineEditCSqaTh
	* spinBoxK2U
* in *QAfData* add CSQA relate operations "run", "compile_only" and "recalculate" according the CQSA flow in af_main.cpp
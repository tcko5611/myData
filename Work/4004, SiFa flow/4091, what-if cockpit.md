---
date : 2023-02-19 12:12
priority : 4
---
# Metadata
Status :: #Status/Validate 
Type ::
# Question
How to invoke what-if flow in different tb names? Use  new tb name and remove old tb resuts.
# Answer
* Use a new tag whatIfTag
``` json
"what_if" : { 
    "recalculate_coverage" : true,
    "whatIfTag" : "cfg61"
},  
```
# Note
## code procedure for RecalculateCoverage = true
* af_parse_config.cpp:3601, \_parse\_combined\_config.json,  br 6
	* set simulator for xa
* af_defs.cpp:207, afdata::getRecalculateCoverage, br 1
	* definition
* af_parse_config.cpp:4872, \_check_config_consistency, br 7
	* check fault list exist or not
* af_parse_faultlist.cpp:151, \_non_parametric_setup, br 8,9
	* if not, runIdTag should not in flistd
	* if yes, get afdata_obj.setLastRunDir(lastRunDir)
* af_utils.cpp:1646, CheckFlistdConsistency, br 20
	* check af.getRunIdTag in flist
* af_print_config.cpp:3299, PrintConfigFiles, br 15
	* check needed nominal run or not base on last result
* af_print_config.cpp:3658, PrintConfigFiles, br 17
	* check last wave files
* af_print_config.cpp:660, \_create_csh_file, br 10
	* seting license type
*  af_print_config.cpp:681, \_create_csh_file, br 11
	* replace "-fault_inject" to "-c" for primesim/finesim
* af_print_config.cpp:2216,  \_add_run_script_to_flistd, br 13, 14
	* add or replace run script in flistd
* af_metadata.cpp:300, UpdateOrCheckMetaData, br 3
	* newtag = false except for RecalculateCoverage
* af_metadata.cpp:309, UpdateOrCheckMetaData, br 4
	* add metadata\["recalculate_coverage"\] flag
* af_main.cpp:646, CleanRecalculateCoverageData, br 2
	* remove tag results
* af_metadata.cpp:438, UpdateOrCheckTagData, br 5
	* change out_prefix, complete
* af_utils.cpp:231, PeriodicFdbReportFlush, br 19
	* period output
* af_read_stderr.cpp:1825, ResultsFromStderr, br 18
	* call \_add_NS_ForSortedSkipSimulateFaults
## plan for code.
* add a string property **WhatIfTag**
* get lastRunDir from **WhatIfTag**
* After parse fault list, remove all **WhatIfTag** information from flistd
* run simulation as usual

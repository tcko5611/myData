---
date : 2022-04-23 07:31
aliases : []
---
# Metadata
Status :: #Status/Validate  
Type :: #CF/QA
Topics :: [[synmake]]
# Note
* info: for  cosim what_if flow, the merge fsda will be like: **mos_1_1_vcs.fsdb**, and we don't want to run cosim in what_if recalculation
* code: change af_print_config.cpp to check **mos_1_1.fsdb** and **mos_1_1_vcs.fsdb**
* case: \,<p4_client\>/unit_cf/msv/snps/what_if/merge_fsdb
   add run_tb1.sh, run_whatif1.sh, what_if1.json to complete the case

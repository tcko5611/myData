---
date : 2022-06-13 10:22
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Progress 
Type :: #CF/QA 
Topics :: # Metadata
# Note
* primewave don't need to set "step" in "tran" staements, finesim need or finesim will fail.
* using *cp -RP ../../../unit_cf/gui .* to create a run directory
* create ~/.tmpdisplay with your $DISPLAY
* using /slowfs/fs_qae13/minfan/P4_CLIENTS/sith_case_t2022.06_dev/unit_common/sith/scripts/sith_launch -bin=/u/cdmgr/image/CUSTOMCOMPILER/T-2022.06/latest_with_pw/Testing/bin/custom_compiler -simulator=/global/apps/primesim_2022.06/bin/primesim -dir=/remote/hsim_eng2/ktc/ktc_dev_customfault/object_root/customfault/linux64/gui -host=/remote/hsim_eng2/ktc/ktc_dev_customfault/csim/src/rules/whitebox_mach.LINUX64 -env=PATH+=/remote/hsim_eng2/ktc/ktc_dev_customfault/customfault/snps/customfault/nightly_opt/bin to run simulation

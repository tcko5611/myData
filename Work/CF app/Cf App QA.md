---
date : 2023-03-08 14:15
priority : 1
---
# Metadata
Status :: #Status/Done 
Type :: #CF/GUI 
# Question
How to run CF APP
# Answer
# Note
## process for run CF APP QA
* cp -rP <p4_client>/unit_cf/gui .
* run command
``` sh
/remote/swefs1/PE/products/cktsim/u2023.03_dev/clientstore/cktsim_u2023.03_dev_testcase/unit_common/sith/scripts/sith_launch -host=/remote/hsim_eng2/ktc/ktc_dev_customfault/csim/src/rules/whitebox_mach.LINUX64 -bin=/remote/swefs1/PE/products/cd/u2023.03_dev/image/nightly/customcompiler_optimize/latest_with_pw/Testing/bin/custom_compiler  -simulator= /remote/swefs1/PE/products/cktsim/u2023.03_dev/image/nightly/csim_optimize/latest/Testing/bin/xa -env=PATH+=/remote/hsim_eng2/ktc/ktc_dev_customfault/customfault/snps/customfault/nightly_opt/bin
```
* gather result include uncompleted result
```bash
/remote/swefs1/PE/products/cktsim/u2023.03_dev/clientstore/cktsim_u2023.03_dev_testcase/unit_common/sith/scripts/sith_gather > tt
```
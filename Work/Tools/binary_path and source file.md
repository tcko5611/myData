# Binary path
## xa, customfault, finesim, primesim
- /remote/swefs1/PE/products/cktsim/main/image/nightly/csim_optimize
- /remote/swefs1/PE/products/cktsim/main/image/nightly/customfault_optimize
- /remote/sweifs1/PE/products/cktsim/s2021.09_dev/image/nightly/finesim_optimize
-  /remote/sweifs1/PE/products/cktsim/s2021.09_dev/image/nightly/primesim_optimize
## PrimeWave, CustomCompiler
- /u/wvmgr/image/PrimeWave/T-2022.06/latest/Testing/bin
- /u/cdmgr/image/CUSTOMCOMPILER/T-2022.06/latest/Testing/bin
# Build binary
## Xa
- symake install, synmake install-degug, synmake install-fulldebug
- cd $\<client\>/csim/src/code/dki_cosim, synmake shlib-1, synmake shlib-debug-1
## cck
- synmake install, synmake install-debug
## customfault
- synmake -k -j XA_RD_ssh TARGET_ARCH=linux64 P4ID=12345 -echo install-debug
# Run Binary
## Xa run verilog env
```bash
export PVA_HDL=1
export SNPS_PLATFORM=amd64
export XA_HOME="/global/apps/xa_2014.09"
export XA_GCC="/depot/qsc/QSCJ/bin/gcc"
```
# CC enviroment
## cc inlcude files:
  /u/cdmgr/clientstore/cd_main_us01/snps/synopsys-infra/include
## cc binary:
  /u/cdmgr/image/CUSTOMCOMPILER/U-2023.03/latest_with_pw/Testing/bin
  /u/cdmgr/image/CUSTOMCOMPILER/T-2023.03/latest_with_pw/Testing/.pw_image/bin
## primewave binary:
  /u/wvmgr/image/PrimeWave/U-2023.03/latest/Testing/bin
  /u/wvmgr/image/PrimeWave/T-2023.03/latest/Testing/.cc_image/bin
# sith
## CF
```bash
/remote/swefs1/PE/products/cktsim/main/clientstore/cktsim_main_testcase/unit_common/sith/scripts/sith_launch -bin = /remote/hsim_eng2/ktc/ktc_dev_customfault/customfault/snps/customfault/nightly_dbg/bin/customfault -simulator = /remote/hsim_eng2/ktc/ktc_dev_customfault/csim/snps/csim/nightly_dbg/bin/xa -dir= /remote/hsim_eng2/ktc/ktc_dev_customfault/object_root/customfault/linux64/whitebox -host = /remote/hsim_eng2/ktc/ktc_dev_customfault/csim/src/rules/whitebox_mach.LINUX64
```
## Xa
```bash
/remote/swefs1/PE/products/cktsim/main/clientstore/cktsim_main_testcase/unit_common/sith/scripts/sith_launch -bin = /remote/hsim_eng2/ktc/ktc_dev_customfault/csim/snps/csim/nightly_dbg/bin/xa -dir= /remote/hsim_eng2/ktc/ktc_dev_customfault/object_root/whitebox/linux64/dbg -host = /remote/hsim_eng2/ktc/ktc_dev_customfault/csim/src/rules/whitebox_mach.LINUX64
```

# Example directory:
```
/remote/tw_rnd1/ktc/prog/qt
```
# LSF
```bash
source /global/lsf/cells/ams_lsf_grid/conf/cshrc.lsf
```
## dpconfig file
```
1| |20|/tmp|CUSTOM|bsub -app batch -R \"select[os=CS7.3]\" -e ./bsub.err -o ./bsub.out
```
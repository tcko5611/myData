---
date : 2022-04-27 10:37
aliases : []
priority : 5
---
# Metadata
Status :: #Status/Done 
Type :: #CF/Cockpit 
Topics :: # Metadata
# Note
## Licese server
### set env for dump license stats
setenv FLEXLM_DIAGNOSTICS 3

### Start license server

You can use this license file so far /slowfs/fs_qae39/wyang/wyang_new_main/unit_common/license_test/xa/lic_2020.12_temp_key_fsei704.txt  ( I will checkin once main branch unlocked)

How to use:

1. Start the license server:

	a. ssh to fsei704, cd /slowfs/hs_scr1/tcko/license_test

	b. /global/apps/scl_2017.12/linux64/bin/ethis_license_file

	c. lmstat -a -c 27020@fsei704

	d. lmdown -c primesim2.txt

2. Run xa/cf/finesim jobs:

	a. rsh to your working machine

	b. lm

	c. run your job.

3. Monitor license server output from 1, and tool output from 2.

### Cut license

1. Keys file: unit_common/license_test/xa/lic_2020.12_temp_key_fsei704.txt

2. Checkout the unit_common/license_test/…  or only this case  unit_common/license_test/xa/customfault/custom_fault_nobeta_release   (upper level sith.env are needed to be checkout as well)

3. use <p4_client>/unit_common/license_test/xa/customfault/custom_fault_nobeta_release/sith.run.example -b =/remote/swefs1/PE/products/cktsim/t2022.06_dev/image/pci/fastspice_optimize/latest/csim/bin/xa to generate keys.

4. Modify sith.run.example on the keys you want to cut to lic_temp.txt . This example cut 2 custom_fault , and 8 custom_fault_sim keys

create_license -in /slowfs/fs_qae39/wyang/wyang_new_main/unit_common/license_test/xa/lic_2020.12_temp_key_fsei704.txt -out $lic_file -token {

    custom_fault  2

    custom_fault_sim 8

}

5. use the temp_lic.txt file to start license server and use for your testing.  Make sure you stop old license server before start new one.

### Simple way to checkout license

$>module load scl

sclsh> co custom_fault_sim 4

## Customfault Code:

In af_license.cpp provide free_slot, and in MasterFlow::execute will use free_slot.

The LicensePackManager::kSlotPerPack=4(default) is each pack can run n slot simulation.
## License problem
* P10241015-2819, about check in kety for queue, some error message will dump to server after 1st one check in.
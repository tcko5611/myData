---
date : 2022-12-04 08:36
priority : 1
---
# Metadata
Status ::
Type :: #CF/Cockpit 
Summary :: 
Topics :: 
# Note
## json file
```json
    "sqa": {
      "type": "object",
      "properties": {
		"csqa": { // ConcurrentFaultFlag
          "type": "object",
          "properties": {
            "set_waveform_format": { // Ccfwaveformat
              "type": "string"
            },
            "nominal_run_fsdb": { // CcfnomfsdbFile
              "type": "string"
            },
            "nominal_run_tag": { //CcfnomTag
              "type": "string"
            },
            "defect_sensitization_threshold": { // Ccfsensitizethresh
              "type": "number"
            },
            "recalculate": { // Ccfpostprocess
              "type": "boolean"
            },
            "compile_only": { // Ccfcompileonly
              "type": "boolean"
            }
          }
        }
	}

```
## af related setting
```cpp
afdata_obj.setSqaFlag(true); 
afdata_obj.setRunNominalFlag(true);
afdata_obj.setRunNominalOnlyFlag(true);
afdata_obj.setConcurrentFaultFlag(true);
afdata_obj.setDevToNet2NetFlag(false);
afdata_obj.setCcfwaveformat(ccf["set_waveform_format"].GetString());
afdata_obj.setCcfnomTag(ccf["nominal_run_tag"].GetString());
afdata_obj.setCcfnomfsdbFile(ccf["nominal_run_fsdb"].GetString());
afdata_obj.setCcfsensitizethresh(ccf["defect_sensitization_threshold"].GetDouble());
afdata_obj.setCcfpostprocess(ccf["recalculate"].GetBool());
afdata_obj.setCcfcompileonly(ccf["compile_only"].GetBool());
```
## run setting
```cpp
afdata_obj.getSqaFlag(); // false
afdata_obj.getRunNominalFlag(); // true, need to compare
afdata_obj.getRunNominalOnlyFlag(); // false
afdata_obj.getConcurrentFaultFlag(); // true
afdata_obj.getDevToNet2NetFlag(); // false
afdata_obj.getCcfwaveformat(); // "", default
afdata_obj.getCcfnomTag(); // "", default
afdata_obj.getCcfnomfsdbFile(); // "", default
afdata_obj.getCcfsensitizethresh(); // 0.1, setting
afdata_obj.getCcfpostprocess(); // false, default
afdata_obj.getCcfcompileonly(); // false, default
```
## run process
```cpp
IdentifyFaults(af) // need to set csqa flag before for generate graph infomation, will generate CCB information
flist = ParseFaultListJSON(...); // same as normal run
AddNominal(flistd);
SetupForResume(afdata_obj, flistd);
CalculateWeights(afdata_obj, flistd);
PrintTotalWeight(...);
CreateCCBOutputMap(afdata_obj); // create sqa.probe, this is always exist
GenerateConcurrentFault(afdata_obj, flistd);
CreateFaultwvSigFile(afdata_obj, flistd); // create signal file for simulation, need after generate concurrent fault
string ccflist_path = afdata_obj.getFaultUniverseDirPath();
string ccflistname = ccflist_path + "/sqa_conc.faultlist"; 
afdata_obj.setFaultListFileName(ccflistname);
lfile = afdata_obj.getFaultListFileName();
const char * const ccFaultListFile = lfile.c_str();
flistd = ParseFaultListJSON(ccFaultListFile, afdata_obj, &is_parametric_fault);
AddNominal(flistd);
PrintConfigFiles(afdata_obj, flistd, false);
UpdateOrCheckMetaData(afdata_obj, flistd);
UpdateOrCheckTagData(afdata_obj, flistd);
InjectFaults(afdata_obj, flistd);
GenerateCsqaFdbFile(afdata_obj, flistd); // add information to original raw fdb
PlotsqaccfSensitization(afdata_obj, flistd);
GeneratesqaccfReport(afdata_obj, flistd, false);
```
## run flow
* Need to setup set up *af csqa* related flag before *IdentifyFaults* in order to generate partition info *.dot* files.
* *CreateCCBOutputMap* can move foreward for "sqa.probe" always there
* other should be in simulate stage.
* CF Mfg App, model should save *FaultUniversePath* property when IdentifyFaults for other also need to use the data in this directory. 
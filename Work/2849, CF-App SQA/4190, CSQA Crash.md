---
date : 2023-02-27 15:49
priority : 1
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI
# Question
Why we save OA and load, then CSQA will crash?
# Answer
# Note
## test casse
* /slowfs/hs_scr1/tcko/customfault/sae/jira/2849_sqa/4190_csqa_crash
* PWDE L/C/V: CFAdderDemo/adder4_tb_pwl/PrimeSim_XA_default
* 
## current code, QAfData
### runCsqa
```cpp
  Document flistd;
  flistd.CopyFrom(genFaultListDoc_, flistd.GetAllocator());
  // add for Csqaftd
  Csqaftd.CopyFrom(flistd, Csqaftd.GetAllocator());
  AddNominal(Csqaftd);
  // end for Csqaftd
  CalculateWeights(af, flistd);
  GenerateConcurrentFault(af, flistd);
  CreateFaultwvSigFile(af, flistd);
  string ccflist_path = af.getFaultUniverseDirPath();
  string ccflistname = ccflist_path + "/sqa_conc.faultlist"; 
  af.setFaultListFileName(ccflistname);
  std::string lfile = af.getFaultListFileName();
  const char * const ccFaultListFile = lfile.c_str();
  bool is_parametric_fault = false;
  csqaFaultListDoc_ = ParseFaultListJSON(ccFaultListFile, af, &is_parametric_fault);
  AddNominal(csqaFaultListDoc_);
```
### runCsqaRecalculate
```cpp
  Document flistd;
  flistd.CopyFrom(csqaFaultListDoc_, flistd.GetAllocator());
  CreateFaultwvSigFile(af, flistd);
  GeneratesqaccfResult(af, flistd);
  Document Ufd;
  Ufd.CopyFrom(genFaultListDoc_, Ufd.GetAllocator());
  AddNominal(Ufd);
  cSqaDbName_ =  getLogPrefixName() + "_csqa.fdb";
  GenerateCsqaFdbFileFromPreviousResult(Ufd, flistd, cSqaDbName_.toStdString());
  PlotsqaccfSensitization(af, flistd);
  GeneratesqaccfReport(af, flistd, false); 
```
### runCsqaCompileOnly
```cpp
  Document flistd;
  flistd.CopyFrom(genFaultListDoc_, flistd.GetAllocator());
  // add for Csqaftd
  Csqaftd.CopyFrom(flistd, Csqaftd.GetAllocator());
  AddNominal(Csqaftd);
  // end for Csqaftd
  CalculateWeights(af, flistd);
  GenerateConcurrentFault(af, flistd);
  CreateFaultwvSigFile(af, flistd);
  string ccflist_path = af.getFaultUniverseDirPath();
  string ccflistname = ccflist_path + "/sqa_conc.faultlist"; 
  af.setFaultListFileName(ccflistname);
  std::string lfile = af.getFaultListFileName();
  const char * const ccFaultListFile = lfile.c_str();
  bool is_parametric_fault = false;
  csqaFaultListDoc_ = ParseFaultListJSON(ccFaultListFile, af, &is_parametric_fault);
  AddNominal(csqaFaultListDoc_); 
```

## modify code
* Add a new member varaible csqaOrigFaultListDoc_
* Add a new function buildCsqaOrigFaultListDoc() after gen_fault
* about af.FaultUniverseDirPath 
	* when genFault store into af.FaultUniverseDirPath into configDoc_\["FaultUniverseDirPath"\]
	* when load OA view, if !genFaultListDoc\_.IsNull() then af.FaultUniverseDirPath = configDoc_\["FaultUniverseDirPath"\] 
### code change gen_fault
* store gen\_fault fdb  into genFaultListDoc\_
* call buildCsqaOrigFaultListDoc() after gen\_fault
```cpp
void QAfData::buildCsqaOrigFaultListDoc() {
  af.setSqaFlag(false);
  af.setRunNominalFlag(true);
  af.setRunNominalOnlyFlag(false);
  af.setConcurrentFaultFlag(true);
  af.setDevToNet2NetFlag(false);
  af.setFlistUserDefinedFlag(false);
  Document flistd;
  flistd.CopyFrom(genFaultListDoc_, flistd.GetAllocator());
  CalculateWeights(af, flistd);
  GenerateConcurrentFault(af, flistd);
  CreateFaultwvSigFile(af, flistd);
  string ccflist_path = af.getFaultUniverseDirPath();
  string ccflistname = ccflist_path + "/sqa_conc.faultlist"; 
  af.setFaultListFileName(ccflistname);
  std::string lfile = af.getFaultListFileName();
  const char * const ccFaultListFile = lfile.c_str();
  bool is_parametric_fault = false;
  csqaOrigFaultListDoc_ = ParseFaultListJSON(ccFaultListFile, af, &is_parametric_fault);
  AddNominal(csqaOrigFaultListDoc_); 
  af.setConcurrentFaultFlag(false);
  af.setDevToNet2NetFlag(true);
}
```
### code change in runCsqa
```cpp
  // add for Csqaftd
  Csqaftd.CopyFrom(genFaultListDoc_, Csqaftd.GetAllocator());
  AddNominal(Csqaftd);
  // end for Csqaftd
  csqaFaultListDoc_.CopyFrom(csqaOrigFaultListDoc_, csqaFaultListDoc_.GetAllocator()); 
```
### code change in runCsqaRecalculate (Don't change)
```cpp
  Document flistd;
  flistd.CopyFrom(csqaFaultListDoc_, flistd.GetAllocator());
  CreateFaultwvSigFile(af, flistd);
  GeneratesqaccfResult(af, flistd);
  Document Ufd;
  Ufd.CopyFrom(genFaultListDoc_, Ufd.GetAllocator());
  AddNominal(Ufd);
  cSqaDbName_ =  getLogPrefixName() + "_csqa.fdb";
  GenerateCsqaFdbFileFromPreviousResult(Ufd, flistd, cSqaDbName_.toStdString());
  PlotsqaccfSensitization(af, flistd);
  GeneratesqaccfReport(af, flistd, false); 
```
### runCsqaCompileOnly
```cpp
  Csqaftd.CopyFrom(genFaultListDoc_, Csqaftd.GetAllocator());
  AddNominal(Csqaftd);
  // end for Csqaftd
  csqaFaultListDoc_.CopyFrom(csqaOrigFaultListDoc_, csqaFaultListDoc_.GetAllocator());
```
## In \_write_wvcmp_cmd in af_print_config.cpp
change "mv " to "mv " for rerun(data are in fault_universe)
## MfgWidgte
* change CSqa related widgets enable behavior
* for csqa result related name change to save in configDoc_
## Model
* add *readyForRunCsqa* and *hasCSqaResult* to control MfgWidget widget behavior
* modify *creatFaultList* will generate data for CSqa to use
## QAfData
* add *readyForRunCsqa* and *hasCSqaResult* base on *csqaOrigFaultListDoc_* and *csqaFaultListDoc_*
* *buildCsqaOrigFaultListDoc* will be called after gen_fault to generate *csqaOrigFaultListDoc_*
* *rebuildCsqaData* will be called after load OA view for construct CSQA global data 🤕
* add set/get function for genFdb, csqaOrigFdb, csqaFdb
* add *af.setCcfwaveformat(getStringField("outputDataFormat").toStdString())* in *configDocToCockpit* to set CSQA waveform format.
## cf_sae_link.tcl
* add *set outputDataFormat \[db::getAttr envOpts.outputDataFormat\]* and 
* dump *puts \$jsonfile "      "outputDataFormat": "\$outputDataFormat,""
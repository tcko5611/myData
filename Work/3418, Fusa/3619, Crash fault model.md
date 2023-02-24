---
date : 2022-08-07 09:30
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
Topics :: 
# Note
## code change
* BaseWidget.h -> add fm_mos_pinopen, fm_mos_netopen for BaseFaultModelIndex use
* DefectModelEditor -> change to use TestGui::baseFaultModels() to get base fault models
* in MfgWidget, FuSaWidget 
	* TestGui::baseFaultModels() to get base fault models
	* add fm_mos_pinopen, fm_mos_netopen to baseNAtable_(field that N/A for specific fault model)
	* in addFaultModel ->add line *if (getFaultModelTableValue(row, col) == "N/A") continue;* to avoid miss use N/A field from config/load json.
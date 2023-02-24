---
date : 2022-09-04 09:49
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
Summary :: 
Topics :: 
# Note
## code change
* afdata : add property *PostlayoutNetlistFlag* to indicate postlayout netlist is specified in CFGUI or not.
* in af_model_selection.cpp, af_parse_config.cpp when net2net fault, change *!afdata_obj.getNetFaultIdCmdline().empty()* to *!afdata_obj.getNetFaultIdCmdline().empty() || afdata_obj.getPostlayoutNetlistFlag()* to add *set_ba_option -ccnet \** 
* in QAfData : add *af.setPostlayoutNetlistFlag(bool)* in *setPostlayout* function
* in MfgWidget, add *PostlNetlist* related widgets and connect to model *setPostlayout* function
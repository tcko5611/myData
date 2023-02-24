---
date : 2022-11-25 08:50
priority : 3
---
# Metadata
Status :: #Status/Done  
Type :: #CF/Cockpit 
Summary :: 
Topics :: 
# Note
## fusa vec problem
* The problem comes from netlist is spice and will turn all node to lower case.
	* use strcasecmp function in ad_read_stderr.cpp to solve "vec" signal not match problem.
	* in af_parse_config.cpp, there is a vec signal check problem.
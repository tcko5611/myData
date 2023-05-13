---
date : 2023-03-01 22:56
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
How to show net2net fault in FuSa FDV
# Answer
# Note
## code change in printDiagFile
* add "port1", "port2" in csv file
* they are strings
* for fault, if no instance name then
	* call *parseNet2NetName* to get ins, portstring, net2net, ports
	* dump ports to ports position
	* if !net2netShort, rebuild fault by --i, net2net ^= 1, 


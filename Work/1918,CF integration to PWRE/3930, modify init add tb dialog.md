---
date : 2022-10-30 22:24
priority : 1
---
# Metadata
Status :: #Status/Done 
Type :: #CF/GUI 
Summary :: 
Topics :: 
# Note
1. Add cosim radiocheck, remove select netlist button
2. in MfgWidiget... add setCosim function 
	1. save a global variable cosim_ in Model as true(default is false, for analog netlist flow)
	2. when create new testbench, set "cosim" is true for their config
3. when start new App widget from current widget, set same cosim setting for it.
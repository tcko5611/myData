---
date : 2022-12-14 15:00
priority : 4
---
# Metadata
Status :: #Status/Progress 
Type :: #CF/GUI 
Summary :: 
Topics :: 
# Note
## Change GUI
* Change session tree view
	* each testbensh has it own fault list and result view like FuSa flow
## Change Model
* Enahnce clone process
	* copy gen_fault fault list to new testbench
	* remenber which testbench it copy from, so it will be able to do what-if flow
	* new testbench can generate new fault list, but after that, it cann't do waht-if flow
* Allow user to create totally new testbench as a new testbench
## Add Meas Dialog
* User can fill the meas and cck statement into the new dialog.
* Generate a new meas file from the dialog
* Add the new meas file into do fault campaign
## Enhance Cockpit What-if flow
* Cockpit should be able to create a new tag and extract the result(waveform) from a old fdb to do what-if flow.
## Integrate New What-if flow into CF App 
* CF App will allow clone testbench to do what-if flow from original testbench
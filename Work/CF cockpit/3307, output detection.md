---
date : 2022-06-30 08:59
aliases : []
priority : 1
---
# Metadata
Status :: #Status/Done
Type :: #CF/Cockpit 
Topics :: [P10241015-3307](https://jira.internal.synopsys.com/browse/P10241015-3307)
# Note
## Solution
* create a new function *generateOutputDetect* to generate csv file
	* build map *measIds* for meas(key) covered faults id's(value, vector)
	* build map *faultsName* for fault id(key), name(value)
	* create table for csv, column for meas, row for faults
	* output table to csv file
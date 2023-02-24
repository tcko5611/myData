---
date : 2022-09-06 10:41
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/Cockpit 
Summary :: 
Topics :: 
# Note
* Add ReportCoveredNodeFlag property to control create covered node report or not, default is not to generate covered node report.
* In parse config file, set the ReportCoveredNodeFlag.
* In main flow, after simulation when ReportCoveredNodeFlag is true, call PrintCoveredNode to generate report.
* In PrintCoveredNode
	* use a map to collect covered node touch number and then dump to the report.
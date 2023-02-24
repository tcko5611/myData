---
date : 2022-06-28 19:51
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/Cockpit 
Topics :: [P10241015-3492](https://jira.internal.synopsys.com/browse/P10241015-3492)
# Note
## Solution
* DataSingleton provide intermediate and inExit property to indicate state.
* in af_generate_report and af_diagonostic_report, use these two flags to determine to report WSS|Exhault report
---
date : 2023-02-23 09:15
priority : 4
---
# Metadata
Status :: #Status/Progress 
Type :: #CF/GUI 
# Question
How to integrate What-if flow to CF App
# Answer
# Note
## What-if flow for cockpit (different testbench name)
* in config file, we have a what_if_tag to specify the tb want to replace <br>
```json
"what_if" : { 
    "recalculate_coverage" : true,
    "what_if_tag" : "cfg61"
}
```
* CF has set af.RecalculateCoverge = *true*, af.WhatIfTag = *"xxx"*, af.LastRunDir= *"xxx"* to run new flow
## What-if flow in CF App
* set ui->pushButtonLoadFault->setVisible(false)
* copy from a old testbench
	* remove the fdb results for new testbench
	* set af.LastRunDir from old tb's fdb
	* set af.WhatIfTag from old tb's name
* when run what-if flow
	* set af.RecalculateCoverage = *true*
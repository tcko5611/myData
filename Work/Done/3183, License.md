---
date : 2022-04-17 21:46
aliases : []
---
# Metadata
Status :: #Status/Done 
Jira Type :: #CF/GUI
Topics :: [[CC-cpp]]
# Note
## Questions
1. The  CC_share object need to check out license from CC utilities, how to do that?
2. What is the mechanism of CC lib utility reference count behavior?
## Solution
1. Create a cf_gui\:\:LicManager singleton and provide member function
	* checkOutLic()
	* checkInLic()
	* Need to care about static member variable need to initialize.
2. In MfgCcWidget:  
```qt
connect(mfgWidget_, SIGNAL(destroyed(QObjcet*)), licManager, SLOT(checkInLic()))
```


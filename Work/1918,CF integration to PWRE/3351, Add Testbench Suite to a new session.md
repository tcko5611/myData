---
date : 2022-05-08 08:59
aliases : []
priority : 5
---
# Metadata
Status :: #Status/Done 
Type :: #CC/CPP 
Topics :: # Metadata
# Note
## file change fe6a5, prototype
* MfgWidget 
	* public
		* setFlowType(int type)
	* signals
		* selectFlowType();
		* updatePwTestSuite()
	* private
		* flowType_:FlowType
* StackedWidget
	* public
		* setFlowType(int type)
	* signals
		* selectFlowType()
		* updatePwTestSuite()
* StackedCcWidget
	* private slots
		* selectFlowType()
		* updatePwTestSuite()
		* selectedFlow(int i)
	* private
		* setFlowType(int type)
		* flowType_: FlowTyqpe
* SelectFlowDialog
## implement updatePwTestSuite
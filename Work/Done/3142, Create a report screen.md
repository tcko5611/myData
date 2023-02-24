---
date : 2022-04-24 18:14
aliases : []
priority : 5
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI 
Topics :: # Metadata
# Note

DONE 3142, Create a test coverage report screen
	1. Qt creater add a new Test Coverage report widget, add two buttons, one plaintextedit, one table view,
	2. create QAfData.report1 for plaintextedit,
	3. build flistd connect with tableview with QStandardItemModel
2022/4/28,
	1. move test benches results to the end of table
		1. mfgwidget
			1. set the ResultDelegate to the testbenches column in TableView(fl_end in TableModel)
			2.  remove SimpleSortFilterProxyModel set background color code for ui->tableViewFlist.
		2. MfgModel
			1. set the new order of fixed field and testbenches field, modify get data from flist in flistTableModel_().
			2. TableModel's flist comes from Model::simulateFlistDoc_ and TableModel is a member of Model.
2022/4/29,
	1.
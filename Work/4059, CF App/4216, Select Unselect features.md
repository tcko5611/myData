---
date : 2023-03-27 09:48
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
implement select/unselect for Mfg, FuSa and SiFa flow
# Answer
# Note
in MfgWidget.cpp, FuSaWidget.cpp and SiFaWidget.cpp change action slot functions
```cpp
void MfgWidget::checkSelectedTableUniverseFlistItem()
{
	...
	// QModelIndexList selected = ui->tableViewUniverseFlist->selectionModel()->selectedRows(FlistColumnIndex::fl_Checked);
	QModelIndexList selected = ui->tableViewUniverseFlist->selectionModel()->selectedIndexes();
	...
}
void MfgWidget::unCheckSelectedTableUniverseFlistItem()
{
	...
	// QModelIndexList selected = ui->tableViewUniverseFlist->selectionModel()->selectedRows(FlistColumnIndex::fl_Checked);
	QModelIndexList selected = ui->tableViewUniverseFlist->selectionModel()->selectedIndexes();
	...
}
```

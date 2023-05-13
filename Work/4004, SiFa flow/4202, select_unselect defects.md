---
date : 2023-03-03 14:06
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
add select/unselect functionality
# Answer
# Note
Code change:
	* add select/unselect action to table customContextMenuRequested slot
	* in select/unselect action trigger function
```cpp
QModelIndexList selected = ui->tableViewUniverseFlist->selectionModel()->selectedIndexes();
for (auto &index : selected) {
	...
}
```
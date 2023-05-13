---
date : 2022-06-28 19:29
aliases : []
priority : 1
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI 
Topics :: [[Qt Model, View, Delegate]]
# Note
## model classes for tableViewUnivereFlist
* faultListProxyModel_ (*SimpleSortFilterProxyModel* <- *QSortFilterProxyModel*) -> model of tableViewUniverseFlist, by setModel
	* setSourceModelI(faultListModel_) -> set source model
	* mapTopSource(index) -> get index of source model
	* setFilterRegExp(regExp) -> set filter regexpr
	* filterAcceptsRow <- rewrite *override* function to match string
* faultListModel_ (*QStandardItemModel*) -> move to QAbstractTableModel
	* itemFromIndex() <- should get index and use *setData* to change data
	* setRowCount(0) <- change *rowCount* return value
	* setColumnCount(0) <- change *columnCount* return value
	* horizontalHeaderItem(1) ->setToolTip -> rewrite *headerData* overrirde function
	* setHorizontalHeaderLabels -> rewrite *headerData* overrirde function
	* insertRow -> rewrite insertRows
	* itemChanged (*signal*) -> dataChanged
	* setItem -> change model data
* FlistTableModel
	* void updateData()
	* void onTaskChanged(const QString &runTag, int faultId, Document &flistd)
	* const rapidjson::Value& getFaultFromId(int id)

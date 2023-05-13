---
date : 2022-07-29 16:32
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Done 
Type ::  #Prog/Info 
Topics :: 
# Note
## QAbstractTableModel
### Basic virtual function need to implemented
* *rowCount(const QModelInex &parent = QModelIndex()) const*
* *columnCount(const QModelInex &parent = QModelIndex()) const*
* *QVariant data(const QModelIndex &index, int role = Qt::DisplayRole) const*
* assume that you have a data type *ModelData* for the columns in row, then you can use l_ = QList\<ModelData\> to store your data, 
* for rowCount return l_.size(), columnCount() return ModelData data(should be QListString for add multiple tbs)
### Change header
* *QVariant headerData(int section, Qt::Orientation orientation, int role = Qt::DisplayRole) const*
### upadte for data change
* emit signal *dataChanged* for only current layout(not add/delete row column), 
* emit *layoutAboutToBeChanged*, *layoutChanged* to update QTableView when row, column changed
* in remove/insert rows, call *beginRemoveRows/beginInserttRows*, *endRemoveRows/endInserttRows* to let tablemodle
### basic flow
* assume that we have data_(QList\<QListString\>) to display in table, and headerList_(QStringList) in header
* using data_.size() and data_\[0\].size() as *rowCount* and *columnCount*
* display headerList_\[i\] in *headerData* Qt::DisplayRole, display data_\[row\]\[column\] in *data* Qt::DisplayRole
* update data_ and emit *layoutAboutToBeChanged*, *layoutChanged* signal to update table view.
* update data_\[i\]\[j\], emit *dataChanged* signal to update cell
* emit some data for *View* to use
## QSortFilterProxyModel
### Basic
* *QSortFilterProxyModel* is very expensive with using *QStandardItemModel*, for it need to execute filter option when *data* changed. It will be suitable for user define *QAbstractTableModel*.
* use *setFilterRegExp(QRegExp)* to set filter criterio.
* rewrite *filterAcceptsRow(int row, const QModelIndex &parent) const* to filter out data, *sourceModel* to get the source model.
* use *setSourceModel* to set source data model
* define some slot to take the filter option from other GUI
## QTableView
* use *setHorizontalHeader* to set user defined QHeaderView
* use *setSortingEnabled(True)* to enable sorting(control by model, but will show sort indicator in GUI) 
* use *setModel* to set Model
## QHeaderView
* in derived call can use *QHeaderView(Qt.Horizontal, parent)* to define horizotal header
* need to *setSectionsClickable(True)* to enable click for sorting
* use *setDefaultAlignment* to define tiltles position
* when has Editors in header 
	* define *setFilterBoxes* to create editor for each column
	* define somes slots to set editors contains
	* modify *sizeHint* to adjust sizeHint.height = size.height() + editors.height
	* define *adjustPositions* to define editor's size and location(*move*)
	* modify *updateGeometries* to set *self.setViewportMargins(0, 0, 0, height + self._padding)* to freeze the view
	* connect signals *self.sectionResized* or *parent.horizontalScrollBar().valueChanged* to *self.adjustPositions*

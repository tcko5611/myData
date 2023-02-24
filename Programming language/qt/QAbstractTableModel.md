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
## Basic virtual function need to implemented
* *rowCount(const QModelInex &parent = QModelIndex()) const*
* *columnCount(const QModelInex &parent = QModelIndex()) const*
* *QVariant data(const QModelIndex &index, int role = Qt::DisplayRole) const*
* assume that you have a data type *ModelData* for the columns in row, then you can use l_ = QList\<ModelData\> to store your data, 
* for rowCount return l_.size(), columnCount() return ModelData data(should be QListString for add multiple tbs)
## Change header
* *QVariant headerData(int section, Qt::Orientation orientation, int role = Qt::DisplayRole) const*
## upadte for data change
* emit signal dataChanged, layoutChanged to update QTableView
## basic flow
* assume that we have data_(QList\<QListString\>) to display in table, and headerList_(QStringList) in header
* using data_.size() and data_\[0\].size() as *rowCount* and *columnCount*
* display headerList_\[i\] in *headerData* Qt::DisplayRole, display data_\[row\]\[column\] in *data* Qt::DisplayRole
* update data_ and emit *dataChanged*, *layoutChanged* signal to update table view.
* update data_\[i\]\[j\], emit *dataChanged* signal to update cell
## Note for QSortFilterProxyModel
* *QSortFilterProxyModel* is very expensive with using *QStandardItemModel*, for it need to execute filter option when *data* changed. It will be suitable for user define *QAbstractTableModel*.
* use *setFilterRegExp(QRegExp)* to set filter criterio.
* rewrite *filterAcceptsRow(int row, const QModelIndex &parent) const* to filter out data, *sourceModel* to get the source model.
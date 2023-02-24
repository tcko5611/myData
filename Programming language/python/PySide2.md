
# QAbstractTableModel 
## changed data_ need to update 
``` python
with open(fileName) as f:
  self.data_ = json.load(f)
self.layoutChanged.emit()
pass
```
## remove and insert row
``` python
def insertRows(self, row, count, parent = QModelIndex()):
	if (count < 1 ) or (row < 0) or (row > self.rowCount()) or parent.isValid() :
		return False

    self.beginInsertRows(QModelIndex(), row, row + count - 1)
    for i in range(count):
      t = [''] * 8
      self.data_.append(t)
    self.endInsertRows()
    return True
def removeRows(self, row, count, parent = QModelIndex()):
    self.beginRemoveRows(QModelIndex(), row, row + count - 1)
    for i in range(count):
      del self.data_[row]
    self.endRemoveRows()
    return True
```
---
date : 2022-11-07 09:16
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI
Summary :: 
Topics :: 
# Note
* Need to handle *right click* event in QTableWidget only to emit signal *customContextMenuRequested*
	* write a *RightClickTable* from *QTableWidget*
```cpp
protected:
mousePressEvent(QMousePressEvent *event)
{
	if (event->button() == Qt::RightButton) {
	    emit customContextMenuRequested(event->pos());
    }
    else {
        QTableWidget::mousePressEvent(event);
    }
}
```
}
* Need to handle action in Contextmenu with data of row like
```cpp
// in customContextMenuRequested funtion
action.setData(table->indexAt(pos).row());
// in slot function
action = qobject_cast<QAction *>(sender());
int row = action->data().toInt();
...

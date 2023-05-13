 ***ui->tableViewUniverseFlist,  faultListProxyModel_, faultListModel_**
		  faultListProxyModel_ = new SimpleSortFilterProxyModel(this);
		  faultListModel_ = new QStandardItemModel(this);
		  faultListProxyModel_->setSourceModel(faultListModel_); 
		  ui->tableViewUniverseFlist->setModel(faultListProxyModel_);
		  selectionModel = ui->tableViewUniverseFlist->selectionModel();
		  headerView = ui->tableViewUniverseFlist->horizontalHeader()
		  **tableViewUniverseFlist**
		  setContextMenuPolicy(Qt::CustomContextMenu);
		  clearSelection()
		  setSortingEnabled(true);
		  setItemDelegateForColumn(i, resultDelegate_);
		  currentIndex()
		  mapToGlobal(pos)
		  **selectionModel**
		  connect(selectionModel,
		  SIGNAL(selectionChanged(const QItemSelection &, const QItemSelection &)),
		  this, SLOT(copySelected(const QItemSelection &, const QItemSelection &)));
		  QModelIndexList selected = selectedRows(FlistColumnIndex::fl_Checked);
		  selectedIndexes()
		  **horizontalHeader**
		  setSectionResizeMode(QHeaderView::Interactive);
		  setMinimumSectionSize(50)
		  setSortIndicator(-1, Qt::AscendingOrder);
		  setSectionResizeMode(FlistColumnIndex::fl_Id, QHeaderView::ResizeToContents)
		  **faultListProxyModel_**
		  setSourceModel(faultListModel_)
		  rowCount()
		  index(i, 1)
		  mapToSource(index)
		  setFilterRegExp(regExp)
		  **faultListModel_**
		  horizontalHeaderItem(1)->setToolTip(tooltip)
		  itemFromIndex(faultListProxyModel_->mapToSource(index))
		  setRowCount(0)
		  horizontalHeaderItem(1)->setToolTip(tooltip);
		  horizontalHeaderItem(1)
		  setColumnCount(FlistColumnIndex::FlistColumnEnd);
		  setHorizontalHeaderLabels(tableHeaders);
		  connect(faultListModel_,
		  SIGNAL(itemChanged(QStandardItem*)), this,
		  SLOT(checkItemChanged(QStandardItem*)));
		  rowCount()
		  insertRow(rowCount)
		  setItem(rowCount, j, item)
		  item(row, col)
		  insertColumn(FlistColumnIndex::fl_Id)
		  setHorizontalHeaderItem(FlistColumnIndex::fl_Id, hitem);
		- **task done flow**
		  MsaterFlow.execute->afdata_obj.taskDone(n_complted, faultId)->
		  ::taskDone(QAfData, n_completed, faultId) ->QAfData.taskDone(n_completed, faultId)->
		  SIGNAL(taskChanged)->Model.onTaskChanged()->SIGNAL(taskChanged)->
		  MfgWidget.onTaskChanged()->updateFaultId(faultId, model_.getRunTag())
		- **ResultDelegate**
			1. save painter, text= index.data().toString()
			2. rec=(rect.x()+3, rect.y()+10, 11, 11), brush(Qt::SolidPattern)
			3. use text select color: brush.setColor()
			4. paiter.drawRect(cb), painter.fillRect(cb, brush)
			5. painter.drawText(cb.x()+ 14, cb.y()+11, text)
			6. restore painter() 
# treeViewSession
## treeView
ui->treeViewSession->resizeColumnToContents(0);
ui->treeViewSession->setModel(sessionModel_);
  ui->treeViewSession->setContextMenuPolicy(Qt::CustomContextMenu);
  ui->treeViewSession->setCurrentIndex(sessionModel_->index(tbInfos_.size()-1, 0));
  const QModelIndex &curIdx = ui->treeViewSession->currentIndex();
  BlockSignals bs2(ui->treeViewSession);
  QMenu tbMenu(ui->treeViewSession);
  tbMenu.exec(ui->treeViewSession->mapToGlobal(pos));
  ui->treeViewSession->selectionModel()
  ui->treeViewSession->header()->setDefaultAlignment(Qt::AlignHCenter);
  ui->treeViewSession->setSortingEnabled(false)
  ui->treeViewSession->setExpanded(index, tbInfo->expanded_);
  connect(ui->treeViewSession,
          SIGNAL(expanded(const QModelIndex &)),
          this,
          SLOT(sessionExpanded(const QModelIndex &)));
  connect(ui->treeViewSession,
          SIGNAL(collapsed(const QModelIndex &)),
          this,
          SLOT(sessionCollapsed(const QModelIndex &)));
  connect(ui->treeViewSession->selectionModel(),
          SIGNAL(currentChanged(const QModelIndex &, const QModelIndex &)),
          this,
          SLOT(sessionCurrentChanged(const QModelIndex &, const QModelIndex &)));
ui->treeViewSession->setExpanded(index, tbInfo->expanded_);
## selectionModel
    connect(ui->treeViewSession->selectionModel(),
          SIGNAL(currentChanged(const QModelIndex &, const QModelIndex &)),
          this,
          SLOT(sessionCurrentChanged(const QModelIndex &, const QModelIndex &)));        
BlockSignals bs(ui->treeViewSession->selectionModel());
## sessionModel(QStandardItemModel)
sessionModel_ = new QStandardItemModel(this);
ui->treeViewSession->setModel(sessionModel_);
connect(sessionModel_,
          SIGNAL(itemChanged(QStandardItem \*)),
          this,
          SLOT(sessionItemChanged(QStandardItem \*)));
ui->treeViewSession->setCurrentIndex(sessionModel_->index(tbInfos_.size()-1, 0));
setSessionNameBold(tbInfos_.size()-1, true);
QString name = sessionModel_->index(i, 0).data(Qt::DisplayRole).toString();
QStandardItem \*curItem = sessionModel_->item(i, 1);
const QModelIndex &current = sessionModel_->index(currentRow, currentColumn);
sessionModel_->item(row, 0)->hasChildren()
row >= sessionModel_->rowCount()-g_globalSettingSize
ui->stackedWidget->setCurrentIndex(row-sessionModel_->rowCount()+g_globalSettingSize);
int lastrow = sessionModel_->rowCount() - g_sessionDefaultSize;
sessionModel_->setHorizontalHeaderItem(0, item);
sessionModel_->itemFromIndex(index)->hasChildren()
sessionModel_->removeRow(row);
sessionModel_->setRowCount(0);
sessionModel_->setColumnCount(tableHeaders.size());
sessionModel_->setHorizontalHeaderLabels(tableHeaders);
sessionModel_->insertRow(cnt);
sessionModel_->setItem(cnt, 0, item);
# selectionModel
connect(ui->tableViewUniverseFlist->selectionModel(),
          SIGNAL(selectionChanged(const QItemSelection &, const QItemSelection &)),
          this, SLOT(copySelected(const QItemSelection &, const QItemSelection &)));
QModelIndexList selected = ui->tableViewUniverseFlist->selectionModel()->selectedRows(FlistColumnIndex::fl_Checked);



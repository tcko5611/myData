---
date : 2022-07-19 08:56
aliases : []
priority : 1
---
# Metadata
Status :: #Status/Done
Type ::  #CF/GUI 
Topics :: 
# Note
## Main Purpose
Change the global setting widget  to top
## Solution
1. ui->treeViewSession is control by sessionModel_ and tbInfos_
2.  start, *updateSessionTable*
	1. define header in sessionModel_
	2. change rows *"Defect Model"*, *"<+Add Tb"* to  sessionModel_
3. add testbench, *onNewSessionGenerated(name)*
	1. add new tb to tbInfo_
	2. *insertTestbench*, ->add item to sessionModel_ at (tbInfo_.size()) for sessionModel_'s first row is global defect model, addTestbenchChildren
	3. updateSessions 
		1. from tbInfos_ update sessionModel_ contain
		2. from tbInfos_ index i must correspond to sessionModel_->item(i+1, 0) for first row in sessionModel_ is global "defecft model"
	4. set current index is the last one(tbInfos_.size(), add 0 is glocal defect model widget)
4. deletesession
	1. call model_->removeSession(currSession)
	2. row = ui->treeView->currentRow, sessionModel_->removeRow(row), tbInfos_.removeAt(row)
5. moveTestbench(src, dst)
	1. tb = tbInfos_\[src\]; tbInfos_->removeAt(src); tbInfos_.insert(dst, tb)
	2. updateSessions()
6. change current tb, *sessionCurrentChanged(QModelIndex &current, QModelIndex &previous)*
	1. find row in sessionModel_(not in child)
	2. if not item(row, 0) has children :  
			assert (row == 0 || row == tbsSize +1)
			if row == 0:
				set ui->stackedWidget(0)
			return
	3. if current is child :
			set local ui->stackedWidget
		else:
			set ui->stackedWidget tb setting widget
	1. idx = current tb index
	2. newName from idx
	3. currName from model
	4. if newName == currName : return
	5. get preIdx from previous
	6. oldName from preIdx
	7. model_->getSession(newName)
7. when use  setSessionNameBold(i, bool) or sesssionModel_->index(i,0) if i comes from tbInfos_, then i must add 1 for the functions set sessionModel_ data.
8. Using moveTestbench(src, dst), for row comes from sessionModel_, so row in \[1, tbInfos_.size()\]
9. in *on_treeViewSession_doubleClicked*, the select flow or add item in in last row sessionModel_->rowCount() - 1
10. In *setFlowfunctios*, the row = sessionModel_->rowCount() - 1(last one)


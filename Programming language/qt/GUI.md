# GUI

## QMainWindow

Create a main window like follows:

```
MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);
	....
}

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
    // QApplication a(argc, argv); /* for GUI application */
	...
    return a.exec();
}

```

## QTableWidget

Create a table widget like follows:

```
ui->tableWidget->setColumnCount(5); 
ui->tableWidget->setShowGrid(true); 
ui->tableWidget->setSelectionMode(QAbstractItemView::SingleSelection);
ui->tableWidget->setSelectionBehavior(QAbstractItemView::SelectRows);
ui->tableWidget->setHorizontalHeaderLabels(headers); // QStringLIst
ui->tableWidget->horizontalHeader()->setStretchLastSection(true);
ui->tableWidget->hideColumn(0);
ui->tableWidget->insertRow(i);
ui->tableWidget->setItem(i,0, new QTableWidgetItem(query.value(0).toString()));
QTableWidgetItem *item = new QTableWidgetItem();
item->data(Qt::CheckStateRole);
/* Check on the status of odd if an odd device, 
 * exhibiting state of the checkbox in the Checked, Unchecked otherwise
 * */
if(query.value(1).toInt() == 1){
  item->setCheckState(Qt::Checked);
} else {
  item->setCheckState(Qt::Unchecked);
}
// Set the checkbox in the second column
ui->tableWidget->setItem(i,1, item);
/ Next, pick up all the data from a result set in other fields
ui->tableWidget->setItem(i,2, new QTableWidgetItem(query.value(2).toString()));
ui->tableWidget->setItem(i,3, new QTableWidgetItem(query.value(3).toString()));
ui->tableWidget->setItem(i,4, new QTableWidgetItem(query.value(4).toString()));

```
## using sizehint
```
sizePolicy : fixed,  prefered, expanding are good to used
notice : need to be careful about minimumsize and maxximumSize when use layout features.
```
## Delegate
* In case if we have a complex delegate editor problem, we can do the following thing
```cpp
QWidgte *creator(...) {
	auto editor = new QWidget(parent);
	auto dialog = new MultiSelectDialog(editor);
	...
	connect(dialog, SIGNAL(accepted()), this, SLOT(commitAndCloseEditor()));
	connect(dialog, SIGNAL(rejected()), this, SLOT(commitAndCloseEditor()));
	return editor
}
void setEditorData(...) {
	dlg = editor->findChild<MultiSelectDialog *>();
	...
	dlg->open();
	dlg->move(QCursor::pos());
}
void setModelData(...) {
	dlg = editor->findChild<MultiSelectDialog *>();
	...
	model->setData(index,value,Qt::EditRole);
}
void commitAndCloseEditor() {
	QWidget *editor = qobject_cast<QWidget *>(sender())->parentWidget();
	emit commitData(editor);
	emit closeEditor(editor);
}
```

---
date : 2023-04-26 15:00
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
The widgets show/hide not as expect
# Answer
# Note
## code
MfgWidget.cpp, refer to [[Qt GUI#GUI#using sizehint|Qt sizehint]]
```cpp
// need to first call spacer changeSize, then call show/hide for widget 
void cf_gui::MfgWidget::on_checkBoxNCAReport_toggled(bool checked)
{
  DumpCfLog d{};
  BlockSignals bs(ui->checkBoxNCAReport);
  if (!checked && (ui->checkBoxDfa->checkState() == Qt::Unchecked)) {
    ui->verticalSpacer_3->changeSize(20, 0, QSizePolicy::Minimum,
                                     QSizePolicy::Expanding);
  }
  else {
    ui->verticalSpacer_3->changeSize(20, 0, QSizePolicy::Minimum,
                                     QSizePolicy::Minimum);
  }
  if (checked) {
    ui->tableViewNCA->show();
  }
  else {
    ui->tableViewNCA->hide();
  }
  dumpCfLog({__func__, QVariant(checked).toString()});
}
```


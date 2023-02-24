# Question
Where is qt settings file location? How to save Qt settings?
# Answer
## setting location
```c++
QCoreApplication::setOrganizationName("Synopsys");
QCoreApplication::setOrganizationDomain("synopsys.com");
QCoreApplication::setApplicationName("NetlistTreemapViewer");
```
the file will store in "~/.config/Synopsys/NetlistTreemapViewer.conf"
## how to save/restore  settings
### save
```cpp
QSettings setting;
setting.beginGroup("SCHEMATIC");
setting.setValue("cxbin", _cxbin->text());
...    
setting.endGroup();
setting.sync();
```
## restore
```cpp
QSettings setting;
setting.beginGroup("SCHEMATIC");
if (setting.value("cxbin").isValid())
      _cxbin->setText(setting.value("cxbin").toString());
...
setting.endGroup();
```
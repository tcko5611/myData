<---
date : 2022-05-04 09:49
aliases : []
priority : 5
---
# Metadata
Status :: #Status/Info
Type :: #CF/GUI 
Topics :: # Metadata
# Note
## From PWDE, invokeMfg, FuSa, SiRa
* in cf_sae_link.tcl, cf::showMenu define menus and correpoding functions
	* cf::stacked::invokeMfg
	* cf::stacked::invokeFuSa
	* cf::stacked::invokeSiFa
```mermaid
sequenceDiagram
participant PWDE
participant CC
participant StackedCcWidget
PWDE ->>+ CC : invokeMfg pwWin
CC ->> StackedCcWidget : winId = createWindow -lib "" -cell "" -view ""
CC ->> StackedCcWidget : initWindow $winId mfg $pwWin 
CC ->> CC : setAnalysisType mfg
CC ->> CC : createFaultSimJson $pwWin
CC ->> StackedCcWidget : loadSessions $winId ".faultsim.json" $pwWin
CC ->> CC : initCurrentPwde $pwWin
CC ->> CC : initCurrentSession $winId $pwWin
CC ->>- PWDE : return
```
## From click on CC customfault icon
* for icon we use "cf::stacked::invoke"
```mermaid
sequenceDiagram
participant User
participant CC
participant StackedCcWidget
User ->>+ CC : invoke $lib $cell $view $viewFile
CC ->> StackedCcWidget : winId = createWindow -lib $lib -cell $cell -view $view
CC ->> CC : initCurrentSession $winId
CC ->> StackedCcWidget : pwWin = initWindow $winId viewFile $viewFile 0
CC ->> CC : initCurrentPwde $pwWin
CC ->> CC : setSessionData pwWin $pwWin $winId
CC ->>- User: return
```

* code in loadLib.tcl, oa::... define customfault view and the entry point is cf::stacked::invokeView
```mermaid
sequenceDiagram
participant User
participant invokeView
User ->>+ invokeView : invokeView dmFile readOnly attrs
invokeView ->> invokeView : extract lib, cell, view from dmFile
invokeView ->> loadView : loadView $lib $cell $view
loadView ->> loadView : find state file 
alt founded state file
loadView ->> invoke : invoke $lib $cell $view $viewFile
else
loadView ->> invoke : invoke $lib $cell $view
end
invokeView ->>- User: return
```
## double click on session panel
### first time, need to select flow
#### Sequence diagram
```mermaid
sequenceDiagram
  participant User
  participant FuSaWidget
  participant StackedCcWidget
  participant SelectDialog
  participant CC
  User ->>+ FuSaWidget: double clicked
  FuSaWidget->>StackedCcWidget : selectFlowType
  StackedCcWidget ->> SelectDialog : invoke
  alt is TestSuite
    SelectDialog ->>+ StackedCcWidget: bindPw(pwLib, pwCell, pwView)
    StackedCcWidget ->> CC : cf::utils::openViewWindow
    StackedCcWidget ->> CC : cf::createFaultSimJson
    opt loadSessions
      StackedCcWidget ->> FuSaWidget : loadSessions
    end
    StackedCcWidget ->> FuSaWidget : setFlowType(TestSuite)
    StackedCcWidget -->>- SelectDialog : return
  else is Netlist 
    SelectDialog ->>+ StackedCcWidget: createNewTestbench(tb, nl)
    StackedCcWidget ->> FuSaWidget : setFlowType(Netlist)
    StackedCcWidget ->> FuSaWidget : createNewTestbench(tb, nl)
  end
  FuSaWidget -->>- User: return
```
#### Calass diagram
```mermaid
classDiagram 
direction LR
class FuSaWidget {
  +selectFlowType()
  +loadSessions()
  +setFlowType(int)
  +createNewTestbench(tb, nl)
}
class StackedCcWidget {
  +selectFlowType()
  +bindPw(lib,cell,view)
  +loadSessions()
}
class SelectDialog {
  +invoke()
}
class Cc {
 +cf::utils::openViewWindow()
 +cf::createFaultSimJson()
}
FuSaWidget *-- StackedCcWidget 
StackedCcWidget --* SelectDialog
StackedCcWidget --> Cc
```
### PW test suite flow
```mermaid
sequenceDiagram
  participant User
  participant FuSaWidget
  participant StackedCcWidget
  participant CC
  User ->>+ FuSaWidget : updatePwTestSuite
  FuSaWidget ->> StackedCcWidget : updatePwTestSuite(fN)
  StackedCcWidget->> CC : cf::sae::updateSession fN (tcl)
  FuSaWidget ->> FuSaWidget : updateSessions(fN)
  FuSaWidget-->>- User : return
  
```
### netlist flow
```mermaid
sequenceDiagram
  participant User
  participant FuSaWidget  
  participant FuSaModel
  User ->>+ FuSaWidget : on_actionNew_triggered
  FuSaWidget ->> FuSaWidget : widgetsToModel()
  FuSaWidget ->> FuSaModel : newSession(name)
  FuSaWidget -->>- User : return  
```
## PushButtons
### Reload Expr
```mermaid
sequenceDiagram
  participant User
  participant FuSaModel
  participant FuSaWidget
  participant StackedCcWidget
  participant CC
  User ->>+ FuSaWidget : on_actionReloadExpr_triggered
  FuSaWidget ->> StackedCcWidget : updateTb(tb, fN)
  StackedCcWidget ->> CC : cf::sae::updateTbWithName tb fN(tcl)
  FuSaWidget ->> FuSaModel : clear EXpr, SimStop
  FuSaWidget ->> FuSaModel : updateTb(fN)
  FuSaWidget -->>- User : return
  
```


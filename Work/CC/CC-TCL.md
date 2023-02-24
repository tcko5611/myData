sa::---
date : 2022-05-17 13:28
aliases : []
priority : 1
---
# Metadata
Status :: #Status/Progress 
Type :: #CC/TCL
Topics :: # Metadata
# Note
## window(gi::\*)
* gi::getActiveWindow *//set win \[gi::getActiveWindow\]
* gi::setActiveWindow *//gi::setActiveWindow $winId -raise true
* gi::getWindows *//set win \[gi::getWindows $winId\], set wins \[gi::getWindows -type \[gi::getWindowTypes $type \]
* set windowType \[db::getAttr windowType -of $win\]
* \[db::getAttr windowType.name\] == "cfStackedLaunch"
* \[db::getAttr win.id\] == 1
* gi::createAppIcon *// 
* gi::createCarouselItem *//
* **gi::createDialog**
* **gi::createListInput**
* **gi::execDialog**
* **gi::findChild**
* gi::prompt *// like QmessageBox*
## Database (db::)
* db::checkVersion *// \[db::checkVersion -atLeast 2022.06-ALPHA-20220304\]
* db::createCallback *// no use for new version
* db::eval "cf::stacked::cfReplay $winId $cmd" -log true *// write to log*
* db::expr *// like tcl expression for add engineering suffixes
* db::foreach
* db::getAttr
* db::getCount
* db::getIndex \[expr $l -1\] -of $wins
* db::getNext
* db::getPrefValue *// set useSsl2 \[db::getPrefValue $prefName\]
* db::getProcessInfo *// set args \[db::getAttr args -of \[db::getProcessInfo\]\]
* db::init
* db::isEmpty
* db::isObject
* db::resolve *// find real path from TCL PATH
* db::setAttr icon -value a.png -of \[gi::getWindowTypes cfStackedLaunch\]
* db::vwait *// db::vwait until variable change*
## Design Editing (de::)
* de::createApplication *// create application for CC L/C/V*
* de::createArgument 
* de::createAssociation *// create view  
* de::createCommand
* de::open *// de::open $dmPWDECellView -readOnly 1 
* de::sendMessage "information" -severity information
## Simulation Script(ss::)
* set se \[ss::getActiveSession\]
* set se \[db::getNext \[sa::getSessions -window $win\]\]
## Data Management(dm::)
* dm::findCell *//set dmCellView \[dm::createCellView $viewName -cell \[dm::findCell $cellName -lib $lib \] -viewType customfault\]
* dm::findCellView : *// set dmCellView \[dm::findCellView $viewName -cellName $cellName -libName $libName\]
* dm::findDMFile : *// set designFile \[db::getAttr path -of \[dm::findDMFile "customfault.file" -dmContainer $dmCellView\]\]
* dm::getCells: *// \[dm::getCells $cellName -libName $libName\]*
* dm::getCellViews : *// \[dm::getCellViews -cell \$cell -filter %viewType=~/^$viewType/\]
* dm::getLibs *// set libs \[db::createList \[db::getAttr name -of \[dm::getLibs\]\]\]

## Simulation and Analysis(sa::)
### get testbench, lib/cell/view
```tcl
set session [sa::getSessions -window $windID]
    if {[::sa::_utils::isTestSuite $session]} {
        # get testbenches
        set tbs [sa::getTestbenches -of $se]
        # get lib/cell/view        
        set storage [sa::getTestSuiteStorage -storageType session -session $session]
        puts "Lib: [db::getAttr storage.library]"
        puts "Cell: [db::getAttr storage.cell]"
        puts "View: [db::getAttr storage.name]"
    } else {
        # STB session
        set testbench [sa::getTestbenches -of $session]
        puts "Lib: [db::getAttr testbench.openaccessState.libName]"
        puts "Cell: [db::getAttr testbench.openaccessState.cellName]"
        puts "View: [db::getAttr testbench.openaccessState.name]"
    }
```
#### get model include flag
```
proc _appendIncludePaths {tb command} {
        set envOpts [db::getAttr environmentOptions -of $tb]
        set includePaths [db::getAttr includePath -of $envOpts]
        set models [db::getAttr models -of $tb]
        set modelPath [db::getAttr includePath -of $models]
        set resolvedIncludePaths {}
        foreach includePath $includePaths {
            append command " -I [sa::_utils::getResolvedPath $includePath]"
            lappend resolvedIncludePaths [sa::_utils::getResolvedPath $includePath]
        }
        if {$modelPath != "" && [lsearch -exact $resolvedIncludePaths [sa::_utils::getResolvedPath $modelPath]] == -1} {
            append command " -I [sa::_utils::getResolvedPath $modelPath]"
        }
        return $command
    }
```
### misc
* sa::actions
* sa::createNetlist -testbench $tb -createStructural 1
* sa::createRunScript *// sa::createRunScript -testbench $tb, cosim
* sa::createSession *// create session for wave viewer 
* set netdir \[sa::getNetlistDir -of $tb\]
* set se \[sa::getSessions -window $win\]
* sa::getTestbenches *// set tbs \[sa::getTestbenches -of $se\]
* sa::HSPICE
*  set prefName \[sa::makePrefName "envOpts useSsl2" -simulator $simulator\]
* sa::map
* sa::menus
* sa::resolveDesignVariables
* sa::showDesignWindow *//sa::showDesignWindow -parent $pwWin
* sa::VCSUtils
* sa::XAHSPICE
* set script \[sa::\_utils::getRunScriptName $tb\] *// get cosim run script*
* \[sa::\_utils::getSimulator \[db::getAttr simulator -of $tb\]\] // get simulator of sae
*  set outputs \[sa::\_utils::getOutputs $tb\] *//  get output for measure*
## OA database
* oa::CellFind *// set cellView \[oa::CellViewFind $lib $cellName $viewName\]
* oa::CellViewFind
* oa::getAccess *// if {!\[oa::getAccess $lib oa::LibAccess oa::oacWriteLibAccess\]\]}
* oa::getView 
* oa::LibFind *//  set lib \[oa::LibFind $libName\]
* oa::oacWriteLibAccess *// if {!\[oa::getAccess $lib oa::LibAccess oa::oacWriteLibAccess\]\]}
* oa::releaseAccess *// oa::releaseAccess $lib
* oa::ViewTypeCreate
## digital net to analog net
```tcl
# Get the current testbench
set tb [ss::getActiveTestbench]
# Get the top design’s hierarchy context
set envOpts [db::getAttr tb.environmentOptions]
set cellView [db::getAttr tb.design]
set heCtx [he::createContext -cellView $cellView -viewSearchList [db::getAttr envOpts.switchViewList] -viewStopList [db::getAttr envOpts.stopViewList]]
# Get the equivalent nets
set eqvNets [db::getEquivalentNets a4 -context $heCtx]
# The result is a list of sublists, each sublist is a pair of a hierarchy context object and the equivalent net object
foreach pair $eqvNets {
    lassign $pair ctx net
    set ctxPath [db::getAttr ctx.occurrence]
    set netName [db::getAttr net.name]
    puts "Path: $ctxPath, Net: $netName"
}
# The names above are schematic names. To get the netlist names, you will need to map them as before.
# For example:
sa::map /xa4/M0/D -testbench $tb -type net
# will return xa4.m0.D
```
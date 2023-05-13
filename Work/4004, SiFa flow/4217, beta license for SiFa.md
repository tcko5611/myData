---
date : 2023-03-20 10:05
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
* Remove CF_APP_DVP from SiFa
* check out beta license for SiFa
# Answer
# Note
## code change
### cf_sae_link.tcl
```tcl
# change showMenu to add SiFa Menu
proc showMenu { } {
	...
	gi::addActions { actionInvokeCfMfg } -to $m
	gi::addActions { actionInvokeCfFuSa } -to $m
	gi::addActions { actionInvokeCfFailure } -to $m
}
namespace eval ::startup {
	cf::showMenu
	cf::stacked::setWidowTypeIcon
	cf::isInReplay
}
```
### cfStacked.tcl
```tcl
proc invokeSiFa {pwWin} {
	...
	# add -type "SiFa" to check out beta license for SiFa
	set winId [cf::stacked::createWindow -lib "" -cell "" -view "" -type "SiFa"] 
}
```
### loadLib.tcl
```tcl
namespace eval cf::stacked::createWindow {
	...
	# add new argument for create SiFa widget for license to check
	lappend argList [de::createArgument "-type" \
	    -description "type name" \
	    -optional 1 ]	
}
```
### CcCf.cpp
```cpp
void initCcCfWindowTypes(Tcl_Interp* interp) {
	g_interp = interp;
	/* remove not to check out license here
	if (cf_gui::LicManager::getInstance().checkOutBeta()) {
		QString cmd = QString("cf::showMenu 1");
		cf_gui::execTclCmd(cmd);
		cf_gui::LicManager::getInstance().checkInBeta();
	}
	else {
		QString cmd = QString("cf::showMenu 0");
		cf_gui::execTclCmd(cmd);
	}
	*/
}

```
### CcCfStacked.cpp
```cpp
int CcCfStacked::create_stacked_window(ClientData clientData, Tcl_Interp *interp, int objc, Tcl_Obj *CONST objv[])
{
	...
	// 	if (getenv("CF_APP_DVP")) { <- not only use CF_APP_DVP to determine check out beta license
	bool isSiFa = false;
	if (objc >= 9) {
		QString widgetType = Tcl_GetStringFromObj(objv[8], NULL);
	    isSiFa = (widgetType == "SiFa");
	}
	bool betaCheckout = false;
	if ((getenv("CF_APP_DVP") && string(getenv("CF_APP_DVP")) == "1") || isSiFa) {
		betaCheckout = cf_gui::LicManager::getInstance().checkOutBeta();
		if (!betaCheckout) {
			...
			return TCL_OK;
		}
	}
	...
	gui->setBetaCheckoutFlag(betaCheckout); // to let StackedCcWidget know that beta licence is check out
}
```
### StackedCcWidget.cpp
```cpp
StackedCcWidget::StackedCcWidget()
{
	...
	/* remove check out beta here, move to SiFa active
	betaCheckout_ = cf_gui::LicManager::getInstance().checkOutBeta();
	if (betaCheckout_) {
		connect(stackedWidget_, SIGNAL(destroyed(QObject*)),
			&LicManager::getInstance(), SLOT(checkInBeta()));
	}
	*/
	...
}
int StackedCcWidget::init(InitMode mod, const QString &viewFile)
{
	...
	/* not need to invisible for sifa
	if (!betaCheckout_) {
		actionFailure_->setVisible(false);
	}
	*/
}
// new function to set beta license from CcCfStacked
void StackedCcWidget::setBetaCheckoutFlag(bool b)
{
	betaCheckout_ = b;
	if (b) {
	connect(stackedWidget_, SIGNAL(destroyed(QObject*)),
		&LicManager::getInstance(), SLOT(checkInBeta()));
	}
}
void StackedCcWidget::newSiFaActions()
{
	...
	if (!betaCheckout_) { // for select analysis type
		betaCheckout_ = cf_gui::LicManager::getInstance().checkOutBeta();
		if (betaCheckout_) {
			connect(stackedWidget_, SIGNAL(destroyed(QObject*)),
				&LicManager::getInstance(), SLOT(checkInBeta()));
		}
		else {
			return;
		}	
	}
	...
}
```
### StackedCcWidget.h
```cpp
class StackedCcWidget : public QWidget
{
	...
	// new function to set beta license from CcCfStacked
	void setBetaCheckoutFlag(bool b);
	...
}
```
## Test plan
* set "CF_APP_DVP" = 1 and no beta license 
	* [x] can't create CF App widget
* set "CF_APP_DVP" = 0 or no CF_APP_DVP and no beta licese
	* [x] SiFa flow can not used in both PrimeWave and SiFa
* set "CF_APP_DVP" = 0 or no CF_APP_DVP
	* Primewave flow
		* [x] CF App will not check out beta when select Mfg, FuSa 
		* [x] CF App will check out beta when select SiFa 
	* Netlist flow
		* [x] CF App will not check out beta when select Mfg, FuSa 
		* [x] CF App will check out beta when select SiFa 
* set "CF_APP_DVP" = 1
	* PrimeWave flow
		* [x] CF App will checkout beta license when create CF widget
	* Netlist flow
		* [x] CF App will checkout beta license when create CF widget
	* no additional beta license will check out


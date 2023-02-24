---
date : 2022-11-25 08:44
priority : 1
---
# Metadata
Status ::
Type ::
Summary :: 
Topics :: 
# Note
## Nick: wv expr eval
* test case: unit_cf/gui/cc/wv_expr_eval
* solve procedure
	* it's due to dc cover stop simulation
	* 1. modify worker not to kill all process include parent process.
	* 2. in masterflow, when stop simulation or task is killed, then stop fault campaign loop and kill all running tasks
	* 3. in QAfData when in replay mode stop showing any QMessageBox.
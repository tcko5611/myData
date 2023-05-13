---
date : 2023-04-25 14:47
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
# Answer
# Note
## af_read_stderr.cpp
* in what if flow, we will have *af.RecalculateCoverage = true*, and when this happen we will need to use *str_done = "XA done"* for gen_fault is xa
```cpp
int ResultsFromStderr(...) {
// if (IsFsim)
+  if (IsFsim && !afdata_obj.getRecalculateCoverage())
     {
       str_done = "FineSim Successfully Completed at";
     }
// else if (IsPsim)
+  else if (IsPsim && !afdata_obj.getRecalculateCoverage())
     {
       str_done = "PrimeSim Successfully Completed at";
	}
}
```
QAfData.cpp
* when a TB si generate new fault list, then TB must not be a what-if flow TB anymore for it can't use the original TB's result
```cpp
 int QAfData::createFaultList(QString fileName)
 {
+  af.setLastRunDir("");
+  af.setWhatIfTag("");
+  setStringField("last_run_dir", QString());
+  setStringField("what_if_tag", QString());
+ 
	...
}
```
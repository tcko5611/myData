---
date : 2023-03-23 10:27
priority : 1
---
# Metadata
Status :: #Status/Done 
Type :: #CF/GUI 
# Question
Can't load meas file correctly
# Answer
# Note
## code change
* QAfData.cpp
code error, need to change to correct vesrion
```cpp
void QAfData::saveUserDefineMeasFile(const QString &fN)
{
	...
	// old code has use min for the field  
	s += max.empty() ? "" : string(" max=") + max; 
	s += logic.empty() ? "" : string(" logic=") + logic;
	s += stop.empty() ? "" : string(" stop=") + stop;
}
```
## usage problem
* the User Define Meas Table only for user to define their meas/se_sim_stop statments
* other statements must in original netlist like paramter, probe ...
## new problem
* xa/gen_fault has problem for post process waveform, can't find symbol when read waveform.
* send e-mail to Mayukh to find if there someone can help on this.
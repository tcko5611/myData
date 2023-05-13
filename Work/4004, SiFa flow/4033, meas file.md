---
date : 2023-04-04 11:45
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
If meas and extract commands not with set_sim_stop, the load meas not working for cross reference for meas statement.
# Answer
# Note
## QAfData.cpp
```cpp
QString QAfData::setSimCmdLine(const QString &s)
{
	...
	// if (tokens[i] == "-c") {
	if (tokens[i] == "-c" && ((i+1) < tokens.size())) { // add check command is completed or not
	 ...
}
void QAfData::loadUserDefineMeasFile(const QString &fN)
{
	...
	// add sets for set_sim_stop used meas, extract
	std::set<string> measSet, extractSet;
	...
	measSet.insert(name); // set_sim_stop used meas
	...
	extractSet.insert(name); // set_sim_stop used extract
	...
	// add remain meass
	for (auto &s : v["meas"].GetObject()) {
		string name = s.name.GetString();
		if (measSet.find(name) != measSet.end()) continue;
		string check = "meas";
		string expr = s.value["expr"].GetString();
		string min, max, logic, stop;
		Value vals(kArrayType);
		vals.PushBack(Value(name.c_str(), a).Move(), a);
		vals.PushBack(Value(check.c_str(), a).Move(), a);
		vals.PushBack(Value(expr.c_str(), a).Move(), a);
		vals.PushBack(Value(min.c_str(), a).Move(), a);
		vals.PushBack(Value(max.c_str(), a).Move(), a);
		vals.PushBack(Value(logic.c_str(), a).Move(), a);
		vals.PushBack(Value(stop.c_str(), a).Move(), a);
		configDoc_["userDefineMeas"].PushBack(vals, a);
	}
	// add remain extract
	for (auto &s : v["extract"].GetObject()) {
		string name = s.name.GetString();
		if (extractSet.find(name) != extractSet.end()) continue;
		string check = "extract";
		string expr = s.value["expr"].GetString();
		string min, max, logic, stop;
		Value vals(kArrayType);
		vals.PushBack(Value(name.c_str(), a).Move(), a);
		vals.PushBack(Value(check.c_str(), a).Move(), a);
		vals.PushBack(Value(expr.c_str(), a).Move(), a);
		vals.PushBack(Value(min.c_str(), a).Move(), a);
		vals.PushBack(Value(max.c_str(), a).Move(), a);
		vals.PushBack(Value(logic.c_str(), a).Move(), a);
		vals.PushBack(Value(stop.c_str(), a).Move(), a);
		configDoc_["userDefineMeas"].PushBack(vals, a);
	}
}
void QAfData::saveUserDefineMeasFile(const QString &fN)
{
	...
	// if no min, max then for meas, extract there is no set_sim_stop
	if ((type == "extract" || type == "meas") && min.empty() && max.empty()) {
		continue;
	}
}
```

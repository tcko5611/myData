---
date : 2023-04-04 14:14
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
MEAS can't parse in CF GUI
# Answer
# Note
## DiagCommon.cpp
```cpp
// remove additional space for expression
Meas Diag::buildMeas(const string& line)
{
	...
	// d.expr_ = line.substr(line.find(tokens[1]) + tokens[1].length() + 1);
	d.expr_ = boost::algorithm::trim_copy(line.substr(line.find(tokens[1]) + tokens[1].length() + 1));
	...
	// d.expr_ = line.substr(line.find(tokens[2]) + tokens[2].length() + 1);
	d.expr_ = boost::algorithm::trim_copy(line.substr(line.find(tokens[2]) + tokens[2].length() + 1));
	...
}
ExtractCmd Diag::buildExtractCmd(const string& line)
{
	...
	// d.expr_ = line.substr(line.find(tokens[1]) + tokens[1].length() + 1);
	d.expr_ = boost::algorithm::trim_copy(line.substr(line.find(tokens[1]) + tokens[1].length() + 1));
	...
	// d.expr_ = line.substr(line.find(tokens[2]) + tokens[2].length() + 1);
	d.expr_ = boost::algorithm::trim_copy(line.substr(line.find(tokens[2]) + tokens[2].length() + 1));
	...
}
```
## DiagSetting.cpp
```cpp
void DiagSetting::buildSimStops(const std::string &fN)
{
	...
	// find both "meas" or "MEAS"
    // auto const measPos = line.find("meas");
    auto measPos = line.find("meas");
    if (measPos == string::npos) {
      measPos = line.find("MEAS");
    }
    ...
}
```

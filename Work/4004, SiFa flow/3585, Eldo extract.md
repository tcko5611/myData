---
date : 2023-02-14 08:01
priority : 2
---
# Metadata
Status :: #Status/Progress 
Type :: #CF/GUI 
# Question
How to integrate extract into CF App?
# Answer
* It need to use *QAfData* to parse the file and collect extract statement in netlist into meas json data
* In user define meas/extract data, we may need to store the data in a new json field and when run simulation include the user defined meas/extract statements.
# Note
## test case
* case dir: /slowfs/hs_scr1/tcko/customfault/jira/3585_etract
* netlist: netlist
* json file: config.json
## code spec
* extract statement:
```
.EXTRACT tran label=rise_n2 trise(v(N2), vl=0.1, vh= 2.9)
.extract tran label=rise_n2 trise(v(N2), vl=0.1, vh= 2.9)
```
### parse extract statement: In *DiagCommon.cpp* add followinf code
``` cpp
  struct ExtractCmd {
    std::string label_;
    std::string expr_;
  };
ExtractCmd Diag::buildExtractCmd(const string& line)
{
  ExtractCmd d;
  istringstream iss(line);
  vector<string> tokens;
  copy(istream_iterator<string>(iss),
       istream_iterator<string>(),
       back_inserter(tokens));
  if (tokens[1] != "tran") {
    d.label_ = tokens[1].substr(tokens[1].find("=")+1);
    d.label_ = (d.label_[0] == '"') ? d.label_.substr(1) : d.label_;
    if (d.label_.back() == '"') {
      d.label_.pop_back();
    }
    d.expr_ = line.substr(line.find(tokens[1]) + tokens[1].length() + 1);
  }
  else {
    d.label_ = tokens[2].substr(tokens[2].find("=")+1);
    d.label_ = (d.label_[0] == '"') ? d.label_.substr(1) : d.label_;
    if (d.label_.back() == '"') {
      d.label_.pop_back();
    }
  }
  return d;
}
```
### insert extracts into DiagSettings
``` cpp
struct DiagSetting {
...
  std::map<std::string, ExtractCmd> extracts_; // key is label
...
};
void DuiagSetting::buildSimStops {
...
   auto extractPos = line.find("extract");
   extractPos = (extractPos == string::npos) ? line.find("EXTRACT") : extractPos;
   if (extractPos != string::npos && 
       (extractPos == start+1 && line[start] == '.')) {
     ExtractCmd d = buildExtractCmd(line);
     extracts_[d.label_] = d; 
   }
   ...
}
Value  DiagSetting::getSimStopsValue(rapidjson::Document::AllocatorType &a)
{
...
  Value extractCmd(kObjectType);
  for (auto &e : extracts_) {
    auto &m = e.second;
    Value key;
    key.SetString(m.label_.c_str(), a);
    Value vals(kObjectType);
    vals.AddMember(Value("expr",a).Move(), Value(m.expr_.c_str(), a).Move(), a);
    extractCmd.AddMember(key, vals, a);
  }
  v.AddMember("extract", extractCmd, a);
  return v;
}
```
## QAfData collect "Extract" statements
```cpp
void QAfData::appendToSimStopDoc(const std::string &f) {
...
	simStopDoc_.AddMember(Value("extract", a).Move(), Value(kArrayType).Move(), a);
...
  for (auto &m : v["extract"].GetObject()) {
    simStopDoc_["extract"].AddMember(m.name, m.value, a);
  }
}
```
## MfgWidget display "extract" statments
```cpp
void MfgWidget::buildTableExpr(const Document &exprJson)
{
...
    if (expr1.empty() && simStopDoc["extract"].HasMember(v.value["measName"].GetString())) {
      expr1 = simStopDoc["extract"][v.value["measName"].GetString()]["expr"].GetString();
      type->setText("extract");
      if (name->text().isEmpty()) {
        name->setText(v.value["measName"].GetString());
      }
    }
...
}

```
---
date : 2022-05-05 10:17
aliases : []
priority : 5
---
# Metadata
Status :: #Status/Info 
Type :: #CF/Utilites
Topics :: # Metadata
# Note
## Save to file
```cpp
Document d;
StringBuffer buffer;
PrettyWriter < StringBuffer > writer(buffer);
Writer<StringBuffer> writer(buffer);
d.Accept(writer);
std::ofstream outfile;
if (append)
    outfile.open(name, std::ios_base::app);
else
    outfile.open(name);
outfile << buffer.GetString();
```
## Load from file
```c++
Document d
ifstream ifs(filename);
IStreamWrapper iw(ifs);
if (d.ParseStream<kParseCommentsFlag|kParseNanAndInfFlag>(iw).HasParseError()) {
	uint o = (unsigned) dfault.GetErrorOffset();
	if (o < 40) o = 40;
	string content;
	ifstream nfile(file);
	content.assign((istreambuf_iterator<char>(nfile)),
		(istreambuf_iterator<char>()));
    cout << string("JSON parse error\n\nError:") +
	        GetParseError_En(dfault.GetParseError()) +
            "\nMost likely here: \n" + content.substr(o-40, 80) +
             "...");
}
```
## Load from string
```cpp
String line
Document d;
if (d.Parse<kParseCommentsFlag|kParseNanAndInfFlag>(line.c_str()).HasParseError()) {
	...
}
```
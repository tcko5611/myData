---
date : 2023-05-08 10:32
priority : 1
---
# Metadata
Status :: #Status/Progress 
Type :: #CF/GUI 
# Question
How to read existing model files
# Answer
# Note
## solution
User need to provide json file like:
```json
{
	"include_files" : ["file1", "file2"],
...
}
```
and the file contains must like:
```json
{
	"faults" : {
		"models" : [
	
		]
	} 
}
```
---
date : 2022-04-17 22:56
aliases : []
---
# Metadata
Status :: #Status/Info 
Type :: #CPP 
Topics :: # Metadata
# Note
## std::function, std::bind
## Misc
* alias function
```cpp
const auto& new_fn_name = old_fn_name;

int stoi (const string&, size_t*, int);
int stoi (const wstring&, size_t*, int);

const auto& new_fn_name = static_cast<int(*)(const string&, size_t*, int)>(std::stoi);
```
* get absolute path
```c
	#include <limits.h>
    #include <stdlib.h>
    char *realpath(const char *path, char *resolved_path);
```
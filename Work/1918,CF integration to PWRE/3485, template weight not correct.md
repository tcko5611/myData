---
date : 2022-06-28 19:28
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
Topics :: [P1024105-3485](https://jira.internal.synopsys.com/browse/P10241015-3485)
# Note
## Reproduce step
1. idetify fault with "short"
2. template file is "short"
3. idetify fault with "short" and "open"
4. template file is only "short", not correct
## Solution
* The problem comes from QAfData, when we create a template file, we use *QFile::copy* funtion, but it will not overwright when the destination file is exist. we need to use *QFile::remove* to remove the destination file first.
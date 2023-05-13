---
date : 2022-06-28 15:49
aliases : []
priority : 4
---
# Metadata
Status :: #Status/Progress 
Type :: #CF/GUI 
Topics :: # Metadata
# Note
## model path in CC for simulators
| simulator | .opt search | comman line |
| :---------: | :----------:| :------: |
|xa | yes | -I ....|
| finesim | yes | no |
| primesim | no | -inc ..|

## Evergreen Note
Question :: How to implement in CF App

Answer :: 1. create model_path field in faultsim.json 
2. when 
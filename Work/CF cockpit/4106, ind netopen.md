---
date : 2023-02-09 11:06
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/Cockpit 
# Question
The inductor netopen has wrong gweight compared with diode netopen  
# Answer
The problem comes from function *_insert_default_weigt* in file *af_sampling.cpp:300*, miss to check fault type *ind_netopen*
# Note

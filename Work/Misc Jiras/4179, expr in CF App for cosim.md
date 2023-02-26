---
date : 2023-02-26 18:04
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
# Question
in PWDE cosim case, why set_sim_stop statements not in expr table
# Answer
The problem is when parse cosim script, we need to 
* first insert "meas_spec_xa.cfg" into "commnads" for cosim
* then setInitFile will call setCaseDir to parse the set_sim_stop statements. 
# Note

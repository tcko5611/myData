---
date : 2023-03-24 11:01
priority : 1
---
# Metadata
Status :: #Status/Done
Type :: #CF/GUI 
# Question
How to solve problem for 
# Answer
# Note
## case and code dir
case: /slowfs/hs_scr1/tcko/customfault/sae/jira/4159_expr
test code : /remote/tw_rnd1/ktc/prog/cpp/wv_expr
## test procedure for test code
1. test new library is OK or not "/remote/swefs1/PE/products/cktsim/u2023.03_dev/clientstore/cktsim_u2023.03_dev_build/dependent/wvadv/latest/rapi_nolic/linux64/librapi_nolic_o.a"
2. change CMakeLists.txt "WV_ROOT" for include "\${WV_ROOT}/rapi_nolic/include" and lib "\${WV_ROOT}/rapi_nolic/linux64/librapi_nolic_o.a"
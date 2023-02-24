---
date : 2022-07-20 14:39
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Backlog 
Type :: #CF/Cockpit 
Topics :: 
# Note
## Question
Why for nmos with 
inject_fault -inst mnvthtest -param_name vth -del_abs_val -5.000000e-02
will cause vth value to add 50mv
## Answer
In test case "unit_xa/small/models/param_fault/bsim3v3/test4_vth" from model team,
```
netlist:
Vds0 d0 0 0.050
mdut0 d0 g s b  nch_svt_mac W=12.8u L=0.2u
Vds1 d1 0 0.050
mdut1 d1 g s b  nch_svt_mac W=12.8u L=0.2u 
Vds2 d2 0 0.050
mdut2 d2 g s b  nch_svt_mac W=12.8u L=0.2u delvto=-0.1
Vds3 d3 0 -0.050
mdut3 d3 g1 s b  pch_svt_mac W=3.6u L=0.2u
Vds4 d4 0 -0.050
mdut4 d4 g1 s b  pch_svt_mac W=3.6u L=0.2u 
Vds5 d5 0 -0.050
mdut5 d5 g1 s b  pch_svt_mac W=3.6u L=0.2u delvto=0.1

config.tcl:
set_waveform_resolution -i 0.0 -v 0.0 
set_sim_level 7
inject_fault -inst mdut1  -param_name vth -del_val 0.1nmos)
inject_fault -inst mdut4  -param_name vth -del_val 0.1
```
will find mdut1 same with mdut2, mdut4 same with mdut5. so nmos is oppositte between "vth" and "delvto", need to clarify with model team why the behavior is like this?
---
date : 2022-07-12 16:12
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Validate 
Type ::  #CF/GUI 
Topics :: 
# Note
* when CF in netlist flow, set schWindowId = 0 in CcCfFdv
* in cf::openFdv(tcl) when pwWin == 0 (associate with Cf), direct call *cf::fdv::invoke 0(schwin)*
* in cf::fdv::invoke when schWin == 0, direct register FDV belong to which winodw(CF App), don't create relate data  structure related to schematic(use to cross probing) 

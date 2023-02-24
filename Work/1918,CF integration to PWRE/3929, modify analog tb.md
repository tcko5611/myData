---
date : 2022-10-30 22:22
priority : 2
---
# Metadata
Status :: #Status/Validate  
Type :: #CF/GUI 
Summary :: 
Topics :: 
# Note
## Cockpit Flow
### old format
```json
{
	"simulator": "xa",
	"top_netlist" : {
		"schematic" : "test.scs",
		"postlayout" : "postlayout.sp"
	},
	"sim_cmdline" : {
		"prefix" : "-spectre",
		"suffix" : "-c measFile"
	},
	"compilation_cmdline" : {
		"prefix" : "-spectre",
		"suffix" : "-c measFile"
	},
	"cosim" : true,
	"cosim_info" : {
		"init" : "initfile",
		"compilation" : "compilation_cmd",
		"simv_cmdline" : "simv_cmdline",
		"fsdb_check" : {
			"signal_file" : "signal_file",
			"golden_file" : "waveform_file"			
		}
	},	
}
```
### new format
```json
{
	"analog_setup" : {
	    "sim_cmdline" : "xa -spectre test.scs -c measFile",
	    "compilation_cmdline" : "xa -spectre test.scs -c measeFile",
	    "postl_id_cmdline" : "xa -hspice postlayout.sp"
	},
	"cosim_setup" : {
	    "cosim_info" : {
			"init" : "initfile",
			"compilation" : "compilation_cmd",
			"simv_cmdline" : "simv_cmdline",
			"fsdb_check" : {
				"signal_file" : "signal_file",
				"golden_file" : "waveform_file"			
			}
	    },
	    "postl_id" : {
	    	"init" : "initfile",
			"compilation" : "compilation_cmd",
			"simv_cmdline" : "simv_cmdline"
	    }
	}
}
```

1. *af.setPostlayout2021Flag(true)* when has "analog_setup" or "cosim_setup" set
2. ParseNetlistConfig(d, af)
	1.  *_read_analog_setup(d\["analog_setup"\], af)*
		1. *af.setSimCmdLine(as\[sim_cmdline\])*
		2. *af.setSimulatorPath(first word of sim_cmdline)*
		3. from compilation_cmdline or sim_cmdline set *af.setGenFaultSuffix(rest_of_cmd)**
		4. from postl_id_cmdline *afdata_obj.setGenFaultNetFaultSuffix(rest_of_cmd)*
	2.  *_read_cosim_setup(d\["cosim_setup"\], af)*
		1. *\_parse_cosim_fields(d\["cosim_setup"\], af)*, it like *\_parse_cosim_field(d)* if d has member "cosim_info"
		2. *\_parse_postl_cosim_fields(cs, af)*, setting some properties in af, left comp empty will allow use exist simv
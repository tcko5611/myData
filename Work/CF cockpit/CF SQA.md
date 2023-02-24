---
date : 2022-09-02 14:00
priority : 1
---
# Metadata
Status ::
Type ::
Summary :: 
Topics :: 
# Note
## syntax 
```json
{
    "sqa" : {
      "asqa" : { 
	    /*
	      afdata_obj.setSqaFlag(true);
	      afdata_obj.setRunNominalFlag(true);
	      afdata_obj.setRunNominalOnlyFlag(true);
        */
        "devices" : [
            {
                "type" : "mos" // only support mos now, afdata_obj.setSqaMosStatusFlag(true);
            }
        ]
      }, 
      "fdv" : true, // default false, afdata_obj.setSqaFdvFlag(asqa["fdv"].GetBool());
      "sparkline" : true // default true, afdata_obj.setSqaGenSparkline(asqa["sparkline"].GetBool());
      
    },
    "outdir": "tmp",
    "outprefix": "out",
    "dpconfig_file": "hosts_qsub",
    "cpulimit": 300,
    "case_dir": ".",
    "analog_setup": {
       "sim_cmdline": "xa -hspice netlist1.sp -ssl2 -wavefmt fsdb"
    },
    "cleanup_level": 1
}
```
in CF App:
* MfgWidget::on_pushButtonSqa_clicked -> model->runSqa()
* model emit sqaFinished -> MfgWidget->onSqaFinished() -> open FDV
``` mermaid
sequenceDiagram
participant User
participant StackedWidget
participant MfgWidget
participant MfgModel
User ->> StackedWidget : Click on SQA action
StackedWidget ->>+ MfgWidget : on_pushButtonSqa_clicked
MfgWidget ->>+ MfgModel : runSqa()
MfgModel ->>- MfgWidget : onSqaFinished()
MfgWidget ->>- StackedWidget : openFdv()

```
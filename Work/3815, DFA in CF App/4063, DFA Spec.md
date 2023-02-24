---
date : 2022-12-21 15:52
priority : 1
---
# Metadata
Status ::
Type ::
Summary :: 
Topics :: 
# Note
## Write letter to Huiping about the flow
I find the flow of DFA is like:
1.  Generate fault list from config json file.
2.  Set af. setDetectabilityFlag(true)
3.  Call CheckOrRunDetectability(afdata_obj, flistd), then CF will write DFA result into flistd. The af.json need contain correct “detectability” fields.
4.  In af_generate_report.cpp use “\_dfa_report” to generate dfa report.

Is this the flow for DFA analysis in CF?

In CF App, we can keep DFA flistd in memory and control  write to file when needed. Is the above procedure enough to do DFA analysis?

We plan to have the following steps to build DFA analysis in CF App

1.  Generate flistd as normal CF.
2.  Provide a dialog for user  to set their “detectability” fields.
3.  Write the contains of dialog into af.json Document for DFA use.
4.  Call CheckOrRunDetectability to generate DFA result into flistd.
5.  Provide a function to wrapper \_dfa__report to generate DFA report.
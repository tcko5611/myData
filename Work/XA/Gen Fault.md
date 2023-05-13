---
date : 2023-05-06 17:16
priority : 1
---
# Metadata
Status ::
Type ::
# Question
# Answer
# Note
## Flex and Bison
* fdsop_l.l, fdsop_y.y
* add token in l, add syntax in y
* using \_report_faults to store commands contain
* using sepeartor to store options in command
* commands are store *XA_FAULT_DEVICE*, *XA_FAULT_SUBCKT* and *XA_FAULT_NET2NET*
* scope are store in *XA_FAULT_INST*, *XA_FAULT_EXCEPT_INST*, *XA_FAULT_MACRO*
* filter are use *XA_FAULT_NET2NET_PAIR*, *XA_FAULT_EXCEPT_NET2NET_PAIR*
## Dns fuction
* dnsNOMATCH : no found
* dnsInsIndex(char \*insName)
	* return ins index
* dnsSTRING(name) 
	* create local name string to use
* dnsBuildSigName(id, signame)
	* build name from node id to signame
* dnsBuildInsName(id, insname)
	*  build name from inst id to insname (include subinst)
* dnsBuildDefName(id, subdefname)
	*  build name from subdef id to subdefname
* dnsSigIndex(sigName)
	* return sig index
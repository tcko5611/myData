---
date : 2023-03-10 08:29
priority : 1
---
# Metadata
Status :: #Status/Validate 
Type :: #CF/Cockpit 
# Question
CF will error out with
```json
{
    "create_fault_files": true ,
    "what_if" : { 
        "change_weight_only" : true
    },
    "identify_only": true,    
}
```
# Answer
* "change_weight_only" and "identify_only" can't be true simutaneously
* "create_fault_files" values
	* if "change_weight_only" = true 
		* then "create_fault_files" = false
	* else if "identify_only" = true
		* then user can set  "create_fault_files"
	* else
		* then "create_fault_files" = true
# Note
## code
### af_parse_config.cpp
* \_parse_combined_config_json
```cpp
  if (afdata_obj.getIdOnlyFlag() || afdata_obj.getChangeWeightOnlyFlag())
  {
    sprintf(buffer, "create_fault_files");
    if(afdata_obj.getIdOnlyFlag() && d.HasMember(buffer)) // if "identify_only" = true, then user can set  "create_fault_files" 
    {
      afdata_obj.afassert(d[buffer].IsBool(),
          "Field create_fault_files should be of type boolean");
      afdata_obj.setCreateFaultFilesFlag(d[buffer].GetBool());
    }
    else                        // if "change_weight_only" = true, then "create_fault_files" = false
      afdata_obj.setCreateFaultFilesFlag(false);
  }
  else // "create_fault_files" = true
  {
    // if we  are doing  fault campaign, then  does not  matter what
    // user wants, we need to create all fault files:
    afdata_obj.setCreateFaultFilesFlag(true);
  }
```
* \_check_config_consistency
```cpp
// "change_weight_only" and "identify_only" can't be true simutaneously
  if (afdata_obj.getChangeWeightOnlyFlag() && afdata_obj.getIdOnlyFlag()) {
    logError() << "Don't set \"change_weight_only\" and \"identify_only\" true simutaneously.\n";
    exit(-1);
  }
```
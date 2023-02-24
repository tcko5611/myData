1. In UpdateOrCheckMetaData(af_metadata.cpp) add/update flistd\["metadata"\]\["recalculate_tag"\] with afdata_oobj.GetRunIdTag.
2. In TotalSimCompleted(af_genertae_report.cpp), change tagname =  flistd\["metadata"\]\["recalculate_tag"\] if in recalculate_coverage state.
3. For rapidjson,
	1. if we want to remove a member("r") in object (d\["m"\]), d\["m"\].RemoveMember("r")
	2. if we want to change a member value("r") in object(d\["m"\]),  d\["m"\]\["r"\].SetString(str, d.GetAllocator())
	3. if we want to add a member with key(r), value(v) to object(d\["m"\]),  d\["m"\].AddMember(r, v, d.GetAllocator()), **if string is literal, we can direct use it, otherwise it more safe to use SetString with Allocator if you don't ensure the string has a safe life-cycle.**
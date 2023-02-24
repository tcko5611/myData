1. add new option "recalculate_simulator" : "gen_fault" to specified recalculate simulator-> afdata
2. if recalculate simulator is gen_fault, afdata need to set simulator as xa
3. in _create_csh_file(af_print_config.cpp) when recalculate simulator is gen_fault change run_cmd simulator to "gen_fault" and option "-fault_inject"(finesm/primesim) to -c(xa/gen_fault)
4. use "gen_fault" as default recalculate simulator, afdata_obj.geRecalculateSimulator()
5. cpp string can use *substr, find, replace* to manipulate string.
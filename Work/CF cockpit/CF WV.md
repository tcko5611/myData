w ## customfault --wvcreate
* commnad customfault --wvcreate 1 -w \<wavefile name\> -e \<input snps.sim\> -p \<outputprefix\>
* map file : dirname(\<input snps.sim\>)/.snps.map
* output: \<output\>.snps.sim.result
### test case
* location : /slowfs/hs_scr1/tcko/customfault/sae/jira/3443_wv/case2
* test snps.sim : test.snps.sim
* test command : customfault --wvcreate 1 - w test.fsdb -e test.snps.sim -p ktc
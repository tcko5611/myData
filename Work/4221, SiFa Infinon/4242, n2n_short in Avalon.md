---
date : 2023-05-04 08:54
priority : 1
---
# Metadata
Status ::
Type ::
# Question
The Jira need to solve net2net_short problem for avalon_file
# Answer
# Note
## basic solution
* In CF create a new command option "-avalon_node nodes" for gen_fault(xa)
	* nodes is comes from parse avalon_file
* In xa 
	* parse the option "-avalon_node"
	* generate a hash table to store the correct node(id)
	* when net pairs don't match the node, exclude the net pair
* af_model_selection.cpp
```cpp
static string _parse_avalon_node(const string &avalon_inst)
{
  ifstream afile(avalon_inst);
  string line;
  string node_str = " -avalon_node";
  int node_cnt = 0;
  while (getline(afile, line)) {
    if (line.empty() || line.substr(0,4) != "LVS ")
      continue;
    string node =  line.substr(4);
    if (node.at(0) == '/') {
      node.erase(0, 1);
    }
    std::replace(node.begin(), node.end(), '/', '.');
    node_str = node_str + " " + node;
    ++node_cnt;
  }
  if (node_cnt == 0)
    return "";

  return node_str;
}
static string _get_scope(const Value& afm)
{
...
   string avalon_inst_str;
   if (afm.HasMember("avalon_inst"))
   {
-    avalon_inst_str = _parse_avalon_inst(afm["avalon_inst"].GetString());
+    if (afm.HasMember("fault_model") && afm["fault_model"].Size() == 1 && afm["fault_model"][0] == "net_to_net_short") {
+      avalon_inst_str = _parse_avalon_node(afm["avalon_inst"].GetString());
+    }
+    else {
+      avalon_inst_str = _parse_avalon_inst(afm["avalon_inst"].GetString());
+    }
     // cout << "avalon_inst_str = '" << avalon_inst_str << "'" << endl;
     return avalon_inst_str;
...
}
```
* fdsop_l.l, fdsop_y.y
```cpp

+-avalon_node     { return AVALON_NODE; } // flex

+    tok_list_t *avalon_node; // for n2n_short use
+%token AVALON_NODE
// add in fault_net2net_spec:
+| AVALON_NODE token_list      { _report_faults.avalon_node = $2; }
// add in _process_net2net_faults
+  n += sprintf(fault + n, "\t,!");  /* avalon_node */
+      for (q = _report_faults.avalon_node; q; q = q->next)
+        n += sprintf(fault + n, "%s ", q->name);
   fdso_set_opt_string("XA_FAULT_NET2NET", fault);
```
* fdi_fault.c
```cpp
// add avalon_node_table_
+/** avalon node table */
+static hashHandle_t *avalon_node_table_ = 0;
+
+static void _init_avalon_node_table(void)
+{
+  avalon_node_table_ =
+    hashInitHandle(20, (hash_cmp_t *) _cmp_wire, (hash_value_t *) _hash_wire, (hash_save_t *) _save_wire);
+}
+
+static void _close_avalon_node_table(void)
+{
+  hashFreeHandle(avalon_node_table_);
+  avalon_node_table_ = 0;
+}
+
+static void addAvalonNodeToTable_(char *avalonNodes)
+{
+  char *node, *l;
+  while ((node = ut_strtok_r(avalonNodes, " ", &l)))
+    {
+      avalonNodes = 0;
+      UINT64_t idx = dnsInsIndex(node);
+      if (idx == dnsNOMATCH)
+	{
+	  while ((node = strchr(node, '.')))
+	    {
+	      node = node + 1;
+	      idx = dnsSigIndex(node);
+	      if (idx != dnsNOMATCH)
+		{
+		  wire_t w;
+		  w.idx = idx;
+		  hashFindMatchOrInsert(avalon_node_table_, &w);
+		  break;
+		}
+	    }
+	}
+      else
+	{
+	  wire_t w;
+	  w.idx = idx;
+	  hashFindMatchOrInsert(avalon_node_table_, &w);
+	}
+    }
+}
+
void fdiProcessFault(fdi_def_t * def)
{
...
      for (const char *opt = optFirstString("XA_FAULT_NET2NET"); opt; opt = optNextString())
	{
	...
	+	  char *avalon_node = 0;
 	  fdParseFaultCmd(opt_name, &dev_type, &ftype, &node, &num_nodes, &tag, &Rval, &Cval, 0, 0, 0, 0, 0,
-			  &fault_subckt, 0, 0, 0, 0, 0, 0, _fault_mem);
+			  &fault_subckt, 0, 0, 0, 0, 0, &avalon_node, _fault_mem);
+	  // add for avalon_inst
+	  if (avalon_node && (avalon_node_table_ == 0))
+	    {
+	      _init_avalon_node_table();
+	      addAvalonNodeToTable_(avalon_node);
+	    }
+      if (avalon_node_table_)
+	_close_avalon_node_table();
+
...
}

static UINT32_t _dump_net_to_net_short_fault(...)
{
...
+      if (avalon_node_table_)
+	{
+	  wire_t ww0;
+	  utZero(&ww0, 1);
+	  ww0.idx = match_w0->node_name_idx;
+	  wire_t ww1;
+	  utZero(&ww1, 1);
+	  ww1.idx = match_w1->node_name_idx;
+	  if (!hashFindMatch(avalon_node_table_, &ww0) && !hashFindMatch(avalon_node_table_, &ww1))
+	    return dumped;
+	}
...
}
```
ttt## types
```tcl
# simple
set myVariable 18
puts $myVariable
puts [expr $myVariable + 6 + 9]
# list
set myVariable {hello world}
puts [lindex $myVariable 2]
# associate array
set  marks(english) 80
puts $marks(english)
set  marks(mathematics) 90
puts $marks(mathematics)
# handles
set myfile [open "filename" r]
```
## dictionary
``` tcl
# create
set colours [dict create colour1 "black" colour2 "white"]
puts $colours
# size
set colours [dict create colour1 "black" colour2 "white"]
puts [dict size $colours]
# iteration
set colours [dict create colour1 "black" colour2 "white"]
foreach item [dict keys $colours] {
   set value [dict get $colours $item]
   puts $value
}
# get value
set colours [dict create colour1 "black" colour2 "white"]
set value [dict get $colours colour1]
puts $value
# all keys
[dict keys $dictname]
# all values
set colours [dict create colour1 "black" colour2 "white"]
set values [dict values $colours]
puts $values
# key in dict
set colours [dict create colour1 "black" colour2 "white"]
set result [dict exists $colours colour1]
puts $result
```
## json
``` tcl
set jsonStr { \
  { "photos": { "page": 1, "pages": "726", "perpage": 3, "total": "7257", 
    "photo": [
      { "id": "6974156079", "owner": "74957296@N08", "secret": "005d743f82", "server": "7197", "farm": 8, "title": "Kenya Watamu \"Deep Sea Fishing\" \"Indian Ocean\" \"Blue Marlin\"", "ispublic": 1, "isfriend": 0, "isfamily": 0 },
      { "id": "6822988100", "owner": "52857411@N08", "secret": "56630c18e8", "server": "7183", "farm": 8, "title": "Gedi Ruins, Local Guide", "ispublic": 1, "isfriend": 0, "isfamily": 0 },
      { "id": "6822909640", "owner": "52857411@N08", "secret": "f4e392ea36", "server": "7063", "farm": 8, "title": "Local Fisherman, Mida Creek", "ispublic": 1, "isfriend": 0, "isfamily": 0 }
    ] }, "stat": "ok" }
}

set d1 [json::json2dict $jsonStr]
set d2 [json::json2prettydict $jsonStr]
proc dict2json {dictToEncode} {
    ::json::write object {*}[dict map {k v} $dictToEncode {
        set v [::json::write string $v]
    }]
}
```
## args, argv
```
#! /usr/bin/env tclsh
if {$argc > 0} {
  puts "$argv0"
  puts "The first argument passed was [lindex $argv 0]"
  set s [lindex $argv 0]
  set a [string range $s 0 [expr [string last "/" $s] - 1] ]
  puts $a
}
```
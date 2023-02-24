# XA
cd $\<client\>/csim/src/code
synmake install
synmake install-debug
cd $\<client\>/csim/src/code/dki_cosim
synmake shlib-1
synmake shlib-debug-1
# CF
synmake -k -j XA_RD_ssh TARGET_ARCH=linux64 P4ID=12345 -echo install-debug
synmake -k -j XA_RD_ssh TARGET_ARCH=linux64 P4ID=12345 -echo install
# CCK
synmake install-cck
synmake install-cck-debug
# Master files
* $(LOCAL_OBJ):  $(REFROOT)/object_root
* for share library use
```make
MODULE = cfobject
LIB = cfobject
SHLIB = cfobject
SHLIB_LIBS = -Wl,--version-script=$(ROOT)/customfault/src/cc_object/lib.ver $(CDPL_LIB_$(BUILD)) $(SBL_LIB_$(BUILD)) $(LIBENCKEY_$(BUILD)) $(SCL_$(BUILD)) $(WDF_OPT)
```
to build share library
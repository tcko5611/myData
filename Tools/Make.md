# make

# Basic idea

1.  Create object file in different directory ($(OBJROOT)) from source dir ($(ROOT)) to avoid dirty the source tree.
2.  The generate library ($(OBJROOT)/lib) and binary ($(OBJROOT)/bin) files are in the directory we specify.
3.  Auto generate the dependency files and include it.
4.  Most work should be done in global include file "make.incl" and "make.incl2".

# Detail of global "make.incl" and "make.incl2"

## "make.incl" :

1.  define $(ROOT), $(OBJROOT), $(BIN_DIR) and $(LIB_DIR) variables:

```makefile
ROOT := $(strip $(patsubst %/make.incl, %,$(word $(words $(MAKEFILE_LIST)), $(MAKEFILE_LIST))))
OBJROOT := $(ROOT)/object_root
BIN_DIR := ${OBJROOT}/bin
LIB_DIR := ${OBJROOT}/lib

```

1.  set compile and shell related commands:

```makefile
CC := /depot/qsc/QSCO/bin/gcc
CXX := /depot/qsc/QSCO/bin/g++
AR := /bin/ar
ARFLAGS := -cvr
MKDIR := mkdir -p
MV := mv -f
RM := rm -f
SED := sed
TEST := test

```

1.  set QT related variables

```make
QT_LIBS := -lQtNetwork -lQtGui -lQtCore -lX11 -lSM -lICE -lfontconfig \\\\
        -lfreetype -lXext -lXrender -lpng -ljpeg -lmng -ltiff
QT_ROOT := /global/libs/qt_qsck_2015.06/4.8.5/linux64_optimized
QT_PATH := $(QT_ROOT)/bin/
QT_MOCSOURCE = $(addprefix moc_, $(addsuffix .cpp, $(basename $(QT_MOCFILE))))
QT_RCCSOURCE = $(addprefix qrc_, $(addsuffix .cpp, $(basename $(QT_RCCFILE))))
QT_UICSOURCE = $(addprefix ui_, $(addsuffix .h, $(basename $(QT_UICFILE))))

```

1.  set OBJECT_FILE and DEP_FILES(recursive variables) for each Makefile to use:

```makefile
OBJECT_FILE = $(addprefix $(OBJECTDIR)/, $(addsuffix .o, $(basename $(SOURCE_FILE))))
DEP_FILES = $(addprefix $(OBJECTDIR)/, $(addsuffix .dep, $(basename $(SOURCE_FILE))))

```

1.  set global compile related flags

```makefile
CFLAGS := -I. -I$(ROOT)/lmserver/src/include -I$(QT_ROOT)/include/QtCore \\\\
-I$(QT_ROOT)/include/QtNetwork -I$(QT_ROOT)/include/QtGui 
CXXFLAGS := -I. -I$(ROOT)/lmserver/src/include -I$(QT_ROOT)/include/QtCore \\\\
-I$(QT_ROOT)/include/QtNetwork -I$(QT_ROOT)/include/QtGui 
LDFLAGS :=  -static-libgcc /depot/qsc/QSCO/GCC/lib64/libstdc++.a -static-libstdc++ \\\\
-L$(LIB_DIR) -L/global/libs/qt_qsck_2015.06/4.8.5/linux64_optimized/lib
LIBS := $(QT_LIBS) -pthread -ldl -lz

```

## "make.incl2" :

1.  Include the depdency files, should be here for DEP_FILES needed after SOURCE_FILE to define in the Makefile

```makefile
include $(DEP_FILES)

```

1.  define all, create_dirs and clean target. if the Makefile has defined the variable, then it will execute, otherwise there is no effect on it)

```makefile
all: create_dirs $(EXE) $(MODULE)

$(EXE): $(QT_UICSOURCE) $(OBJECT_FILE) 
	$(CXX) -o $(BIN_DIR)/$@ $(OBJECT_FILE) $(LDFLAGS) $(LOCAL_LIBS) $(LIBS) 

$(MODULE): $(QT_UICSOURCE) $(OBJECT_FILE)
	$(AR) -cvq $(LIB_DIR)/lib$@.a $(OBJECT_FILE)
create_dirs:
	$(TEST) -d $(BIN_DIR) || $(MKDIR) $(BIN_DIR)
	$(TEST) -d $(LIB_DIR) || $(MKDIR) $(LIB_DIR)
	$(TEST) -d $(OBJECTDIR) || $(MKDIR) $(OBJECTDIR)
clean:
	$(RM) $(OBJECT_FILE) $(TARGET) $(QT_MOCSOURCE) $(QT_RCCSOURCE) $(QT_UICSOURCE)
	if [ x$(EXE) != "x" ] ; then  $(RM) $(BIN_DIR)/$(EXE); fi
	if [ x$(MODULE) != "x" ];  then $(RM) $(LIB_DIR)/lib$(MODULE).a; fi

```

1.  define file rules, include QT and dependency rules:

```makefile
$(OBJECTDIR)/%.o: %.cpp
	$(CXX) -c $< -o $@ $(CXXFLAGS) 
moc_%.cpp: %.h 
	$(QT_PATH)/moc $(DEFINES) $(INCLUDE) $< -o $@
qrc_%.cpp: %.qrc
	$(QT_PATH)/rcc $< -o $@
ui_%.h: %.ui
	$(QT_PATH)/uic $< -o $@
$(OBJECTDIR)/%.dep: %.cpp
	@echo generate $@
	$(CXX) -M $(CXXFLAGS) $< > $@.$$$$; \\\\
	sed 's,\\\\($*\\\\).o[ :]*,$(OBJECTDIR)/\\\\1.o $@ : ,g' <  $@.$$$$ > $@; \\\\
	$(RM) $@.$$$$

```

# Local Makfile

1.  Use a shell script to get the "make.incl" file and include it.

```makefile
INCL_FILE := $(shell path="."; \\\\
               ret=""; \\\\
              while [[ $$path != / ]]; \\\\
              do \\\\
                ret="$$(find $$path -maxdepth 1 -mindepth 1 -iname "make.incl")"; \\\\
                if [ x"$$ret" != x"" ]; then \\\\
                  break; \\\\
                fi; \\\\
                path="$$(readlink -f "$$path"/..)"; \\\\
              done; \\\\
              echo $$ret)
include $(INCL_FILE)

```

1.  define related object directory variable

```makefile
OBJECTDIR:=$(patsubst $(ROOT)/%, $(OBJROOT)/%, $(CURDIR))

```

1.  Define the QT use files variables and SOURCE files:

```makefile
QT_MOCFILE = TasksManager.h MainWindow.h
QT_RCCFILE =
QT_UICFILE = mainwindow.ui 	 
SOURCE_FILE = main.cpp LicManager.cpp TasksManager.cpp MainWindow.cpp \\\\
$(QT_MOCSOURCE) $(QT_RCCSOURCE)

```

1.  Define Target EXE: for executable, MODULE: for library and local compile related flags.

```makefile
EXE = customfault
LOCAL_LIBS = -lscl -lutils

```

1.  include "make.incl2"

```makefile
include $(ROOT)/make.incl2

```

# sample files

make.incl

```makefile
# -*-Makefile-*-
# setting ROOT and OBJROOT
#
ROOT := $(strip $(patsubst %/make.incl, %,$(word $(words $(MAKEFILE_LIST)), $(MAKEFILE_LIST))))
OBJROOT := $(ROOT)/object_root

#
# set bin and lib output directories
#
BIN_DIR := ${OBJROOT}/bin
LIB_DIR := ${OBJROOT}/lib

#
#  set compiler and related shell commands
#
CC := /depot/qsc/QSCO/bin/gcc
CXX := /depot/qsc/QSCO/bin/g++
AR := /bin/ar
ARFLAGS := -cvr
MKDIR := mkdir -p
MV := mv -f
RM := rm -f
SED := sed
TEST := test

#
# Qt special function
#
QT_LIBS := -lQtNetwork -lQtGui -lQtCore -lX11 -lSM -lICE -lfontconfig \\\\
        -lfreetype -lXext -lXrender -lpng -ljpeg -lmng -ltiff
QT_ROOT := /global/libs/qt_qsck_2015.06/4.8.5/linux64_optimized
QT_PATH := $(QT_ROOT)/bin/
QT_MOCSOURCE = $(addprefix moc_, $(addsuffix .cpp, $(basename $(QT_MOCFILE))))
QT_RCCSOURCE = $(addprefix qrc_, $(addsuffix .cpp, $(basename $(QT_RCCFILE))))
QT_UICSOURCE = $(addprefix ui_, $(addsuffix .h, $(basename $(QT_UICFILE))))
OBJECT_FILE = $(addprefix $(OBJECTDIR)/, $(addsuffix .o, $(basename $(SOURCE_FILE))))
DEP_FILES = $(addprefix $(OBJECTDIR)/, $(addsuffix .dep, $(basename $(SOURCE_FILE))))
#
# global compile related flags
#
CFLAGS := -I. -I$(ROOT)/lmserver/src/include -I$(QT_ROOT)/include/QtCore -I$(QT_ROOT)/include/QtNetwork -I$(QT_ROOT)/include/QtGui 
CXXFLAGS := -I. -I$(ROOT)/lmserver/src/include -I$(QT_ROOT)/include/QtCore -I$(QT_ROOT)/include/QtNetwork -I$(QT_ROOT)/include/QtGui 
LDFLAGS :=  -static-libgcc /depot/qsc/QSCO/GCC/lib64/libstdc++.a -static-libstdc++ -L$(LIB_DIR) -L/global/libs/qt_qsck_2015.06/4.8.5/linux64_optimized/lib
LIBS := $(QT_LIBS) -pthread -ldl -lz

```

"make.incl2"

```makefile
include $(DEP_FILES)

all: create_dirs $(EXE) $(MODULE)

$(EXE): $(QT_UICSOURCE) $(OBJECT_FILE) 
	$(CXX) -o $(BIN_DIR)/$@ $(OBJECT_FILE) $(LDFLAGS) $(LOCAL_LIBS) $(LIBS) 

$(MODULE): $(QT_UICSOURCE) $(OBJECT_FILE)
	$(AR) -cvq $(LIB_DIR)/lib$@.a $(OBJECT_FILE)

create_dirs:
	$(TEST) -d $(BIN_DIR) || $(MKDIR) $(BIN_DIR)
	$(TEST) -d $(LIB_DIR) || $(MKDIR) $(LIB_DIR)
	$(TEST) -d $(OBJECTDIR) || $(MKDIR) $(OBJECTDIR)

clean:
	$(RM) $(OBJECT_FILE) $(QT_MOCSOURCE) $(QT_RCCSOURCE) $(QT_UICSOURCE) $(DEP_FILES)
	if [ x$(EXE) != "x" ] ; then  $(RM) $(BIN_DIR)/$(EXE); fi
	if [ x$(MODULE) != "x" ];  then $(RM) $(LIB_DIR)/lib$(MODULE).a; fi
 
# $(OBJECTDIR)/%.o: %.c
#	$(CC) -c $< -o $@ $(CFLAGS) 

$(OBJECTDIR)/%.o: %.cpp
	$(CXX) -c $< -o $@ $(CXXFLAGS) 

moc_%.cpp: %.h 
	$(QT_PATH)/moc $(DEFINES) $(INCLUDE) $< -o $@

qrc_%.cpp: %.qrc
	$(QT_PATH)/rcc $< -o $@

ui_%.h: %.ui
	$(QT_PATH)/uic $< -o $@

$(OBJECTDIR)/%.dep: %.cpp
	@echo generate $@
	$(CXX) -M $(CXXFLAGS) $< > $@.$$$$; \\\\
	sed 's,\\\\($*\\\\).o[ :]*,$(OBJECTDIR)/\\\\1.o $@ : ,g' <  $@.$$$$ > $@; \\\\
	$(RM) $@.$$$$

```

Root Makefile

```makefile
SUBDIRS := utils IsAmd SclApis LmServer CustomSim CustomFault

.PHONY: all clean $(SUBDIRS)

all clean: $(SUBDIRS)

$(SUBDIRS):
	$(MAKE) -C $@ $(MAKECMDGOALS)

```

Subdir Makefile

```makefile
INCL_FILE := $(shell path="."; \\\\
               ret=""; \\\\
              while [[ $$path != / ]]; \\\\
              do \\\\
                ret="$$(find $$path -maxdepth 1 -mindepth 1 -iname "make.incl")"; \\\\
                if [ x"$$ret" != x"" ]; then \\\\
                  break; \\\\
                fi; \\\\
                path="$$(readlink -f "$$path"/..)"; \\\\
              done; \\\\
              echo $$ret)

include $(INCL_FILE)

OBJECTDIR:=$(patsubst $(ROOT)/%, $(OBJROOT)/%, $(CURDIR))

QT_MOCFILE = TasksManager.h MainWindow.h
QT_RCCFILE =
QT_UICFILE = mainwindow.ui 

EXE = customfault
SOURCE_FILE = main.cpp LicManager.cpp TasksManager.cpp MainWindow.cpp $(QT_MOCSOURCE) $(QT_RCCSOURCE)
LOCAL_LIBS = -lscl -lutils

include $(ROOT)/make.incl2

```
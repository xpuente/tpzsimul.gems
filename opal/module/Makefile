#   Portions Copyright (c) 2000 by Virtutech AB.
#
#   Permission to use, copy, modify, and distribute this software
#   for any purpose with or without fee is hereby granted, provided
#   that the above copyright notice and this permission notice appear
#   in all copies, and that the name of Virtutech AB not be used in
#   advertising or publicity pertaining to distribution of the document
#   or software without specific, written prior permission.
#
#   THE SOFTWARE IS PROVIDED "AS IS" AND VIRTUTECH AB DISCLAIMS ALL
#   WARRANTIES WITH REGARD TO THIS SOFTWARE, INCLUDING ALL IMPLIED
#   WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL 
#   VIRTUTECH AB BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR
#   CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM 
#   LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT,
#   NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN
#   CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

# For Simics 2.X
#GEMS_ROOT = $(SIMICS_BASE)/../

# For Simics 3.0
GEMS_ROOT = $(SRC_BASE)/../../

include $(GEMS_ROOT)/common/Makefile.simics_version

MODULE_DIR = opal
MODULE_NAME = opal
FILENAME = opal

MODULE_CFLAGS += -DTARGET_VA_BITS=64 -DTARGET_PA_BITS=64
MODULE_LDFLAGS += -ggdb -g3

ifeq ($(SIMICS_VERSION),2.2.X)
  MODULE_CFLAGS += -DSIMICS22X
  SIMICS_API = 2.0
  # NOTE: Don't place .a files in EXTRA_OBJ_FILES
  EXTRA_OBJ_FILES = $(shell ls -1 $(SIMICS_BASE)/../opal/$(HOST_TYPE)/obj/*.o | grep -v simdist12.o | grep -v memscan.o |\
				   	grep -v makeipage.o | grep -v readipage.o | grep -v usd.o | grep -v regtest.o | grep -v simmain.o | \
					grep -v bp.o | grep -v ptracetest.o | tr '\n' ' ')
  CALC_HOST = $(SIMICS_BASE)/../scripts/calc_host.sh
else
ifeq ($(SIMICS_VERSION),3.0)
  MODULE_CFLAGS += -DADD_ARCH_TO_API -DSIMICS30
  SIMICS_API = 3.0
  EXTRA_OBJ_FILES = $(shell ls -1 $(SRC_BASE)/../../opal/$(HOST_TYPE)/obj/*.o | grep -v simdist12.o | grep -v memscan.o |\
				   	grep -v makeipage.o | grep -v readipage.o | grep -v usd.o | grep -v regtest.o | grep -v simmain.o | \
					grep -v bp.o | grep -v ptracetest.o | tr '\n' ' ')
  CALC_HOST = $(SRC_BASE)/../../scripts/calc_host.sh
else
	@echo "Unrecognized value for SIMICS_VERSION: see commmon/Makefile.common"
  MODULE_CFLAGS += -qvcstopcompile
endif
endif

 # add -m64 and library paths if needed
CC_VERSION=3.4.4
HOST_TYPE = $(shell $(CALC_HOST))
ifeq ($(HOST_TYPE),x86-linux)
  MODULE_LDFLAGS += -m32
  # if using a non-native compiler, specify runtime paths to libstdc++ and other system libraries
  # MODULE_LDFLAGS += -Wl,-R/s/gcc-$(CC_VERSION)/lib
else
ifeq ($(HOST_TYPE),amd64-linux)
  MODULE_LDFLAGS += -m64
  MODULE_LDFLAGS += -Wl,-R/s/gcc-$(CC_VERSION)/lib64
else
ifeq ($(HOST_TYPE),v9-sol8-64)
  MODULE_LDFLAGS += -m64
  MODULE_LDFLAGS += -Wl,-R/s/gcc-$(CC_VERSION)/lib/sparcv9
else
  echo "WARNING: illegal host type!"
endif
endif
endif

# Optioally, you can specify which library path being written into the object
# Normally, you don't need this
#MODULE_LDFLAGS += -Wl,-R/your/lib/path

# Control whether we link in gcc and stdc++ libraries or not.
# In fact, simics Makefile has had a "-shared" in front of us, so we are
# overwritting their linking parameters.
#MODULE_LDFLAGS += -Wl,-Bstatic -lstdc++ -static-libgcc
MODULE_LDFLAGS += -Wl,-Bdynamic -lstdc++ -shared-libgcc

# For SUNWspro compiler
#MODULE_LDFLAGS = -staticlib=Cstd

LDFLAGS_DYN = 

SRC_FILES = opal.c

MODULE_ARCHS = sun4u

MODULE_CLASSES = opal

ifeq ($(SIMICS_VERSION),2.2.X)
  INCLUDE_FILENAME := $(SIMICS_BASE)/src/devices/common/device-makefile
  EXTRA_VPATH = $(SIMICS_BASE)/../opal:$(SIMICS_BASE)/../opal/common:$(SIMICS_BASE)/../opal/system:$(SIMICS_BASE)/../opal/benchmark/tester:$(SIMICS_BASE)/$(HOST_TYPE)/obj/include/simics
else
ifeq ($(SIMICS_VERSION),3.0)
  INCLUDE_FILENAME := $(MODULE_MAKEFILE)
  EXTRA_VPATH = $(SRC_BASE)/../../opal:$(SRC_BASE)/../../opal/common:$(SRC_BASE)/../../opal/system:$(SRC_BASE)/../../opal/benchmark/tester:$(SRC_BASE)/include/simics
else
  INCLUDE_FILENAME := stopcompile
endif
endif

include $(INCLUDE_FILENAME)



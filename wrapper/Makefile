PLATFORM ?= $(shell uname -s)
OPTIMIZE ?= 2
LTO ?= 0

OSXCROSS_VERSION ?= unknown
OSXCROSS_TARGET ?= darwin12
OSXCROSS_OSX_VERSION_MIN ?= 10.5
OSXCROSS_LINKER_VERSION ?= 134.9
OSXCROSS_LIBLTO_PATH ?=
OSXCROSS_CXXFLAGS ?= 

override CXXFLAGS=-std=c++0x -Wall -Wextra -pedantic -I. -O$(OPTIMIZE) -g

ifeq ($(LTO), 1)
  override CXXFLAGS+=-flto
endif

override CXXFLAGS+=-DOSXCROSS_VERSION="\"$(OSXCROSS_VERSION)\""
override CXXFLAGS+=-DOSXCROSS_TARGET="\"$(OSXCROSS_TARGET)\""
override CXXFLAGS+=-DOSXCROSS_OSX_VERSION_MIN="\"$(OSXCROSS_OSX_VERSION_MIN)\""
override CXXFLAGS+=-DOSXCROSS_LINKER_VERSION="\"$(OSXCROSS_LINKER_VERSION)\""
override CXXFLAGS+=-DOSXCROSS_LIBLTO_PATH="\"$(OSXCROSS_LIBLTO_PATH)\""

override CXXFLAGS+= $(OSXCROSS_CXXFLAGS)

ifneq (,$(findstring FreeBSD, $(PLATFORM)))
  override LDFLAGS+=-lutil
else ifneq (,$(findstring Darwin, $(PLATFORM)))
  override CXXFLAGS+=-Wno-deprecated
  override LDFLAGS+=-framework CoreServices -Wno-deprecated
else ifeq (,$(findstring Windows, $(PLATFORM)))
  override LDFLAGS+=-lrt
endif

BIN=wrapper

SRCS= \
 main.cpp \
 tools.cpp \
 target.cpp \
 programs/osxcross-version.cpp \
 programs/osxcross-env.cpp \
 programs/osxcross-conf.cpp \
 programs/osxcross-cmp.cpp \
 programs/sw_vers.cpp \
 programs/pkg-config.cpp

OBJS=$(subst .cpp,.o,$(SRCS))

all: wrapper

wrapper: $(OBJS)
	$(CXX) $(CXXFLAGS) -o wrapper $(OBJS) $(LDFLAGS)

.PHONY: clean

clean:
	rm -f $(BIN) $(OBJS)

-include $(SRCS:cpp=d)

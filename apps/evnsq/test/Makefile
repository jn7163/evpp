PLATFORM := $(shell uname)
PLATFORM_VERSION := $(shell uname -r| awk -F 'el' '{printf("el%d", substr($$2,1,1))}')

CXX := g++

CURRENT_DIR=$(shell echo `pwd`)
SOURCE_DIR=$(CURRENT_DIR)
EVPP_DIR=$(CURRENT_DIR)/../../..
APPS_DIR=$(EVPP_DIR)/apps

CPPFLAGS= -g -c -fPIC \
		  -D_DEBUG -D_USE_GTEST \
		  -Wshadow -Wcast-qual -Wcast-align -Wwrite-strings \
		  -Wsign-compare -Winvalid-pch -fms-extensions -Wall \
		  -MMD -Woverloaded-virtual -Wsign-promo -fno-gnu-keywords -std=c++11\
		  -I$(APPS_DIR)\
		  -I$(EVPP_DIR)\
		  -I$(EVPP_DIR)/3rdparty \

RTFLAGS := \
    -Wl,-rpath=. \
    -Wl,-rpath=$(EVPP_DIR)/evpp \
    -Wl,-rpath=$(APPS_DIR)/evnsq \

LDFLAGS= $(RTFLAGS) \
		 -L$(EVPP_DIR)/evpp \
		 -L$(APPS_DIR)/evnsq \
		 -lpthread \
		 -levpp -levnsq -levent -lglog

DIRNAME=$(shell basename `pwd`)

SRCS := $(wildcard *.cc)
OBJS := $(patsubst %.cc, %.o, $(SRCS))
DEPS := $(patsubst %.o, %.d, $(OBJS))

TARGET=evnsqtest

all : $(TARGET) 

check : $(TARGET)
	./$^

$(TARGET) : $(OBJS)
	$(CXX) $(OBJS) $(LDFLAGS) -o $@

-include $(DEPS)

%.o : %.cc
	$(CXX) $(CFLAGS) $(CPPFLAGS) $< -o $@

clean:
	rm -rf *.o *.d $(OBJS) $(DEPS) $(TARGET) *.tmp decoded.txt log


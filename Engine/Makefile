MODULE:=$(ENGINE)

CURR:=$(ROOT)/$(MODULE)
GEN_DIR:=$(CURR)/$(GEN)

CPP_SOURCE:=$(wildcard *.cpp)
OBJ_FILES:=$(patsubst %.cpp, $(GEN_DIR)/%.o, $(CPP_SOURCE))
CPP_SOURCE_FULL:=$(patsubst %.cpp, $(CURR)/%.cpp, $(CPP_SOURCE))
DEPS=$(patsubst %.o, %.d, $(OBJ_FILES))
LIB_NAME=$(BIN_DIR)/lib$(MODULE).so

LDFLAGS+=$(LDFLAGS_PYTHON)
LDFLAGS+=$(LDFLAGS_BOOST_SYSTEM)
LDFLAGS+=$(LDFLAGS_BOOST_FILESYSTEM)
LDFLAGS+=$(LDFLAGS_BOOST_LOG)
LDFLAGS+=$(LDFLAGS_BOOST_THREAD)
LDFLAGS+=$(LDFLAGS_BOOST_PYTHON)
LDFLAGS+=$(LDFLAGS_BOOST_DATETIME)
LDFLAGS+=-L$(BIN_DIR)
LDFLAGS+=-lData -lAlgorithm

INCL+=-I$(COMMON_DIR)
INCL+=-I$(PYTHON_INCLUDE_DIR)

DEF_BUILD_DLL+=-DENGINE_EXPORTS
DEF_BUILD_DLL+=-DDATA_IMPORTS -DALGORITHM_IMPORTS

all:$(LIB_NAME)

$(LIB_NAME):$(OBJ_FILES)
	$(LD) $(LINK_SHARED) -o $@ $^ $(LDFLAGS)

-include $(DEPS)

$(DEPS): $(GEN_DIR)/%.d: $(CURR)/%.cpp
	$(CC) -c $(INCL) $< -MM > $@.$$
	@sed -r 's,($*)\.o[:]*,$(GEN_DIR)/\1.o $@:,g' < $@.$$ > $@
	@rm -rf $@.$$

$(OBJ_FILES): $(GEN_DIR)/%.o: $(CURR)/%.cpp
	$(CC) -c -Wall -fPIC $(CFLAGS) $(CPPFLAGS) $(INCL) $< $(DEF_BUILD_DLL) -o $@

.PHONY: all show clean

show:
	@echo $(CPP_SOURCE)
	@echo $(OBJ_FILES)
	@echo $(DEPS)
	@echo $(CURR)
	@echo $(LIB_NAME)

clean:
	rm -rf $(GEN_DIR)/*
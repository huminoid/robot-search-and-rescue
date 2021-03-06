# Add the desired name of our executable here.
TARGET = Main

# Add compiler flags here, things like Warnings (-Wall), debugging (-g) or optimizations (-O)
CFLAGS = -O2 -Werror

# Add any preprocessor definitions here.
USER_DEFINES = 

# Add libraries needed by your program here.
USER_LIBS = -lpthread #-lncurses #If you want to use ncurses with your project, remove the first # on this line.

#######################################################################
# DO NOT EDIT PAST THIS LINE UNLESS YOU REALLY KNOW WHAT YOU'RE DOING!#
#######################################################################

DEP_DIR=dep
OBJ_DIR=obj
SRC_DIR=src

# Cross compilers for the ARM processor on the nemo
CROSS_CC = overo-gcc
CROSS_CXX = overo-g++


# Build a list of all of our source files by searching for .cpp, .C, .cc, .cxx, and .c files.
CPP_OTMP = $(patsubst $(SRC_DIR)/%.cpp,%.o,$(wildcard $(SRC_DIR)/*.cpp))
CPP_OTMP += $(patsubst $(SRC_DIR)/%.C,%.o,$(wildcard $(SRC_DIR)/*.C))
CPP_OTMP += $(patsubst $(SRC_DIR)/%.cc,%.o,$(wildcard $(SRC_DIR)/*.cc))
CPP_OTMP += $(patsubst $(SRC_DIR)/%.cxx,%.o,$(wildcard $(SRC_DIR)/*.cxx))
C_OTMP = $(patsubst $(SRC_DIR)/%.c,%.o,$(wildcard $(SRC_DIR)/*.c))

# Build the final lists of objects
CROSS_OBJS = $(patsubst %,$(OBJ_DIR)/nemo/%,$(CPP_OTMP)) $(patsubst %, $(OBJ_DIR)/nemo/%,$(C_OTMP))

# Build a list of dependency files
CROSS_DEPS = $(patsubst %.o,$(DEP_DIR)/nemo/%.d,$(CROSS_OBJS))


CSCI-445_DIR = ${HOME}/Nemo/Software/Gumstix
CSCI-445_INC_DIR = ${CSCI-445_DIR}/includes

SHARED_INCS = -I${SRC_DIR} -I${CSCI-445_INC_DIR}
CROSS_INCS = ${SHARED_INCS} -I/opt/IceE-1.2.0/include

CROSS_CSCI-445_OBJ_DIR = ${CSCI-445_DIR}/arm7

SHARED_DEFINES = -DROUTINE_CLASSNAME='${CLASS_NAME}' -DROUTINE_FILEPATH='<${CLASS_HEADER}>'
CROSS_DEFINES = ${USER_DEFINES} ${SHARED_DEFINES}

# Libraries requried for the nemo
CROSS_LIBS = ${USER_LIBS} -L/opt/IceE-1.2.0/lib -lIceE -ljpeg

CROSS_CSCI-445_OBJS =	${CROSS_CSCI-445_OBJ_DIR}/Image/ArrayData.o \
	${CROSS_CSCI-445_OBJ_DIR}/Ice/IceImageCompressor.o \
	${CROSS_CSCI-445_OBJ_DIR}/Ice/IceImageDecompressor.o \
	${CROSS_CSCI-445_OBJ_DIR}/Ice/ImageIce.o \
	${CROSS_CSCI-445_OBJ_DIR}/Ice/CSCI445.o \
	${CROSS_CSCI-445_OBJ_DIR}/captureMjpeg/color.o \
	${CROSS_CSCI-445_OBJ_DIR}/captureMjpeg/utils.o \
	${CROSS_CSCI-445_OBJ_DIR}/captureMjpeg/v4l2uvc.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/abort.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/backtrace.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/debug.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/demangle.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/error_context.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/error.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/freelist.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/fstring.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/mutex.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/pipe.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/prof.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/sfmt.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/stderror.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/stdiobuf.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/time.o \
	${CROSS_CSCI-445_OBJ_DIR}/rutz/trace.o \
	${CROSS_CSCI-445_OBJ_DIR}/serial/Serial.o \
	${CROSS_CSCI-445_OBJ_DIR}/Util/AllocAux.o \
	${CROSS_CSCI-445_OBJ_DIR}/Util/Alloc.o \
	${CROSS_CSCI-445_OBJ_DIR}/Util/log.o \
	${CROSS_CSCI-445_OBJ_DIR}/Util/sformat.o \
	${CROSS_CSCI-445_OBJ_DIR}/nemo/Nemo.o \
	${CROSS_CSCI-445_OBJ_DIR}/nemo/PhysicalNemo.o 



	# ${OBJ_DIR}/nemo/NemoMain.o # Even though this is compiled from files in ilab-includes
	# it depends on the individual project's NemoRoutine so it is compiled here rather than in the
	# ilab-includes makefile


# Pull in dependency info for our objects
-include ${CROSS_DEPS}


# Make targets

nemo : init ${CROSS_OBJS} ${CROSS_CSCI-445_OBJS}
	@echo Linking ${TARGET}
	@${CROSS_CXX} $(CROSS_DEFINES) ${CROSS_OBJS} ${CROSS_CSCI-445_OBJS} ${CROSS_LIBS} -o ${TARGET} 

# Commented out building simulator in all
all : nemo #simulator

clean :
	@rm -fr ${DEP_DIR}/*
	@rm -fr ${OBJ_DIR}/*
	@rm -fr ${TARGET}

init :
	@mkdir -p ${DEP_DIR}
	@mkdir -p ${DEP_DIR}/nemo
	@mkdir -p ${OBJ_DIR}
	@mkdir -p ${OBJ_DIR}/nemo
	@mkdir -p ${SRC_DIR}

install :
	@copyToBot ${TARGET}

# Make rules for cross compiled files
${OBJ_DIR}/nemo/%.o : ${SRC_DIR}/%.cpp
	@echo Cross-Compiling $<
	@${CROSS_CXX} ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} -o $@ -c $<
	@${CROSS_CXX} -MM -MT ${OBJ_DIR}/nemo/$*.o ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} $< > ${DEP_DIR}/nemo/$*.d

${OBJ_DIR}/nemo/%.o : ${SRC_DIR}/%.C
	@echo Cross-Compiling $<
	@${CROSS_CXX} ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} -o $@ -c $<
	@${CROSS_CXX} -MM -MT ${OBJ_DIR}/nemo/$*.o ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} $< > ${DEP_DIR}/nemo/$*.d

${OBJ_DIR}/nemo/%.o : ${SRC_DIR}/%.cc
	@echo Cross-Compiling $<
	@${CROSS_CXX} ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} -o $@ -c $<
	@${CROSS_CXX} -MM -MT ${OBJ_DIR}/nemo/$*.o ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} $< > ${DEP_DIR}/nemo/$*.d

${OBJ_DIR}/nemo/%.o : ${SRC_DIR}/%.cxx
	@echo Cross-Compiling $<
	@${CROSS_CXX} ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} -o $@ -c $<
	@${CROSS_CXX} -MM -MT ${OBJ_DIR}/nemo/$*.o ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} $< > ${DEP_DIR}/nemo/$*.d

${OBJ_DIR}/nemo/%.o : ${SRC_DIR}/%.c
	@echo Cross-Compiling $<
	@${CROSS_CC} ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} -o $@ -c $<
	@${CROSS_CC} -MM -MT ${OBJ_DIR}/nemo/$*.o ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} $< > ${DEP_DIR}/nemo/$*.d

${OBJ_DIR}/nemo/NemoMain.o : ${CSCI-445_INC_DIR}/nemo/NemoMain.C ${SRC_DIR}/${CLASS_HEADER} ${SRC_DIR}/${CLASS_IMPL}
	@echo Cross-Compiling $<
	@${CROSS_CC} ${CROSS_DEFINES} ${CFLAGS} ${CROSS_INCS} -o $@ -c $<


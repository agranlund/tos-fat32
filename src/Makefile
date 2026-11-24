
BIN = fat32.prg

CPUFLAGS = -m68000

OPTS = -Os 

OBJS = 	main.o \
		debug.o \
		hdd.o \
		asm.o \
		fatfs/source/ff.o \
		fatfs/source/ffunicode.o \
		#


# -----------------------------------------------------------------------------------
# Tools
# -----------------------------------------------------------------------------------

GCDIR			:= gcc

GCC			    := m68k-atari-mintelf-gcc
GAS			    := m68k-atari-mintelf-gcc
GLD			    := m68k-atari-mintelf-gcc
GSTRIP          := m68k-atari-mintelf-strip -s
GFLAGS          := m68k-atari-mintelf-flags

GCC_OBJS		:= $(GCDIR)/lgcc.o

GCC_LIBDIR      := $(GCDIR)/lib
GCC_INCDIR      := $(GCDIR)/inc
GCC_BINDIR      := $(GCDIR)/bin

LIBCMINI_DIR	:= $(GCDIR)
LIBCMINI_INC    := $(GCDIR)/include
LIBCMINI_LIB    := $(GCDIR)/lib
LIBCMINI_CRT	:= $(LIBCMINI_LIB)/crt0.o
LIBCMINI_SRC    := $(GCDIR)/src/libcmini

# -----------------------------------------------------------------------------------
# GCC compiler flags
# -----------------------------------------------------------------------------------

MAPFILE = $(BIN).map

INCLUDE = -I. -I$(ROOTDIR)/gcc -I$(ROOTDIR)/lib

SFLAGS = $(OPTS) $(INCLUDE) -Wa,--register-prefix-optional

CFLAGS = $(OPTS) $(DEFS) $(INCLUDE) -I$(LIBCMINI_INC) \
	-std=c99 -fomit-frame-pointer -fno-exceptions -ffast-math \
	-fno-strict-aliasing -Wno-stringop-truncation \
    -Wa,--register-prefix-optional \
	-Wall -Wno-multichar -Wno-unused -Wno-unused-but-set-variable -Wno-unused-variable -Wno-unused-function -Wno-format
	#

LFLAGS = -L$(LIBCMINI_LIB) -nodefaultlibs -nostdlib -nostartfiles -lgcc -lcmini -Wl,-Map,$(MAPFILE)

LINKOBJS = $(LIBCMINI_CRT) $(GCC_OBJS) $(OBJS)



# -----------------------------------------------------------------------------------
# Standard rules
# -----------------------------------------------------------------------------------

.PHONY: clean distclean


all: $(current_makefile) $(LINKOBJS)
	$(GCC) $(CFLAGS) $(CPUFLAGS) $(LINKOBJS) $(LFLAGS) -o $(BIN)
	$(GSTRIP) $(BIN)
	$(GFLAGS) $(PRGFLAGS) $(BIN)
	cp $(BIN) ../

gcc/lgcc.o: gcc/lgcc.S
	$(GCC) -m68000 -c $< -o $@

gcc/lib/crt0.o:
	make -C gcc/libcmini install

%.o : %.S
	$(GCC) $(SFLAGS) $(CPUFLAGS) -c $< -o $@

%.o : %.c
	$(GCC) $(CFLAGS) $(CPUFLAGS) -c $< -o $@

clean:
	rm -f $(BIN)
	rm -f $(OBJS)
	rm -f $(OBJS:.o=.d)
	rm -f $(OBJS:.o=.O)
	rm -f $(MAPFILE)

distclean: clean
	make -C gcc/libcmini clean
	rm -rf gcc/include/*
	rm -rf gcc/lib/*
	rm -f gcc/lgcc.o



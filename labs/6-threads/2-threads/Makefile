# Makefile for a single file.  Makes it really hard to break anything.
MAKEFLAGS +=  --no-print-directory

# we've had some make issues going cross windows/mac/linux, so we just do brute stupid
# to make things work: no fancy rules, just always recompile.
NAME = test-thread

# check that your path is defined.
ifndef CS140E_PATH
$(error CS140E_PATH is not set: this should contain the absolute path to where this directory is.  Define it in your shell's initialiation.  For example, .tcshrc for tcsh or .bashrc for bash)
endif

include $(CS140E_PATH)/libpi/includes.mk

CFLAGS += -Wno-unused-function -Wno-unused-variable 

# if you want to compile everything in a directory, do something like this.
#   SOURCES := $(wildcard ./*.c)
#   OBJS := $(patsubst %.c, %.o, $(SOURCES))
PROGS := 1-test-thread 3-test-yield
PROG_BIN := $(patsubst %, %.bin, $(PROGS))
OBJS := rpi-thread.o thread-asm.o

# if you want to add all the .h's as dependencies:
DEPS := $(wildcard ./*.h) $(LPI) ./Makefile
# DEPS := $(LPI) ./Makefile 

all: libpi $(OBJS) $(PROG_BIN)

libpi: 
	@ # @make -s -C $(LPP)
	@ make -C $(LPP)

fake:
	@make -f ./Makefile.unix NAME=equiv-check.fake

$(OBJS): $(DEPS)

%.o: %.S
	$(CC) -c $(CPP_ASFLAGS) $< -o $@

# <LPI> will get rebuilt if anything changes, so we don't need any other
# dependencies.
%.bin: %.o $(MEMMAP) $(OBJS) $(LPI) ./Makefile
	$(LD) $(START) $< $(OBJS) -T $(LPP)/memmap -o $@.elf  $(LPI)
	$(OD) -D  -S $@.elf > $@.list
	$(OCP) $@.elf -O binary $@.bin

run:
	my-install $(NAME).bin

tags: 
	ctags *.[chSs] */*.[chSs]

clean:
	rm -rf $(BUILD_DIR) $(NAME) *~ tags *.o *.bin *.elf *.list *.img Makefile.bak 

cleanall:
	make -C $(LPP) clean
	make clean

.PHONY: libpi all run tags clean  cleanall

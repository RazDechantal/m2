# Makefile for building code for HOST and MSP432 platforms

# Architecture Specific Flags
CPU = cortex-m4
ARCH = armv7e-m
SPECS = nosys.specs

# Platform Specific Flags
LINKER_FILE = msp432p401r.lds

# Compiler options
CC = gcc
LD = arm-none-eabi-ld
CFLAGS = -Wall -Werror -g -O0 -std=c99
CPPFLAGS =
LDFLAGS =
ARCH_FLAGS =
PLATFORM =
SOURCES =
INCLUDES = -I../include/common -I../include/msp432 -I../include/CMSIS
#INCLUDES = -I../include/msp432 -I../include/CMSIS

# Platform-specific settings
ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	LDFLAGS += -T msp432p401r.lds
	PLATFORM = -MSP432
	CFLAGS += $(PLATFORM)
	ARCH_FLAGS = -mcpu=cortex-m4 -mthumb -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16 --specs=nosys.specs
else ifeq ($(PLATFORM),HOST)
	PLATFORM = -HOST
	CFLAGS += $(PLATFORM)
endif

# List of source files and include directories
include sources.mk

# Object files
OBJECTS = $(SOURCES:.c=.o)

# Targets
.PHONY: all clean build

build: c1m2.out


%.o: %.c
	$(CC) -c $(CFLAGS) $(ARCH_FLAGS) $(INCLUDES) -o $@ $<

c1m2.out: $(OBJECTS)
#	$(CC) $(CFLAGS) $(ARCH_FLAGS) $(LDFLAGS) $(OBJECTS) -o $@ -Wl,-Map=c1m2.map
	$(CC) $(CFLAGS) $(ARCH_FLAGS) $(LDFLAGS) $(OBJECTS) -o $@ -Wl,-Map=c1m2.map

clean:
	rm -f *.o *.d *.i *.asm c1m2.out c1m2.map

# Individual source file rules
$(SOURCES:.c=.i): %.i: %.c
	$(CC) $(CFLAGS) $(ARCH_FLAGS) $(INCLUDES) -E $< > $@

$(SOURCES:.c=.asm): %.asm: %.c
	$(CC) -S $(CFLAGS) $(ARCH_FLAGS) $(INCLUDES) $< -o $@
	objdump -d $(patsubst %.asm,%.o,$@) > $(patsubst %.asm,%.asm.txt,$@)

# Dependency file rules
%.d: %.c
	$(CC) -M $(CFLAGS) $(ARCH_FLAGS) $(INCLUDES) $< -o $@

-include $(SOURCES:.c=.d)

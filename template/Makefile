#
# This Source Code Form is subject to the terms of the Mozilla Public
# License, v. 2.0. If a copy of the MPL was not distributed with this
# file, You can obtain one at https://mozilla.org/MPL/2.0/.
#

.SUFFIXES:

MINRT		:= ..

PROJECT		:= gba-minrt-template

SOURCES		:= rt/crt0.s src/main.c

INCLUDES	:=

LIBDIRS		:=

LIBS		:=

BUILDDIR	:= build

CFLAGS		:= -g3 -gdwarf-4 -O2 -std=c99 -ffunction-sections -fdata-sections
LDFLAGS		:= -mthumb -nostartfiles \
		   -specs=nano.specs -specs=nosys.specs \
		   -Wl,-T,rom.ld \
		   -L$(MINRT)/rt

#
# Internal
#

vpath rt/% $(MINRT)

CC 	= arm-none-eabi-gcc
OBJCOPY = arm-none-eabi-objcopy

ELFFILE = $(BUILDDIR)/$(PROJECT).elf
ROMFILE = $(BUILDDIR)/$(PROJECT).gba
MAPFILE = $(BUILDDIR)/$(PROJECT).map

OBJECTS = $(SOURCES:%=$(BUILDDIR)/obj/%.o)
DEPENDS = $(SOURCES:%=$(BUILDDIR)/dep/%.d)
OBJDIRS = $(dir $(BUILDDIR) $(OBJECTS) $(DEPENDS))

CFLAGS += \
	  -mcpu=arm7tdmi \
	  -mabi=aapcs \
	  -mthumb \
	  $(LIBDIRS:%=-I%/include) \
	  $(INCLUDES:%=-I%) \

LDFLAGS += \
	   -Wl,--gc-sections \
	   -Wl,-Map,$(MAPFILE) \
	   $(LIBDIRS:%=-L%/lib) \
	   $(LIBS:%=-l%) \

$(ROMFILE): $(ELFFILE)
$(ELFFILE): $(OBJECTS)

$(OBJECTS): builddirs

$(BUILDDIR)/obj/%.o: %
	@echo "$<"
	@$(CC) -c -o $@ $(CFLAGS) -MMD -MP -MF $(BUILDDIR)/dep/$<.d $<

%.gba:
	@echo "$@"
	@$(OBJCOPY) -O binary $< $@

%.elf:
	@echo "$@"
	@$(CC) -o $@ $^ $(LDFLAGS)

builddirs:
	@mkdir -p $(OBJDIRS)

clean:
	@echo "clean"
	@rm -rf $(BUILDDIR)

.PHONY: builddirs clean

-include $(DEPENDS)

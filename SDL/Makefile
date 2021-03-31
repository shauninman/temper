# PCE emulator makefile
# Gilead Kutnick - Exophase
#PROFILE=YES
#PROFILE=APPLY

# Global definitions	gcc4.8 is faster
CROSS_COMPILE := /opt/trimui-toolchain/bin/arm-buildroot-linux-gnueabi-
AS        = $(CROSS_COMPILE)gcc
CC        = $(CROSS_COMPILE)gcc
STRIP     = $(CROSS_COMPILE)strip --strip-all --remove-section=.comment --remove-section=.note

OBJS      = main.o cpu.o memory.o irq.o io.o video.o timer.o psg.o disasm.o    \
            screen.o event.o audio.o menu.o debug.o cd.o adpcm.o netplay.o     \
            arcade_card.o bin_cue.o                                         \
            SDL_main.o SDL_screen.o SDL_event.o SDL_menu.o SDL_audio.o crc32.o \
			bitwise.o block.o codebook.o floor0.o floor1.o framing.o \
			info.o mapping0.o mdct.o registry.o res012.o sharedbook.o \
			synthesis.o vorbisfile.o window.o
BIN       ?= temper 

ifeq ($(PROFILE), YES)
CFLAGS 		+= -fprofile-generate -fprofile-dir=/mnt/SDCARD/profile/temper
else ifeq ($(PROFILE), APPLY)
CFLAGS		+= -fprofile-use -fprofile-dir=./profile/ -fbranch-probabilities
endif

# Platform specific definitions

VPATH      	+= .. ../SDL ../linux ../tremor
CFLAGS		+= -DTRIMUI_BUILD -Ofast -marm -march=armv5te -mtune=arm926ej-s
CFLAGS		+= -fdata-sections -ffunction-sections -fno-PIC -flto -fno-math-errno
#for gcc 6.1
INCLUDES   	= -I.. -I../SDL -I../linux -I../tremor

#LIBS		= -nodefaultlibs
LIBS    	+= -lc -lgcc -lSDL -lasound -lz -lm -lbz2 -pthread -Wl,--sort-common,--gc-sections -flto -s
LIBS		+= -lSDL_image -lSDL_ttf -ldl

ifeq ($(PROFILE), YES)
LIBS 		+= -lgcov
endif

# Compilation:

.SUFFIXES: .c .S

%.o: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -c -o $@ $< $(LIBS)

%.o: %.S
	$(AS) ${CFLAGS} -c -o $@ $<

all:	$(OBJS)
	$(CC) $(CFLAGS) $(OBJS) -o $(BIN) $(LIBS)
	${STRIP} ${BIN}

clean:
	rm -f *.o $(BIN) 

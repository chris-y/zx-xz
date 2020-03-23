#
# Makefile
#
# Author: Lasse Collin <lasse.collin@tukaani.org>
#
# This file has been put into the public domain.
# You can do whatever you want with this file.
#

CC = zcc +zxn
BCJ_CPPFLAGS =
#-DXZ_DEC_X86 -DXZ_DEC_POWERPC -DXZ_DEC_IA64 \
#		-DXZ_DEC_ARM -DXZ_DEC_ARMTHUMB -DXZ_DEC_SPARC
CPPFLAGS = -DXZ_USE_CRC32 -DXZ_DEC_ANY_CHECK
CFLAGS = -v -startup=30 -clib=sdcc_iy -SO3 --opt-code-size -pragma-define=CLIB_MALLOC_HEAP_SIZE=-1 $(CPPFLAGS)
RM = rm -f
VPATH = ../linux/include/linux ../linux/lib/xz
COMMON_SRCS = xz_crc32.c xz_crc64.c xz_dec_stream.c xz_dec_lzma2.c xz_dec_bcj.c
COMMON_OBJS = $(COMMON_SRCS:.c=.o)
COMMON_SRCS_LINUX = $(addprefix ../linux/lib/xz/, $(COMMON_SRCS))
XZMINIDEC_OBJS = xzminidec.o
BYTETEST_OBJS = bytetest.o
BUFTEST_OBJS = buftest.o
BOOTTEST_OBJS = boottest.o
XZ_HEADERS = xz.h xz_private.h xz_stream.h xz_lzma2.h xz_config.h
PROGRAMS = xzminidec bytetest buftest boottest
LDFLAGS = -subtype=dotn -create-app

ALL_CFLAGS = -I../linux/include/linux -I. $(CFLAGS)

all: $(PROGRAMS)

%.o: %.c $(XZ_HEADERS)
	$(CC) $(ALL_CFLAGS) -c -o $@ $<

xzminidec: $(COMMON_OBJS) $(XZMINIDEC_OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $(COMMON_OBJS) $(XZMINIDEC_OBJS)

xzdec: $(COMMON_SRCS) xzminidec.c
	$(CC) $(ALL_CFLAGS) -o xzdec $(COMMON_SRCS_LINUX) xzminidec.c

bytetest: $(COMMON_OBJS) $(BYTETEST_OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $(COMMON_OBJS) $(BYTETEST_OBJS)

buftest: $(COMMON_OBJS) $(BUFTEST_OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $(COMMON_OBJS) $(BUFTEST_OBJS)

boottest: $(BOOTTEST_OBJS) $(COMMON_SRCS)
	$(CC) $(ALL_CPPFLAGS) $(CFLAGS) $(LDFLAGS) -o $@ $(BOOTTEST_OBJS)

.PHONY: clean
clean:
	-$(RM) $(COMMON_OBJS) $(XZMINIDEC_OBJS) $(BYTETEST_OBJS) \
		$(BUFTEST_OBJS) $(BOOTTEST_OBJS) $(PROGRAMS)

include ../config.mk

NAME=r_util
CFLAGS+=-DR2_PLUGIN_INCORE -I$(TOP)/shlr
PCLIBS=@LIBZIP@ @DL_LIBS@

ifneq ($(OSTYPE),darwin)
ifneq ($(OSTYPE),haiku)
LDFLAGS+=-lm
LINK=-lm
endif
endif

OBJS=binheap.o mem.o unum.o str.o hex.o file.o range.o charset.o
OBJS+=prof.o cache.o sys.o buf.o sys_w32.o ubase64.o base85.o base91.o
OBJS+=list.o flist.o chmod.o graph.o event.o alloc.o donut.o print_code.o
OBJS+=regex/regcomp.o regex/regerror.o regex/regexec.o uleb128.o
OBJS+=sandbox.o calc.o thread.o thread_sem.o thread_lock.o thread_cond.o
OBJS+=strpool.o bitmap.o time.o format.o pie.o print.o utype.o w32.o
OBJS+=seven.o randomart.o zip.o debruijn.o log.o getopt.o table.o
OBJS+=utf8.o utf16.o utf32.o strbuf.o lib.o name.o spaces.o signal.o syscmd.o
OBJS+=udiff.o bdiff.o stack.o queue.o tree.o idpool.o assert.o
OBJS+=punycode.o pkcs7.o x509.o asn1.o astr.o json_parser.o json_indent.o skiplist.o
OBJS+=pj.o rbtree.o intervaltree.o qrcode.o vector.o skyline.o str_constpool.o str_trim.o
OBJS+=ascii_table.o protobuf.o graph_drawable.o axml.o

ifeq (${HAVE_GPERF},1)
OBJS+=d/ascii.o
OBJS+=d/pokered.o
OBJS+=d/ebcdic37.o
endif

ifeq (${HAVE_LIB_GMP},1)
  OBJS+=big-gmp.o
else
  ifeq (${HAVE_LIB_SSL},1)
    OBJS+=big-ssl.o
  else
    OBJS+=big.o
  endif
endif

include deps.mk

CWD=$(shell pwd)

LDFLAGS+=${BN_LIBS}
LDFLAGS+=${TH_LIBS}
LDFLAGS+=${DL_LIBS}
LDFLAGS+=-lm

# NetBSD 7.0 ships with backtrace(3) in -lexecinfo
ifeq (${BUILD_OS},netbsd)
ifneq ($(shell expr "`uname -r`" : '[0-6]\.'), 2)
  LDFLAGS+=-lexecinfo
endif
endif

# FreeBSD 10.0 ships with backtrace(3) in -lexecinfo
ifeq (${BUILD_OS},freebsd)
ifneq ($(shell expr "`uname -r`" : '[0-9]\.'), 2)
  LDFLAGS+=-lexecinfo
endif
endif

ifeq (${BUILD_OS},dragonfly)
  LDFLAGS+=-lexecinfo
endif

ifeq (${BUILD_OS},haiku)
  LDFLAGS+=-lexecinfo
endif

EXTRA_PRE+=spp_config
EXTRA_PRE+=charsets
EXTRA_TARGETS=$(EXTRA_PRE)
EXTRA_CLEAN=doclean

include ../rules.mk
include sdb.mk
include spp.mk

charsets:
	$(MAKE) -C d
.PHONY: charsets

doclean:
	$(MAKE) -C d clean
.PHONY: doclean

sync-regex regex-sync:
	-rm -rf src/
	cvs -qd anoncvs@anoncvs.ca.openbsd.org:/cvs get -P src/lib/libc/regex
	cp src/lib/libc/regex/* regex

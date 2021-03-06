#
# OpenSSL/crypto/rc4/Makefile
#

DIR=	rc4
TOP=	../..
CC=	cc
CPP=    $(CC) -E
INCLUDES=
CFLAG=-g
AR=		ar r

RC4_ENC=rc4_enc.o rc4_skey.o

CFLAGS= $(INCLUDES) $(CFLAG)
ASFLAGS= $(INCLUDES) $(ASFLAG)
AFLAGS= $(ASFLAGS)

GENERAL=Makefile

LIB=$(TOP)/libcrypto.a
LIBSRC=rc4_skey.c rc4_enc.c
LIBOBJ=$(RC4_ENC)

SRC= $(LIBSRC)

HEADER=	rc4_locl.h

ALL=    $(GENERAL) $(SRC) $(HEADER)

top:
	(cd ../..; $(MAKE) DIRS=crypto SDIRS=$(DIR) sub_all)

all:	lib

lib:	$(LIBOBJ)
	$(AR) $(LIB) $(LIBOBJ)
	$(RANLIB) $(LIB) || echo Never mind.
	@touch lib

rc4-586.s:	asm/rc4-586.pl ../perlasm/x86asm.pl
	$(PERL) asm/rc4-586.pl $(PERLASM_SCHEME) $(CFLAGS) $(PROCESSOR) > $@

rc4-x86_64.s: asm/rc4-x86_64.pl
	$(PERL) asm/rc4-x86_64.pl $(PERLASM_SCHEME) > $@
rc4-md5-x86_64.s:	asm/rc4-md5-x86_64.pl
	$(PERL) asm/rc4-md5-x86_64.pl $(PERLASM_SCHEME) > $@

rc4-ia64.S: asm/rc4-ia64.pl
	$(PERL) asm/rc4-ia64.pl $(CFLAGS) > $@

rc4-parisc.s:	asm/rc4-parisc.pl
	$(PERL) asm/rc4-parisc.pl $(PERLASM_SCHEME) $@

rc4-ia64.s: rc4-ia64.S
	@case `awk '/^#define RC4_INT/{print$$NF}' $(TOP)/include/openssl/opensslconf.h` in \
	int)	set -x; $(CC) $(CFLAGS) -DSZ=4 -E rc4-ia64.S > $@ ;; \
	char)	set -x; $(CC) $(CFLAGS) -DSZ=1 -E rc4-ia64.S > $@ ;; \
	*)	exit 1 ;; \
	esac

# GNU make "catch all"
rc4-%.s:	asm/rc4-%.pl;	$(PERL) $< $(PERLASM_SCHEME) $@

files:
	$(PERL) $(TOP)/util/files.pl Makefile >> $(TOP)/MINFO

tags:
	ctags $(SRC)

tests:

lint:
	lint -DLINT $(INCLUDES) $(SRC)>fluff

depend:
	@[ -n "$(MAKEDEPEND)" ] # should be set by upper Makefile...
	$(MAKEDEPEND) -- $(CFLAG) $(INCLUDES) $(DEPFLAG) -- $(PROGS) $(LIBSRC)

dclean:
	$(PERL) -pe 'if (/^# DO NOT DELETE THIS LINE/) {print; exit(0);}' $(MAKEFILE) >Makefile.new
	mv -f Makefile.new $(MAKEFILE)

clean:
	rm -f *.s *.o *.obj lib tags core .pure .nfs* *.old *.bak fluff

# DO NOT DELETE THIS LINE -- make depend depends on it.

rc4_enc.o: ../../e_os.h ../../include/openssl/bio.h
rc4_enc.o: ../../include/openssl/buffer.h ../../include/openssl/crypto.h
rc4_enc.o: ../../include/openssl/e_os2.h ../../include/openssl/err.h
rc4_enc.o: ../../include/openssl/lhash.h ../../include/openssl/opensslconf.h
rc4_enc.o: ../../include/openssl/opensslv.h ../../include/openssl/ossl_typ.h
rc4_enc.o: ../../include/openssl/rc4.h ../../include/openssl/safestack.h
rc4_enc.o: ../../include/openssl/stack.h ../../include/openssl/symhacks.h
rc4_enc.o: ../include/internal/cryptlib.h rc4_enc.c rc4_locl.h
rc4_skey.o: ../../e_os.h ../../include/openssl/bio.h
rc4_skey.o: ../../include/openssl/buffer.h ../../include/openssl/crypto.h
rc4_skey.o: ../../include/openssl/e_os2.h ../../include/openssl/err.h
rc4_skey.o: ../../include/openssl/lhash.h ../../include/openssl/opensslconf.h
rc4_skey.o: ../../include/openssl/opensslv.h ../../include/openssl/ossl_typ.h
rc4_skey.o: ../../include/openssl/rc4.h ../../include/openssl/safestack.h
rc4_skey.o: ../../include/openssl/stack.h ../../include/openssl/symhacks.h
rc4_skey.o: ../include/internal/cryptlib.h rc4_locl.h rc4_skey.c

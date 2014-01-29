# Makefile for the "fax" subtree
# of mgetty+sendfax
#
# SCCS: $Id: Makefile,v 4.9 2006/02/22 17:25:30 gert Exp $ Copyright (c) Gert Doering
#

CC=gcc
CFLAGS=-O2 -I.. -Wall

FAX_SCRIPTS=faxspool faxrunq faxq fax-rm 

zigzag: 
	cd .. ; $(MAKE) subdirs

all: $(FAX_SCRIPTS) faxrunqd faxheader faxq-helper

faxspool: faxspool.in ../sedscript
	../sedscript <faxspool.in >faxspool

faxrunq: faxrunq.in ../sedscript
	../sedscript <faxrunq.in >faxrunq

faxrunqd: faxrunqd.in ../sedscript
	../sedscript <faxrunqd.in >faxrunqd

faxq: faxq.in ../sedscript
	../sedscript <faxq.in >faxq

fax-rm: fax-rm.in ../sedscript
	../sedscript <fax-rm.in >fax-rm

faxheader: faxheader.in ../sedscript
	../sedscript <faxheader.in >faxheader

../sedscript: ../policy.h ../Makefile ../mksed.c
	@cd .. ; $(MAKE) sedscript

faxq-helper: faxq-helper.o 
	$(CC) $(CFLAGS) -o faxq-helper faxq-helper.o

faxq-helper.o: faxq-helper.c ../sedscript
	$(CC) $(CFLAGS) -DFAX_SPOOL_OUT=\"$(FAX_SPOOL_OUT)\" \
		-DFAX_OUT_USER=\"$(FAX_OUT_USER)\" \
		-DFAX_ALLOW=\"$(CONFDIR)/fax.allow\" \
		-DFAX_DENY=\"$(CONFDIR)/fax.deny\" \
		-c faxq-helper.c

clean:
	-rm -f $(FAX_SCRIPTS) faxrunqd faxheader faxq-helper *.o 

install: all
#
# programs / scripts
#
	-for i in $(FAX_SCRIPTS); do \
		mv -f $(DESTDIR)$(BINDIR)/$$i $(DESTDIR)$(BINDIR)/$$i.old 2>/dev/null ; \
		$(INSTALL) -m 755 $$i $(DESTDIR)$(BINDIR) ; \
	done
	$(INSTALL) -m 755 faxrunqd $(DESTDIR)$(SBINDIR)
	$(INSTALL) faxq-helper $(DESTDIR)$(LIBDIR)
	chown $(FAX_OUT_USER) $(DESTDIR)$(LIBDIR)/faxq-helper
	chmod 4711 $(DESTDIR)$(LIBDIR)/faxq-helper
#
# data files
#
	$(INSTALL) -m 644 cour25.pbm $(DESTDIR)$(LIBDIR)
	$(INSTALL) -m 644 cour25n.pbm $(DESTDIR)$(LIBDIR)
	-test -f $(CONFDIR)/faxheader || \
		$(INSTALL) -m 644 faxheader $(DESTDIR)$(CONFDIR)
#
# /etc/magic
#
	-@test -r /etc/magic && \
	    if grep "string.*P1" /etc/magic >/dev/null ; then : ; \
	    else \
		echo "WARNING: no PBM magic (string P1) found in /etc/magic" ; \
		echo "         maybe you should append the definitions in \`\`fax/etc-magic''." ; \
	    fi
#
# faxspool.rules sample file
#
	$(INSTALL) -m 644 faxspool.rules $(DESTDIR)$(CONFDIR)/faxspool.rules.sample

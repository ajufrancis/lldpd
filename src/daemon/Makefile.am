AM_CFLAGS = -I $(top_srcdir)/include

sbin_PROGRAMS = lldpd
dist_man_MANS = lldpd.8

noinst_LTLIBRARIES = liblldpd.la

## Convenience library for lldpd and tests
liblldpd_la_SOURCES  = \
	frame.h frame.c \
	lldp.c lldp-tlv.h \
	cdp.c cdp.h \
	sonmp.c sonmp.h \
	edp.c edp.h \
	client.c \
	priv.c privsep_io.c \
	interfaces.c \
	event.c lldpd.c
liblldpd_la_CFLAGS   = $(AM_CFLAGS) @LIBEVENT_CFLAGS@
liblldpd_la_CPPFLAGS = -DSYSCONFDIR='"$(sysconfdir)"' -DLLDPCLI_PATH='"$(sbindir)/lldpcli"'
liblldpd_la_LIBADD   = \
	$(top_builddir)/src/libcommon-daemon-client.la \
	$(top_builddir)/src/libcommon-daemon-lib.la @LIBEVENT_LIBS@

if HOST_OS_LINUX
liblldpd_la_SOURCES += \
	forward-linux.c \
	interfaces-linux.c \
	netlink.c \
	dmi-linux.c \
	priv-linux.c
endif
if HOST_OS_DRAGONFLY
liblldpd_la_SOURCES += \
	forward-bsd.c \
	interfaces-bpf.c \
	interfaces-bsd.c \
	dmi-dummy.c \
	priv-bsd.c
endif
if HOST_OS_FREEBSD
liblldpd_la_SOURCES += \
	forward-bsd.c \
	interfaces-bpf.c \
	interfaces-bsd.c \
	dmi-freebsd.c \
	priv-bsd.c
endif
if HOST_OS_OPENBSD
liblldpd_la_SOURCES += \
	interfaces-bpf.c \
	forward-bsd.c \
	interfaces-bsd.c \
	dmi-openbsd.c \
	priv-bsd.c
endif
if HOST_OS_NETBSD
liblldpd_la_SOURCES += \
	forward-bsd.c \
	interfaces-bpf.c \
	interfaces-bsd.c \
	dmi-dummy.c \
	priv-bsd.c
endif
if HOST_OS_OSX
liblldpd_la_SOURCES += \
	forward-bsd.c \
	interfaces-bpf.c \
	interfaces-bsd.c \
	dmi-osx.c \
	priv-bsd.c
liblldpd_la_LDFLAGS  = -framework Foundation
liblldpd_la_LDFLAGS += -framework CoreFoundation -framework IOKit
liblldpd_la_LDFLAGS += -framework IOKit
endif
if HOST_OS_SOLARIS
liblldpd_la_SOURCES += \
	forward-solaris.c \
	interfaces-bpf.c \
	interfaces-solaris.c \
	dmi-dummy.c \
	priv-bsd.c
endif

# Add SNMP support if needed
if USE_SNMP
liblldpd_la_SOURCES += agent.c agent_priv.c agent.h
liblldpd_la_CFLAGS  += @NETSNMP_CFLAGS@
liblldpd_la_LIBADD  += @NETSNMP_LIBS@
endif

## lldpd
lldpd_SOURCES = main.c
lldpd_LDADD   = liblldpd.la @LIBEVENT_LDFLAGS@

## libevent
if LIBEVENT_EMBEDDED
event.c: $(top_builddir)/libevent/libevent.la
$(top_builddir)/libevent/libevent.la: $(top_srcdir)/libevent/*.c $(top_srcdir)/libevent/*.h
	(cd $(top_builddir)/libevent && $(MAKE))
endif

## systemd service file
if HAVE_SYSTEMDSYSTEMUNITDIR
systemdsystemunit_DATA = lldpd.service
endif

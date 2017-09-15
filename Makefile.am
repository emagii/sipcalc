AUTOMAKE_OPTIONS = subdir-objects

ACLOCAL_AMFLAGS = ${ACLOCAL_FLAGS} -I m4

AM_CPPFLAGS = $(DEPS_CFLAGS)

LIB_CFLAGS = $(LIBNSL_CFLAGS)

AM_CFLAGS = -I$(top_srcdir)/src/include \
		-Wall -O0 -g3 -fmessage-length=0  @AM_CFLAGS@

sipcalc_LDADD = $(DEPS_LIBS) $(LIBNSL_LIBS)

bin_PROGRAMS = sipcalc

sipcalc_SOURCES =		\
	src/sub-func.c		\
	src/interface.c		\
	src/sub-output.c	\
	src/sub.c		\
	src/include/sub.h	\
	src/include/sub-o.h

man_MANS = doc/sipcalc.1

dist_noinst_SCRIPTS = autogen.sh

ACLOCAL_AMFLAGS = ${ACLOCAL_FLAGS}

AM_CPPFLAGS = $(DEPS_CFLAGS)

LIB_CFLAGS = -L/usr/local/lib	$(LIBNSL_CFLAGS)

AM_CFLAGS = -I/usr/local/include -I$(top_srcdir)/include \
		-Wall -O0 -g3 -fmessage-length=0  @AM_CFLAGS@

sipcalc_LDADD = $(DEPS_LIBS) $(LIBNSL_LIBS)

bin_PROGRAMS = sipcalc

sipcalc_SOURCES =		\
	sub-func.c		\
	interface.c		\
	sub-output.c		\
	sub.c			\
	../include/sub.h	\
	../include/sub-o.h

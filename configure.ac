dnl AC_INIT(include/sub.h)
dnl AM_INIT_AUTOMAKE(sipcalc,1.1.6)
AC_INIT([sipcalc], [1.1.7], [simon@routemeister.net],
             [sipcalc-1.1.7], [http://www.routemeister.net/projects/sipcalc/])
AC_PREREQ([2.59])
AM_CONFIG_HEADER(config.h)

AC_PREFIX_DEFAULT(/usr)

AC_PROG_LIBTOOL
AC_CONFIG_MACRO_DIR([m4])

AC_ARG_ENABLE(shared,
[ --enable-shared=[yes/no] Enable use of shared libraries [default=yes]],,
	enable_shared=yes)
AC_SUBST(enable_shared)

AC_ARG_ENABLE(static,
[ --enable-static=[yes/no] Enable use of static libraries [default=yes]],,
	enable_static=yes)
AC_SUBST(enable_static)

AC_PROG_CC
AC_PROG_INSTALL

AM_INIT_AUTOMAKE([1.10 -Wall no-define])

dnl - Configuration.
AC_HEADER_STDC
AC_CHECK_HEADERS(stdint.h sys/ioctl.h unistd.h sys/types.h sys/bitypes.h sys/sockio.h netdb.h ctype.h)
AC_PROG_GCC_TRADITIONAL

# distribute additional compiler and linker flags among Makefiles
# --> set and change these variables instead of CXXFLAGS or LDFLAGS (for user only)
AC_SUBST([AM_CFLAGS])
AC_SUBST([AM_LDFLAGS])

AH_TEMPLATE([HAVE_U_INT32_T], [])
AH_TEMPLATE([HAVE_U_INT16_T], [])
AH_TEMPLATE([HAVE_U_INT8_T], [])
AH_TEMPLATE([HAVE_UINT32_T], [])
AH_TEMPLATE([HAVE_UINT16_T], [])
AH_TEMPLATE([HAVE_UINT8_T], [])
AH_BOTTOM([#include <stdint.h>])
AH_BOTTOM([#define	VERSION	PACKAGE_VERSION])

dnl Check for uint[8,16,32]_t
AC_CHECK_TYPE(u_int32_t,[AC_DEFINE([HAVE_U_INT32_T],[1],[])])
AC_CHECK_TYPE(u_int16_t,[AC_DEFINE([HAVE_U_INT16_T],[1],[])])
AC_CHECK_TYPE(u_int8_t,	[AC_DEFINE([HAVE_U_INT8_T],[1],[])])
AC_CHECK_TYPE(uint32_t,	[AC_DEFINE([HAVE_UINT32_T],[1],[])])
AC_CHECK_TYPE(uint16_t, [AC_DEFINE([HAVE_UINT16_T],[1],[])])
AC_CHECK_TYPE(uint8_t,	[AC_DEFINE([HAVE_UINT8_T],[1],[])])

dnl Solaris also needs libnsl for inet_ntoa and friends.
######### libnsl ##############################################################
# Get libnsl include locations
AC_ARG_WITH([libnsl-include-path],
  [AS_HELP_STRING([--with-libnsl-include-path],[location of the libnsl headers, defaults to /usr/local/include])],
  [LIBNSL_CFLAGS="-I$withval"],
  [LIBNSL_CFLAGS='-I/usr/local/include'])
AC_SUBST([LIBNSL_CFLAGS])

# Get libnsl library locations
AC_ARG_WITH([libnsl-lib-path],
  [AS_HELP_STRING([--with-libnsl-lib-path], [location of the libnsl libraries])],
  [LIBNSL_LIBS="-L$withval -lnsl"],
  [LIBNSL_LIBS='-L/usr/local/include -lnsl '])
AC_SUBST([LIBNSL_LIBS])

# --with-libtool-sysroot, --disable-static

AC_CHECK_FUNCS(socket strstr strtol strtoul gethostbyname gethostbyname2 inet_ntop getipnodebyaddr getaddrinfo)

AC_CHECK_HEADERS(getopt.h,[AC_DEFINE(HAVE_GETOPT_H, 1, getopt_h)])
dnl Check if getopt_long is available.
AC_CHECK_FUNCS([getopt_long],[],[AC_MSG_ERROR([getopt_long is missing!])])

dnl Check if we need netdb.h for resolver routines.
AC_DEFUN([sii_GETADDRINFO],
AC_MSG_CHECKING(for getaddrinfo in netdb.h)
AC_TRY_COMPILE(
[
  #include <netdb.h>
],
[struct addrinfo h; struct addrinfo *r; getaddrinfo("127.0.0.1", NULL, &h, &r)],
[
  AC_MSG_RESULT(yes)
  AC_DEFINE(HAVE_GETADDRINFO_NETDB, 1, getaddrinfo_netdb)
],
AC_MSG_RESULT(no)))


AC_DEFUN([sii_GETHOSTBYNAME],
	AC_MSG_CHECKING(for gethostbyname in netdb.h)
	AC_TRY_COMPILE(
		[#include <netdb.h>],
		[struct hostent *h; h=gethostbyname("127.0.0.1")],
		[
			AC_MSG_RESULT(yes)
			AC_DEFINE(HAVE_GETHOSTBYNAME_NETDB, 1, gethostbyname_netdb)
		],
		AC_MSG_RESULT(no)
	)
)

AC_DEFUN([sii_GETHOSTBYNAME2],
	AC_MSG_CHECKING(for gethostbyname2 in netdb.h)
	AC_TRY_COMPILE(
		[#include <netdb.h>],
		[struct hostent *h; h=gethostbyname("127.0.0.1", AF_INET)],
		[
			AC_MSG_RESULT(yes)
			AC_DEFINE(HAVE_GETHOSTBYNAME2_NETDB, 1, gethostbyname2_netdb)
		],
		AC_MSG_RESULT(no)
	)
)

dnl Solaris has socket(2) in libsocket.
AC_CHECK_LIB(socket, socket,
	[
		AC_DEFINE(HAVE_LIBSOCKET, 1, libsocket)
		LIBS="$LIBS -lsocket"
	]
)


sii_GETHOSTBYNAME
sii_GETHOSTBYNAME2
sii_GETADDRINFO

dnl Check if struct sockaddr has member sa_len

AC_MSG_CHECKING(if struct sockaddr has sa_len field)
AC_CHECK_MEMBER(struct sockaddr.sa_len,
                AC_DEFINE(HAVE_SA_LEN, 1, [Define this if your stack has sa_len in sockaddr struct.]),,
                [#ifdef HAVE_SYS_TYPES_H
                 #include <sys/types.h>
                 #endif
#include <sys/socket.h>])

AC_SUBST([CFLAGS])

AC_OUTPUT(Makefile)

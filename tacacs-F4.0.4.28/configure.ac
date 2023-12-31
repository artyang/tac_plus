# Copyright (c) Facebook, Inc. and its affiliates. All Rights Reserved

dnl Process this file with autoconf to produce a configure script.
dnl A configure script is provided, in cause you do not have autoconf.

AC_PREREQ(2.69)

dnl VERSION needs to be updated in version.h.in such that 'make dist'
dnl uses the correct filename for the directory name and tarball and binaries
dnl get the right version numbers.
AC_INIT([tacacs], 
 m4_esyscmd_s([sed -n 's/.*version.*"\(.*\)".*/\1/p' version.h.in]))

AC_CONFIG_AUX_DIR([aconf])
AC_CONFIG_MACRO_DIR([aconf])
AM_INIT_AUTOMAKE

AM_MAINTAINER_MODE()

dnl AC_CONFIG_SUBDIRS(etc man share)

# what OS
dnl ---- XXX: these really should deal with the individual reasons why
dnl      linux/whatever is different, rather than a blanket stmt
dnl is this crack, i mean linux?
AH_TEMPLATE([AIX], [define this if your o/s is AIX])
AH_TEMPLATE([FREEBSD], [define this if your o/s is FreeBSD])
AH_TEMPLATE([NETBSD], [define this if your o/s is NetBSD])
AH_TEMPLATE([SOLARIS], [define this if your o/s is Solaris])
AH_TEMPLATE([HPUX], [define this if your o/s is HPux])
AH_TEMPLATE([LINUX], [define this if your o/s is Linux])
AH_TEMPLATE([MIPS], [define this if your o/s is MIPS])
AC_CANONICAL_HOST
case "${host_os}" in
    *aix* )
	# For AIX
	echo "See /usr/lpp/bos/bsdport on your system for details of how " \
		"to define bsdcc."
	CC=bsdcc; export CC
	# CPPFLAGS="$CFLAGS -I/usr/local/include"; export CPPFLAGS
	# LDFLAGS="$LDFLAGS -L/usr/local/lib"; export LDFLAGS
	# LIBS="-lcrypt $LIBS"; export LIBS
	AC_DEFINE([AIX])
    ;;
    *freebsd* )
	#CPPFLAGS="$CFLAGS -I/usr/pkg/include"; export CPPFLAGS
	#LDFLAGS="$LDFLAGS -L/usr/pkg/lib -Xlinker -rpath -Xlinker /usr/pkg/lib"
	#export LDFLAGS
	LIBS="-lcrypt $LIBS"; export LIBS
	AC_DEFINE(FREEBSD)
    ;;
    *netbsd* )
	#CPPFLAGS="$CFLAGS -I/usr/pkg/include"; export CPPFLAGS
	#LDFLAGS="$LDFLAGS -L/usr/pkg/lib -Xlinker -rpath -Xlinker /usr/pkg/lib"
	#export LDFLAGS
	LIBS="-lcrypt $LIBS"; export LIBS
	AC_DEFINE(NETBSD)
    ;;
    *solaris* )
	#CPPFLAGS="$CFLAGS -I/usr/local/include"; export CPPFLAGS
	#LDFLAGS="$LDFLAGS -L/usr/local/lib"; export LDFLAGS
	LIBS="-lnsl -lsocket"; export LIBS
	AC_DEFINE(SOLARIS)
    ;;
    *hpux* )
	# For HP/UX
	# CPPFLAGS="$CFLAGS -I/usr/local/include"; export CPPFLAGS
	# LDFLAGS="$LDFLAGS -L/usr/local/lib"; export LDFLAGS
	# LIBS="-lcrypt $LIBS"; export LIBS
	AC_DEFINE(HPUX)
    ;;
    *linux* )
	# XXX: not sure if /usr/local is necessary.
	# XXX: linux libwrap needs -lnsl. configure should check for
	#      existence of libnsl instead of hard-coding
	CPPFLAGS="$CFLAGS -I/usr/local/include"; export CPPFLAGS
	LDFLAGS="$LDFLAGS -L/usr/local/lib -L/lib"; export LDFLAGS
	LIBS="-lcrypt $LIBS"; export LIBS
	AC_DEFINE(LINUX)

	# XXX: does linux need glibc: -DGLIBC
    ;;
    *mips* )
	CPPFLAGS="$CFLAGS -I/usr/local/include"; export CPPFLAGS
	LDFLAGS="$LDFLAGS -L/usr/local/lib"; export LDFLAGS
	LIBS="-lcrypt $LIBS"; export LIBS
	AC_DEFINE(MIPS)
    ;;
    * )
	CPPFLAGS="$CFLAGS -I/usr/local/include"; export CPPFLAGS
	LDFLAGS="$LDFLAGS -L/usr/local/lib"; export LDFLAGS
    ;;
esac

##################

#PATH=${PATH}:/usr/local/bin

dnl default install location
AC_PREFIX_DEFAULT(/usr/local)

dnl this doesnt work well if the program has not been installed before.
dnl # guess the --prefix setting
dnl AC_PREFIX_PROGRAM(tac_plus)

# make sure MAKE sets ${MAKE}
AC_PATH_PROG(MAKE,gmake,no)
if test $MAKE = no; then
    unset ac_cv_path_MAKE
    AC_PATH_PROG(MAKE,make,no)
    if test $MAKE = no; then
	AC_MSG_ERROR([can't locate a make.])
	exit 1
    fi
fi
AC_PROG_MAKE_SET()

AM_MAINTAINER_MODE()

dnl AC_DISABLE_SHARED()
dnl use libtool, not ranlib
dnl AC_PROG_RANLIB
AC_LIBTOOL_DLOPEN()
AC_PROG_LIBTOOL()

ACX_PTHREAD([CC=$PTHREAD_CC; CFLAGS="$CFLAGS $PTHREAD_CFLAGS";
	    LIBS="$PTHREAD_LIBS $LIBS"
	    AC_DEFINE(HAVE_PTHREAD)])

# compiler specifics
AC_PROG_CC
AC_PROG_CPP
AC_C_CONST
AC_C_INLINE
AC_C_STRINGIZE

# compiler compiler specifics
AM_PROG_LEX
dnl XXX sets LEX and LEXLIB == -fl and YYTEXT?
# see if 'lex' is flex in disguise
if test "$LEX" != "flex" ; then
	AC_MSG_CHECKING([whether lex is flex in disguise])
	# this may not be a valid way to test for flex, but it's cheap.
	$LEX --version > /dev/null 2>&1
	if test $? -ne 0 ; then
		AC_MSG_RESULT()
		AC_MSG_ERROR([tac_plus requires gnu flex.  sorry])
	fi
	AC_MSG_RESULT(yes)
fi
AC_PROG_YACC
# see if 'yacc' is bison in disguise
if test "$YACC" != "bison" ; then
	AC_MSG_CHECKING([whether yacc is bison in disguise])
	# this may not be a valid way to test for bison, but it's cheap.
	$YACC --version > /dev/null 2>&1
	if test $? -ne 0 ; then
		AC_MSG_RESULT()
		AC_MSG_ERROR([tac_plus requires gnu bison.  sorry])
	fi
	AC_MSG_RESULT(yes)
fi

# platform specifics
AC_WORDS_BIGENDIAN
AC_LONG_64_BITS

AC_PROG_INSTALL

dnl configure options
dnl
dnl debug - aka compiler symbols
dnl
AC_MSG_CHECKING(whether to include symbols)
AH_TEMPLATE(DBG, [define this to include debugging support])
AC_ARG_ENABLE(debug,
    AS_HELP_STRING([--enable-debug],[include compiler symbols]),
[ case "$enable_debug" in
  no)
    AC_MSG_RESULT(no)
    ;;
  yes)
    AC_MSG_RESULT(yes)
    DBG="-g"
    AC_DEFINE(DBG)
    ;;
  *)
    AC_MSG_RESULT(no)
    ;;
  esac ],
  # ie: no --{enable,disable}-debug option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(DBG)
dnl
dnl warn - aka gcc warnings
dnl
dnl             XXX: this should only be set for gcc....
AC_MSG_CHECKING(whether to set gcc warnings)
AH_TEMPLATE(WARN, [define this to set pedantic gcc warnings])
AC_ARG_ENABLE(warn,
    AS_HELP_STRING([--enable-warn],[pedantic gcc warnings]),
[ case "$enable_debug" in
  no)
    AC_MSG_RESULT(no)
    WARN=""
    AC_DEFINE(WARN)
    ;;
  yes)
    AC_MSG_RESULT([yes -Wall])
    WARN="-Wall"
    AC_DEFINE(WARN)
    ;;
  *)
    AC_MSG_RESULT(no)
    WARN=""
    AC_DEFINE(WARN)
    ;;
  esac ],
  # ie: no --{enable,disable}-warn option, withval == ""
  WARN=""
  AC_DEFINE(WARN)
  AC_MSG_RESULT(no)
)
AC_SUBST(WARN)

dnl
dnl libwarp - aka tcp_wrappers
dnl hijacked this from ssh, but mimiced the '*' "clause" for 'yes'
dnl
AC_MSG_CHECKING(whether to use libwrap)
AH_TEMPLATE(LIBWRAP, [define this to include libwrap (tcp_wrappers) support])
AH_TEMPLATE(HAVE_LIBWRAP, [])
AC_ARG_WITH(libwrap,
    AS_HELP_STRING([--with-libwrap[[=PATH]]],[libwrap (tcp_wrappers) support.  PATH is dir above lib, eg: /usr/local. (default)]),
[ case "$withval" in
  no)
    AC_MSG_RESULT(no)
    ;;
  yes)
    AC_MSG_RESULT(yes)
    AC_DEFINE(LIBWRAP)
    WRAPLIBS="-lwrap"
    OLDLIBS="$LIBS"
    LIBS="$WRAPLIBS $LIBS"
    AC_TRY_LINK([ int allow_severity; int deny_severity; ],
		[ hosts_access(); ],
	[AC_DEFINE(LIBWRAP)
	 WRAPLIBS="-lwrap"
	 AC_DEFINE(HAVE_LIBWRAP) ],
	[AC_MSG_ERROR(Could not find libwrap.  You must first install tcp_wrappers.) ])
    LIBS="$OLDLIBS"
    ;;
  *)
    AC_MSG_RESULT(yes)
    AC_DEFINE(LIBWRAP)
    if test -d "$withval"; then
	WRAPINCS="-I$withval/include"
	WRAPLIBS="-L$withval/lib -lwrap -R$withval/lib"
    else
	WRAPLIBS="$withval"
    fi
    OLDLIBS="$LIBS"
    OLDINCS="$INCLUDES"
    LIBS="$WRAPLIBS $LIBS"
    INCLUDES="$WRAPINCS"
    AC_TRY_LINK([ int allow_severity; int deny_severity; ],
		[ hosts_access(); ],
		[],
		[ AC_MSG_ERROR(Could not find libwrap.  You must first install tcp_wrappers.) ])
    LIBS="$OLDLIBS"
    INCLUDES="$OLDINCS"
    ;;
  esac ],
		# XXX: is "no" correct?
  AC_MSG_RESULT(yes)
  AC_DEFINE(LIBWRAP)
  WRAPLIBS="-lwrap"
  OLDLIBS="$LIBS"
  LIBS="$WRAPLIBS $LIBS"
  AC_TRY_LINK([ int allow_severity; int deny_severity; ],
	[ hosts_access(); ],
	[AC_DEFINE(LIBWRAP)
	 WRAPLIBS="-lwrap"
	 AC_DEFINE(HAVE_LIBWRAP) ],
	[AC_MSG_ERROR(Could not find libwrap.  You must first install tcp_wrappers.) ])
  LIBS="$OLDLIBS"
)
AC_SUBST(WRAPINCS)
AC_SUBST(WRAPLIBS)

dnl
dnl skey - aka One Time Password mechanism
dnl
AC_MSG_CHECKING([whether to include skey support])
AH_TEMPLATE(SKEY, [define this to include support for skey])
AC_ARG_WITH(skey,
    AS_HELP_STRING([--with-skey[[=PATH]]],[libskey (skey) support.  PATH is dir above lib, eg: /usr/local]),
[ case "$withval" in
  no)
    AC_MSG_RESULT(no)
    ;;
  yes)
    AC_MSG_RESULT(yes)
    AC_DEFINE(SKEY)
    AC_SEARCH_LIBS([skeychallenge], [skey],
	[ ],
	[AC_MSG_ERROR(Could not find libskey.  You must first install skey or provide a hint to the location of library and includes, as in --with-skey=/usr/local.)])
    ;;
  *)
    AC_MSG_RESULT(yes)
    AC_DEFINE(SKEY)
    if test -d "$withval" ; then
	LDFAGS="$LDFLAGS -L$withval/lib"
	CFLAGS="$CFLAGS -I$withval/include"
	AC_SEARCH_LIBS([skeychallenge], [skey],
	    [ ],
	    [AC_MSG_ERROR([Could not find libskey.])
	])
    else
	AC_SEARCH_LIBS([skeychallenge], [$withval],
	    [ ],
	    [AC_MSG_ERROR([Could not find lib$withval.])
	])
    fi
    ;;
  esac ],
  AC_MSG_RESULT(no)
  with_skey="no"
)
AC_SUBST(SKEY)
AM_CONDITIONAL([TACSKEY], [test "${with_skey}" != "no"])

dnl
dnl aceclnt - aka RSA SecurID
dnl
AC_MSG_CHECKING([whether to include RSA SecurID support])
AH_TEMPLATE(ACECLNT, [define this to include support for RSA SecurID])
AC_ARG_WITH(aceclnt,
	AS_HELP_STRING([--with-aceclnt[[=PATH]]],[libaceclnt (RSA SecurID) support. PATH is dir above lib, eg: /usr/local]),
[ case "$withval" in
  no)
    AC_MSG_RESULT(no)
    ;;
  yes)
    AC_MSG_RESULT(yes)
    AC_DEFINE(ACECLNT)
    AC_SEARCH_LIBS([SD_Init], [aceclnt],
	[ ],
	[AC_MSG_ERROR(Could not find libaceclnt.  You must first install aceclnt or provide a hint to the location of library and includes, as in --with-aceclnt=/usr/local)])
    ;;
  *)
    AC_MSG_RESULT(yes)
    AC_DEFINE(ACECLNT)
    if test -d "$withval" ; then
	LDFAGS="$LDFLAGS -L$withval/lib"
	CFLAGS="$CFLAGS -I$withval/include"
	AC_SEARCH_LIBS([SD_Init], [aceclnt],
	    [ ],
	    [AC_MSG_ERROR([Could not find libaceclnt.])
	])
    else
	AC_SEARCH_LIBS([SD_Init], [$withval],
	    [ ],
	    [AC_MSG_ERROR([Could not find lib$withval.])
	])
    fi
    ;;
  esac ],
  AC_MSG_RESULT(no)
  with_aceclnt="no"
)
AC_SUBST(ACECLNT)
AM_CONDITIONAL([TACACECLNT], [test "${with_aceclnt}" != "no"])

dnl
dnl		XXX: might be good to have these as config file options
dnl		     or just options for running scripts
dnl userid - aka TACPLUS_USERID
dnl
AC_MSG_CHECKING([whether to setuid()])
AH_TEMPLATE(TACPLUS_USERID, [define this to a UID for setuid() at run-time])
AC_ARG_WITH(userid,
    AS_HELP_STRING([--with-userid=UID],[tacacs will setuid(UID) after it binds the tcp port]),
[ case "$withval" in
  no)
    AC_MSG_RESULT(no)
    ;;
  yes)
    AC_MSG_ERROR([--with-userid requires a UID argument.])
    ;;
  *)
    expr $withval + 1 > /dev/null 2>&1
    if test $? != 0  ; then
	AC_MSG_ERROR([--with-userid requires a numeric UID argument.])
    fi
    AC_MSG_RESULT($withval)
    AC_DEFINE_UNQUOTED(TACPLUS_USERID, $withval)
    ;;
  esac ],
  # ie: no --{with,without}-userid option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(TACPLUS_USERID)

dnl
dnl groupid - aka TACPLUS_GROUPID
dnl
AC_MSG_CHECKING(whether to setgid())
AH_TEMPLATE(TACPLUS_GROUPID, [define this to a GID for setgid() at run-time])
AC_ARG_WITH(groupid,
    AS_HELP_STRING([--with-groupid=UID],[tacacs will setgid(GID) after it binds the tcp port]),
[ case "$withval" in
  no)
    AC_MSG_RESULT(no)
    ;;
  yes)
    AC_MSG_ERROR([--with-groupid requires a GID argument.])
    ;;
  *)
    expr $withval + 1 > /dev/null 2>&1
    if test $? != 0  ; then
	AC_MSG_ERROR([--with-groupid requires a numeric GID argument.])
    fi
    AC_MSG_RESULT($withval)
    AC_DEFINE_UNQUOTED(TACPLUS_GROUPID, $withval)
    ;;
  esac ],
  # ie: no --{with,without}-userid option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(TACPLUS_GROUPID)

dnl
dnl ACLs - aka tacacs config ACLs
dnl
AC_MSG_CHECKING(whether to include ACL support)
AH_TEMPLATE(ACLS, [define this to include ACL support])
AC_ARG_ENABLE(acls,
    AS_HELP_STRING([--enable-acls],[tacacs config ACL support (default)]),
[ case "$enable_acls" in
  no)
    AC_MSG_RESULT(no)
    use_acls=0
    ;;
  yes)
    AC_MSG_RESULT(yes)
    AC_DEFINE(ACLS)
    use_acls=1
    ;;
  *)
    AC_MSG_RESULT(yes)
    AC_DEFINE(ACLS)
    use_acls=1
    ;;
  esac ],
  # ie: no --{enable,disable}-acls option, withval == ""
  AC_MSG_RESULT(yes)
  AC_DEFINE(ACLS)
  use_acls=1
)
AC_SUBST(ACLS)

dnl
dnl UENABLE - aka tacacs config user enable.  UENABLE requires ACLS.
dnl
AC_MSG_CHECKING(whether to include user-enable support)
AH_TEMPLATE(UENABLE, [define this to include user-specific enable password support])
AC_ARG_ENABLE(uenable,
    AS_HELP_STRING([--enable-uenable],[tacacs config per-user enable support (default)]),
[ case "$enable_uenable" in
  no)
    AC_MSG_RESULT(no)
    use_uenable=0
    ;;
  yes | *)
    AC_MSG_RESULT(yes)
    AC_DEFINE(UENABLE)
    use_uenable=1
    ;;
  esac ],
  # ie: no --{enable,disable}-uenable option, withval == ""
  AC_MSG_RESULT(yes)
  AC_DEFINE(UENABLE)
  use_uenable=1
)
AC_SUBST(UENABLE)
if test $use_acls -eq 0 -a $use_uenable -eq 1; then
    AC_MSG_WARN([unenable (user enable) option requires the acls option.])
    AC_DEFINE(ACLS)
    AC_SUBST(ACLS)
fi

dnl
dnl MAXSESS - Enforce a limit on maximum sessions per user
dnl
AC_MSG_CHECKING(whether to include maximum sessions (maxsess) support)
AH_TEMPLATE(MAXSESS, [define this to include MAXSESS support to enforce a limit on maximum sessions per user ])
AC_ARG_ENABLE(maxsess,
    AS_HELP_STRING([--enable-maxsess],[Enforce a limit on maximum sessions per user]),
[ case "$enable_maxsess" in
  no)
    AC_MSG_RESULT(no)
    ;;
  yes)
    AC_MSG_RESULT(yes)
    AC_DEFINE(MAXSESS)
    ;;
  *)
    AC_MSG_RESULT(no)
    ;;
  esac ],
  # ie: no --{enable,disable}-maxsess option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(MAXSESS)
dnl
dnl ENABLE_FINGER - use finger(1) to check number of sessions a user has on
dnl                 a NAS.
dnl
AC_MSG_CHECKING(whether to include maxsess finger support)
AH_TEMPLATE(MAXSESS_FINGER, [define this to include support to finger NASes for
the number of sessions a user is using])
AC_ARG_ENABLE(finger,
    AS_HELP_STRING([--enable-finger],[finger NAS for number of sessions a user is using]),
[ case "$enable_finger" in
  no)
    AC_MSG_RESULT(no)
    use_finger=0
    ;;
  yes)
    AC_MSG_RESULT(yes)
    AC_DEFINE(MAXSESS_FINGER)
    use_finger=1
    ;;
  *)
    AC_MSG_RESULT(no)
    ;;
  esac ],
  # ie: no --{enable,disable}-finger option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(MAXSESS_FINGER)
dnl
dnl ARAP_DES - enable DES for ARAP
dnl
AC_MSG_CHECKING(whether to include ARAP DES support)
AH_TEMPLATE(ARAP_DES, [Define this if you have DES routines you can link to for ARAP (See the user guide for more details)])
AC_ARG_ENABLE(arapdes,
    AS_HELP_STRING([--enable-arapdes],[enable DES for ARAP]),
[ case "$enable_arapdes" in
  yes)
    AC_MSG_RESULT(yes)
    AC_DEFINE(ARAP_DES)
    ;;
  * | no)
    AC_MSG_RESULT(no)
    ;;
  esac ],
  # ie: no --{enable,disable}-arapdes option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(ARAP_DES)
dnl
dnl MSCHAP - enable MSCHAP
dnl
AC_MSG_CHECKING(whether to include MSCHAP support)
AH_TEMPLATE(MSCHAP, [Define this if you need MSCHAP support])
AC_ARG_ENABLE(mschap,
    AS_HELP_STRING([--enable-mschap],[enable MSCHAP]),
[ case "$enable_mschap" in
  yes)
    AC_CHECK_HEADER(mschap_.h)
    AC_MSG_RESULT(yes)
    AC_DEFINE(MSCHAP)
    ;;
  * | no)
    AC_MSG_RESULT(no)
    ;;
  esac ],
  # ie: no --{enable,disable}-mschap option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(MSCHAP_)
dnl
dnl MSCHAP_DES - enable DES for MSCHAP
dnl
AC_MSG_CHECKING(whether to include MSCHAP DES support)
AH_TEMPLATE(MSCHAP_DES, [Define this if you have DES routines you can link to for MSCHAP (See the user guide for more details)])
AC_ARG_ENABLE(mschapdes,
    AS_HELP_STRING([--enable-mschapdes],[enable DES for MSCHAP]),
[ case "$enable_mschapdes" in
  yes)
    AC_CHECK_HEADER(mschap_des.h)
    AC_MSG_RESULT(yes)
    AC_DEFINE(MSCHAP_DES)
    ;;
  * | no)
    AC_MSG_RESULT(no)
    ;;
  esac ],
  # ie: no --{enable,disable}-mschapdes option, withval == ""
  AC_MSG_RESULT(no)
)
AC_SUBST(MSCHAP_DES)

dnl
dnl pid file location
dnl
AC_MSG_CHECKING(for alt pid file FQPN)
if test -d /var/run; then
    TACPLUS_PIDFILE="/var/run/tac_plus.pid"
else
    TACPLUS_PIDFILE="/etc/tac_plus.pid"
fi
AC_ARG_WITH(pidfile,
    AS_HELP_STRING([--with-pidfile=PATH],[alternate pidfile FQPN]),
[ case "$withval" in
  *)
    AC_MSG_RESULT($withval)
    TACPLUS_PIDFILE=$withval
    ;;
  esac ],
  AC_MSG_RESULT($TACPLUS_PIDFILE)
)
AC_SUBST(TACPLUS_PIDFILE)

dnl
dnl default accounting file location
dnl
AC_MSG_CHECKING(for alt accounting file FQPN)
if test -d /var/log; then
    TACPLUS_ACCTFILE="/var/log/tac_plus.acct"
else
    TACPLUS_ACCTFILE="/var/tmp/tac_plus.acct"
fi
AC_ARG_WITH(acctfile,
    AS_HELP_STRING([--with-acctfile=PATH],[alternate accounting file FQPN]),
[ case "$withval" in
  *)
    AC_MSG_RESULT($withval)
    TACPLUS_ACCTFILE=$withval
    ;;
  esac ],
  AC_MSG_RESULT($TACPLUS_ACCTFILE)
)
AC_SUBST(TACPLUS_ACCTFILE)

dnl
dnl default log file location
dnl
AC_MSG_CHECKING(for alt log file FQPN)
if test -d /var/log; then
    TACPLUS_LOGFILE="/var/log/tac_plus.log"
else
    TACPLUS_LOGFILE="/var/tmp/tac_plus.log"
fi
AC_ARG_WITH(logfile,
    AS_HELP_STRING([--with-logfile=PATH],[alternate log file FQPN]),
[ case "$withval" in
  *)
    AC_MSG_RESULT($withval)
    TACPLUS_LOGFILE=$withval
    ;;
  esac ],
  AC_MSG_RESULT($TACPLUS_LOGFILE)
)
AC_SUBST(TACPLUS_LOGFILE)

dnl
dnl default wholog file location
dnl
AC_MSG_CHECKING(for alt wholog file FQPN)
if test -d /var/log; then
    TACPLUS_WHOLOGFILE="/var/log/tacwho.log"
else
    TACPLUS_WHOLOGFILE="/var/tmp/tacwho.log"
fi
AC_ARG_WITH(whologfile,
    AS_HELP_STRING([--with-whologfile=PATH],[alternate wholog file FQPN]),
[ case "$withval" in
  *)
    AC_MSG_RESULT($withval)
    TACPLUS_WHOLOGFILE=$withval
    ;;
  esac ],
  AC_MSG_RESULT($TACPLUS_WHOLOGFILE)
)
AC_SUBST(TACPLUS_WHOLOGFILE)

dnl
dnl profiling
dnl
AC_MSG_CHECKING(whether to profile)
AH_TEMPLATE(PROFILE, [define this to include profiling])
AC_ARG_WITH(prof,
    AS_HELP_STRING([--with-prof],[Compile in profiling.]),
[ case "$withval" in
  yes)
    AC_MSG_RESULT(yes)
    #AC_DEFINE(PROF)
    PROFLAGS="-pg"; export PROFLAGS
    PROFLIBS="-lc_p -lc"; export PROFLIBS
    OLDCFLAGS="$CFLAGS"
    OLDLIBS="$LIBS"
    CFLAGS="$PROFLAGS $CFLAGS"
    LIBS="$PROFLIBS $LIBS"
    AC_TRY_LINK([ ],
		[ moncontrol(0); ],
	[AC_DEFINE(PROFILE) ],
		[ AC_MSG_ERROR(Could not compile with -pg.) ])
    CFLAGS="$OLDCFLAGS"
    LIBS="$OLDLIBS"
    ;;
  *)
    AC_MSG_RESULT(no)
    ;;
  esac ],
  AC_MSG_RESULT(no)
)
AC_SUBST(PROFLAGS)
AC_SUBST(PROFLIBS)

# look for PAM
AH_TEMPLATE(HAVE_PAM, [define if your system has libpam])
AC_CHECK_LIB([pam], [pam_start],
	[AC_DEFINE(HAVE_PAM)
	 LIBS="-lpam $LIBS"])

# check includes/headers
AC_HEADER_STDC
AC_HEADER_TIME
AC_CHECK_HEADERS(crypt.h ctype.h errno.h fcntl.h malloc.h shadow.h stdlib.h \
		stdint.h string.h strings.h sys/resource.h sys/socket.h \
		sys/types.h sys/wait.h sysexits.h syslog.h termios.h unistd.h \
		utmp.h utmpx.h wait.h)

AH_TEMPLATE([SHADOW_PASSWORDS],
	    [define if your system has a shadow password file])
if test $ac_cv_header_shadow_h = yes ; then
    AC_DEFINE(SHADOW_PASSWORDS)
fi

# type checks
dnl AC_TYPE_MODE_T
dnl AC_TYPE_OFF_T
dnl AC_TYPE_PID_T
AC_TYPE_SIGNAL
dnl AC_TYPE_SIZE_T
# Do we have socklen_t definition?
AC_CHECK_TYPES([socklen_t], [], [], [#if HAVE_SYS_TYPES_H
# include <sys/types.h>
#endif
#if HAVE_SYS_SOCKET_H
# include <sys/socket.h>
#endif])
# Do we have pid_t definition?
AC_CHECK_TYPES([pid_t], [], [], [#if HAVE_SYS_TYPES_H
# include <sys/types.h>
#endif
#if HAVE_UNISTD_H
# include <unistd.h>
#endif])

# check functions
AC_CHECK_FUNCS([getdtablesize memcpy memset random strchr strcspn strerror \
		strrchr wait3 wait4 waitpid])
AC_FUNC_SETPGRP

# Is the wait(2) status an int or union
AH_TEMPLATE([UNIONWAIT],
	    [define this if your waitpid() takes an union wait status pointer])
AC_MSG_CHECKING([if waitpid takes a union wait])
AC_TRY_COMPILE([#if HAVE_SYS_WAIT_H
# include <sys/wait.h>
#endif
#if HAVE_WAIT_H
# include <wait.h>
#endif], [union wait status;
int pid;
pid = wait (&status);
#ifdef WEXITSTATUS
/* Some POSIX systems have both the new-style macros and the old
   union wait type, and they do not work together.  If union wait
   conflicts with WEXITSTATUS et al, we dont want to use it.
 */
if (WEXITSTATUS(status) != 0)
	pid = -1;
#ifdef WTERMSIG
	/* If we have WEXITSTATUS and WTERMSIG, just use them on ints. */
	-- blow chunks here --
#endif
#endif
#ifdef HAVE_WAITPID
	/* Make sure union wait works with waitpid.  */
	pid = waitpid(-1, &status, 0);
#endif],
[AC_MSG_RESULT(yes)
 AC_DEFINE(UNIONWAIT)],
[AC_MSG_RESULT(no)])

# Check for re-arming signal
AH_TEMPLATE([REARMSIGNAL],
	    [define this if you find that your daemon quits after being sent
	     more than one SIGUSR1. Some systems need to explicitly re-arm
	     signals after they've been used once])
AC_MSG_CHECKING([if signals need to be re-armed])
AC_TRY_RUN([#include <signal.h>
#if HAVE_STDLIB_H
#include <stdlib.h>
#endif
#if HAVE_UNISTD_H
#include <unistd.h>
#endif
int hit = 0;
RETSIGTYPE
handler(n)
int n;
{
	hit++;
}
int main()
{
	signal(SIGUSR1, handler);
	kill(getpid(), SIGUSR1);
	kill(getpid(), SIGUSR1);
	if (hit == 2)
	    exit(0);
	else
	    exit(114);
}],
[AC_MSG_RESULT(no)],
[AC_MSG_RESULT(yes);AC_DEFINE(REARMSIGNAL)],
[AC_MSG_WARN([tac_plus may be less efficient when cross-compiled])
 AC_DEFINE(REARMSIGNAL)],
)

# Check for need to reap children when the default is to ignore SIGCHLD
AH_TEMPLATE([REAPCHILD],
	    [define this if your o/s needs children reaped even though the
	     SIGCHLD default is SIG_IGN])
AH_TEMPLATE([REAPSIGIGN],
	    [define this if your o/s needs children reaped even with an
	     explicit SIG_IGN])
AC_MSG_CHECKING([if children need to be reaped])
AC_TRY_RUN([#include <signal.h>
#if HAVE_STDLIB_H
#include <stdlib.h>
#endif
#if HAVE_UNISTD_H
#include <unistd.h>
#endif
#if HAVE_SYS_WAIT_H
#include <sys/wait.h>
#endif
#if HAVE_SYS_RESOURCE_H
#include <sys/resource.h>
#endif
#include <errno.h>
pid_t child, pid;
int main()
{
	int status;
	child = vfork();
	if (child == 0)
		exit(1);
#if HAVE_WAIT4
	pid = wait4(child, &status, WNOHANG, NULL);
#else
	do {
		pid = wait3(&status, WNOHANG, NULL);
	} while (pid != child && pid != -1);
#endif
	if (pid == -1 && errno == ECHILD)
		exit(0);
	exit(114);
}],
[AC_MSG_RESULT(no)],
[AC_MSG_RESULT(yes)
 AC_DEFINE(REAPCHILD)
 # try again with SIG_IGN
 AC_MSG_CHECKING([if children need to be reaped with SIG_IGN])
 AC_TRY_RUN([#include <signal.h>
#if HAVE_STDLIB_H
#include <stdlib.h>
#endif
#if HAVE_UNISTD_H
#include <unistd.h>
#endif
#if HAVE_SYS_WAIT_H
#include <sys/wait.h>
#endif
#if HAVE_SYS_RESOURCE_H
#include <sys/resource.h>
#endif
#include <errno.h>
pid_t child, pid;
int main()
{
	int status;
	signal(SIGCHLD, SIG_IGN);
	child = vfork();
	if (child == 0)
		exit(1);
#if HAVE_WAIT4
	pid = wait4(child, &status, WNOHANG, NULL);
#else
	do {
		pid = wait3(&status, WNOHANG, NULL);
	} while (pid != child && pid != -1);
#endif
	if (pid == -1 && errno == ECHILD)
		exit(0);
	exit(114);
}],
 [AC_MSG_RESULT(no)],
 [AC_MSG_RESULT(yes);AC_DEFINE(REAPSIGIGN)])],
[AC_MSG_WARN([tac_plus may be less efficient when cross-compiled])
 AC_DEFINE(REAPCHILD)],
)

# Find an appropriate tar for use in "dist" targets.  A "best guess"
# is good enough -- if we can't find GNU tar, we don't really care.
AC_CHECK_PROGS(TAR, gnutar gtar tar)

AC_SUBST(INST_PROGS)
INST_PROGS=$progs

AC_SUBST(CFLAGS)
AC_SUBST(CPPFLAGS)
AC_SUBST(LDFLAGS)
dnl AC_SUBST(PG_LDFLAGS)
dnl AC_SUBST(PG_CPPFLAGS)

# i did this so that i could end up w/ a #define for the config file */
if test "x$prefix" = xNONE; then
    prefix_save=$prefix
    prefix=$ac_default_prefix
    MYSYSCONFDIR=`eval echo $sysconfdir`
    prefix=$prefix_save
else
    MYSYSCONFDIR=`eval echo $sysconfdir`
fi
AC_SUBST(MYSYSCONFDIR)

# autoheader bits
AH_TOP([
#ifndef CONFIG_H
#define CONFIG_H	1
])
AH_BOTTOM([
/* UENABLE requires ACLS */
#ifdef UENABLE
# ifndef ACLS
#  define ACLS	1
# endif
#endif

/* MAXSESS_FINGER requires MAXSESS */
#ifdef MAXSESS_FINGER
# ifndef MAXSESS
#  define MAXSESS	1
# endif
#endif

/* Some, eg: solaris 2.6, dont have socklen_t */
#ifndef HAVE_SOCKLEN_T
# define socklen_t	int
#endif

/* host specifics */
/* Define this if your password file does not contain age and comment fields. */
#define NO_PWAGE

#if AIX
/*
 * The only way to properly compile BSD stuff on AIX is to define a
 * "bsdcc" compiler on your system.  See /usr/lpp/bos/bsdport on your
 * system for details. People who do NOT do this tell me that the code
 * still compiles but that it then doesn't behave correctly e.g. child
 * processes are not reaped correctly.  Don't expect much sympathy if
 * you do this.
 */
# define _BSD 1
# define _BSD_INCLUDES
# define NO_PWAGE
#endif	/* AIX */

#if LINUX
# define NO_PWAGE
# include <unistd.h>
# ifdef GLIBC
#  define CONST_SYSERRLIST
# endif
#endif	/* LINUX */

#if NETBSD
# define NO_PWAGE
# define CONST_SYSERRLIST
#endif

#if FREEBSD
# define CONST_SYSERRLIST
# define NO_PWAGE
#endif

#if BSDI
# define NO_PWAGE
#endif
#endif /* CONFIG_H */
])
AC_CONFIG_HEADERS(config.h)
AC_CONFIG_FILES(Makefile version.h pathsl.h tac_plus.8 tac_plus.conf.5)

AC_OUTPUT()

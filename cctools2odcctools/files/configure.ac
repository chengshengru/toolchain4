AC_INIT([odcctools], [782.1od1], [peter@pogma.com])
AC_PREREQ([2.57])
AC_REVISION([$Revision: 1.37 $])

AC_CONFIG_SRCDIR([APPLE_LICENSE])
AC_CONFIG_HEADERS([include/config.h])

AC_CANONICAL_BUILD
AC_CANONICAL_HOST
AC_CANONICAL_TARGET

AC_ARG_PROGRAM

case $target_os in
darwin*)
	:
	;;
*)
	AC_MSG_ERROR([$PACKAGE_NAME only supports targeting Darwin])
	;;
esac

case $target_cpu in
     powerpc)
        AC_DEFINE(EMULATED_HOST_CPU_TYPE, 18, [Emulated CPU type])
        AC_DEFINE(EMULATED_HOST_CPU_SUBTYPE, 100, [Emulated CPU subtype])
	;;
     powerpc64)
        AC_DEFINE(EMULATED_HOST_CPU_TYPE, 16777234, [Emulated CPU type])
        AC_DEFINE(EMULATED_HOST_CPU_SUBTYPE, 0, [Emulated CPU subtype])
       ;;
     i?86)
        AC_DEFINE(EMULATED_HOST_CPU_TYPE, 7, [Emulated CPU type])
        AC_DEFINE(EMULATED_HOST_CPU_SUBTYPE, 10, [Emulated CPU subtype])
	;;
     x86_64)
        AC_DEFINE(EMULATED_HOST_CPU_TYPE, 16777223, [Emulated CPU type])
        AC_DEFINE(EMULATED_HOST_CPU_SUBTYPE, 3, [Emulated CPU subtype])
	;;
     arm)
        AC_DEFINE(EMULATED_HOST_CPU_TYPE, 12, [Emulated CPU type])
        AC_DEFINE(EMULATED_HOST_CPU_SUBTYPE, 0, [Emulated CPU subtype])
	;;
     *)
	AC_MSG_ERROR([Unsupported target $target_cpu])
	;;
esac


AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_CC
AC_PROG_CXX

AC_CHECK_TOOL([HOST_RANLIB], [ranlib], :)
AC_CHECK_TOOL([HOST_AR], [ar], :)

ASTARGETS=""
conftargets=""
AC_MSG_CHECKING([for configured assembler targets])
AC_ARG_ENABLE([as-targets],
		AC_HELP_STRING([--enable-as-targets],
				[space-separated list ("ppc", "ppc64", "i386", "x86_64", "arm", "all")]),
		[conftargets="$enableval"],
		[conftargets="default"])

case "$conftargets" in
     all)
        ASTARGETS="ppc ppc64 i386 x86_64 arm"
	;;
     default)
	ASTARGETS="ppc ppc64 i386 x86_64 arm"
	;;
     yes)
	AC_MSG_ERROR([no assembler targets configured])
	;;
     no)
	AC_MSG_ERROR([no assembler targets configured])
	;;
     *)
	if [[ -z "$conftargets" ]]; then
	   AC_MSG_ERROR([no assembler targets configured])
	fi
	ASTARGETS="$conftargets"
	;;
esac

AC_MSG_RESULT([$ASTARGETS])
AC_SUBST([ASTARGETS], [$ASTARGETS]) 

AC_SUBST([ASLIBEXECDIR], ['${libexecdir}/as'])

AC_ARG_WITH([sysroot],
		AC_HELP_STRING([--with-sysroot],
				[filesystem root of cross build environment]),
		[AC_DEFINE_UNQUOTED([CROSS_SYSROOT],["$withval"],[filesystem root of cross build environment])])

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -mdynamic-no-pic"
AC_MSG_CHECKING([if -mdynamic-no-pic is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[const char hw[] = "Hello, World\n";]])],
   [AC_SUBST([MDYNAMICNOPIC],[-mdynamic-no-pic])
    AC_MSG_RESULT([yes])],
   [AC_SUBST([MDYNAMICNOPIC],[])
    AC_MSG_RESULT([no])]
)
CFLAGS=$ORIGCFLAGS

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -fno-builtin-round"
AC_MSG_CHECKING([if -fno-builtin-round is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[const char hw[] = "Hello, World\n";]])],
   [AC_MSG_RESULT([yes])],
   [CFLAGS=$ORIGCFLAGS
    AC_MSG_RESULT([no])]
)

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -fno-builtin-trunc"
AC_MSG_CHECKING([if -fno-builtin-trunc is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[const char hw[] = "Hello, World\n";]])],
   [AC_MSG_RESULT([yes])],
   [CFLAGS=$ORIGCFLAGS
    AC_MSG_RESULT([no])]
)

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -no-cpp-precomp"
supported=0
AC_MSG_CHECKING([if -no-cpp-precomp is supported])
AC_LANG_CONFTEST([[const char hw[] = "Hello, World\n";]])
rm -f conftest.$ac_objext conftest$ac_exeext
out=`$CC -c $CFLAGS $CPPFLAGS conftest.$ac_ext 2>&1`
if [[ $? -ne 0 ]]; then
   supported=0;
else
   unrecog=`echo $out | grep unrecog | wc -l`
   if [[ $unrecog -gt 0 ]]; then
      supported=0;
   else
      supported=1;
   fi
fi

if [[ $supported -eq 1 ]]; then
   AC_MSG_RESULT([yes])
else
   CFLAGS="$ORIGCFLAGS"
   AC_MSG_RESULT([no])
fi
rm -f conftest.$ac_objext conftest$ac_exeext conftest.$ac_ext

WARNINGS=""

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -Wall"
AC_MSG_CHECKING([if -Wall is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[const char hw[] = "Hello, World\n";]])],
   [WARNINGS="$WARNINGS -Wall"
    AC_MSG_RESULT([yes])],
   [AC_MSG_RESULT([no])]
)
CFLAGS=$ORIGCFLAGS

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -Wno-long-double"
AC_MSG_CHECKING([if -Wno-long-double is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[const char hw[] = "Hello, World\n";]])],
   [WARNINGS="$WARNINGS -Wno-long-double"
    AC_MSG_RESULT([yes])],
   [AC_MSG_RESULT([no])]
)
CFLAGS=$ORIGCFLAGS

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -Wno-import"
AC_MSG_CHECKING([if -Wno-import is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[const char hw[] = "Hello, World\n";]])],
   [WARNINGS="$WARNINGS -Wno-import"
    AC_MSG_RESULT([yes])],
   [AC_MSG_RESULT([no])]
)
CFLAGS=$ORIGCFLAGS

AC_SUBST([WARNINGS], [$WARNINGS])

ORIGCFLAGS=$CFLAGS
CFLAGS="$CFLAGS -x objective-c"
OBJC="-x objective-c"
AC_MSG_CHECKING([if -x objective-c is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[@class foo;
void bar(void) {
     [foo alloc];
}
]])],
   [AC_DEFINE(HAVE_OBJC, 1, [Have Objective-C support])
    AC_MSG_RESULT([yes])],
   [OBJC=""
    AC_MSG_RESULT([no])]
)
AC_SUBST([OBJC], [$OBJC])
CFLAGS=$ORIGCFLAGS

AC_MSG_CHECKING([if __private_extern__ is supported])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[__private_extern__ int foo;]])],
   [AC_SUBST([HAVE_PRIVEXT], [YES])
    AC_MSG_RESULT([yes])],
   [AC_SUBST([HAVE_PRIVEXT], [NO])
    AC_MSG_RESULT([no])]
)

ORIGLDFLAGS=$LDFLAGS
LDFLAGS="$LDFLAGS -multiply_defined suppress"
AC_MSG_CHECKING([if -multiply_defined suppress is supported])
AC_LINK_IFELSE([AC_LANG_PROGRAM()],
	[AC_MSG_RESULT([yes])],
	[LDFLAGS="$ORIGLDFLAGS"
	 AC_MSG_RESULT([no])]
)

AC_LANG_PUSH([C++])
ORIGCXXFLAGS=$CXXFLAGS
CXXFLAGS="$CXXFLAGS -Wno-trigraphs"
AC_MSG_CHECKING([if -Wno-trigraphs is supported])
AC_LINK_IFELSE([AC_LANG_PROGRAM()],
	[AC_MSG_RESULT([yes])],
	[CXXFLAGS="$ORIGCXXFLAGS"
	 AC_MSG_RESULT([no])]
)

ORIGCXXFLAGS=$CXXFLAGS
CXXFLAGS="$CXXFLAGS -Wno-deprecated"
AC_MSG_CHECKING([if -Wno-deprecated is supported])
AC_LINK_IFELSE([AC_LANG_PROGRAM()],
        [AC_MSG_RESULT([yes])],
	        [CXXFLAGS="$ORIGCXXFLAGS"
		 AC_MSG_RESULT([no])]
)

ORIGCXXFLAGS=$CXXFLAGS
CXXFLAGS="$CXXFLAGS -Wno-strict-aliasing"
AC_MSG_CHECKING([if -Wno-strict-aliasing is supported])
AC_LINK_IFELSE([AC_LANG_PROGRAM()],
        [AC_MSG_RESULT([yes])],
	        [CXXFLAGS="$ORIGCXXFLAGS"
		 AC_MSG_RESULT([no])]
)

AC_LANG_POP
AC_HEADER_STDC
AC_CHECK_HEADERS([stdio.h errno.h limits.h])
if [[ "x$OBJC" != "x" ]]; then
   ORIGCFLAGS=$CFLAGS
   CFLAGS="$CFLAGS $OBJC"
   AC_CHECK_HEADERS([objc/objc-runtime.h])
   CFLAGS="$ORIGCFLAGS"
fi

AC_MSG_CHECKING([for max unsigned long long])
AC_COMPILE_IFELSE(
   [AC_LANG_SOURCE([[#if HAVE_LIMITS_H
			 #include <limits.h>
		     #endif
		     unsigned long long a = ULLONG_MAX;]])],
   [AC_MSG_RESULT([ULLONG_MAX])],
   [AC_COMPILE_IFELSE(
	[AC_LANG_SOURCE([[#if HAVE_LIMITS_H
			      #include <limits.h>
			 #endif
			 unsigned long long a = 18446744073709551615ULL;]])],
	[AC_DEFINE(ULLONG_MAX, [18446744073709551615ULL], [max unsigned long long])
	 AC_MSG_RESULT([18446744073709551615ULL])],
	[AC_MSG_RESULT([none]) ]
	)
    ]
)

AC_CHECK_FUNCS([strmode qsort_r qsort getattrlist NSIsSymbolNameDefined strlcpy strlcat])
AC_CHECK_HEADERS([CommonCrypto/CommonDigest.h],[],[SSL_LIBS=-lssl])
AC_SUBST(SSL_LIBS)

AC_CHECK_DECLS([getc_unlocked, strlcpy, strlcat])
AH_VERBATIM([HAVE_DECL_GETC_UNLOCKED_REPLACEMENT],
[/* For systems that don't have getc_unlocked, use getc  */
#if !HAVE_DECL_GETC_UNLOCKED
# define getc_unlocked(a) getc(a)
#endif])

AH_VERBATIM([X_DECLS],
[/* Declare functions that are missing declarations */
#if !HAVE_DECL_STRLCPY || ! HAVE_DECL_STRLCAT
#include <sys/types.h>
#endif
#if !HAVE_DECL_STRLCPY
#ifdef __cplusplus
extern "C"
#endif
size_t strlcpy(char *dst, const char *src, size_t size);
#endif
#if !HAVE_DECL_STRLCAT
#ifdef __cplusplus
extern "C"
#endif
size_t strlcat(char *dst, const char *src, size_t size);
#endif
])

case $host_os in
     darwin*)
	AC_SUBST([HAVE_FOREIGN_HEADERS],[NO])
	;;
     *)
	AC_SUBST([HAVE_FOREIGN_HEADERS],[YES])
	AC_SUBST([EMULATED_OBJ], [emulated.c])
	AC_C_BIGENDIAN([AC_SUBST([ENDIAN_FLAG],[-D__BIG_ENDIAN__=1])],
			[AC_SUBST([ENDIAN_FLAG],[-D__LITTLE_ENDIAN__=1])])
	;;
esac

AC_CHECK_SIZEOF([long])
if test $ac_cv_sizeof_long >=8; then
	LD_CLASSIC=
else
	LD_CLASSIC=ld
fi
COMPONENTS="ar as include libstuff libmacho ${LD_CLASSIC} ld64 man misc otool"
AC_SUBST([COMPONENTS], [$COMPONENTS])

AC_CHECK_FUNC([uuid_generate_random],[],
	      [AC_CHECK_LIB([uuid],[uuid_generate_random],
			    [UUID_LIBS="-luuid"],
			    [AC_MSG_ERROR(
			     [No uuid_generate_random function found])])])
AC_SUBST([UUID_LIBS])

# LTO
AC_CHECK_LIB([LTO],[lto_get_version],[
	     LTO_LIBS=-lLTO
	     LTO_DEFS="-DLTO_SUPPORT=1"
	     ])
AC_SUBST(LTO_LIBS)
AC_SUBST(LTO_DEFS)

AC_CONFIG_FILES([Makefile include/Makefile libstuff/Makefile libmacho/Makefile])
AC_CONFIG_FILES([ar/Makefile])
AC_CONFIG_FILES([as/Makefile])
for ast in $ASTARGETS; do
    case $ast in
    ppc)
	AC_CONFIG_FILES([as/ppc/Makefile:as/ppc/Makefile.in:as/Makefile.arch.in])
	;;
    ppc64)
	AC_CONFIG_FILES([as/ppc64/Makefile:as/ppc64/Makefile.in:as/Makefile.arch.in])
	;;
    i386)
	AC_CONFIG_FILES([as/i386/Makefile:as/i386/Makefile.in:as/Makefile.arch.in])
	;;
    x86_64)
	AC_CONFIG_FILES([as/x86_64/Makefile:as/x86_64/Makefile.in:as/Makefile.arch.in])
	;;
    arm)
	AC_CONFIG_FILES([as/arm/Makefile:as/arm/Makefile.in:as/Makefile.arch.in])
	;;
    *)
	AC_MSG_ERROR([Unrecognized assembler target $ast])
	;;
    esac
done
AC_CONFIG_FILES([ld/Makefile])
AC_CONFIG_FILES([ld64/Makefile])
AC_CONFIG_FILES([man/Makefile])
AC_CONFIG_FILES([misc/Makefile])
AC_CONFIG_FILES([otool/Makefile])
AC_OUTPUT

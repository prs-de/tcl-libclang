#!/bin/bash -norc
dnl	This file is an input file used by the GNU "autoconf" program to
dnl	generate the file "configure", which is run during Tcl installation
dnl	to configure the system for the local environment.

#-----------------------------------------------------------------------
# Sample configure.ac for Tcl Extensions.  The only places you should
# need to modify this file are marked by the string __CHANGE__
#-----------------------------------------------------------------------

#-----------------------------------------------------------------------
# __CHANGE__
# Set your package name and version numbers here.
#
# This initializes the environment with PACKAGE_NAME and PACKAGE_VERSION
# set as provided.  These will also be added as -D defs in your Makefile
# so you can encode the package version directly into the source files.
# This will also define a special symbol for Windows (BUILD_<PACKAGE_NAME>
# so that we create the export library with the dll.
#-----------------------------------------------------------------------

AC_INIT([cindex], [1.24])

#--------------------------------------------------------------------
# Call TEA_INIT as the first TEA_ macro to set up initial vars.
# This will define a ${TEA_PLATFORM} variable == "unix" or "windows"
# as well as PKG_LIB_FILE and PKG_STUB_LIB_FILE.
#--------------------------------------------------------------------

TEA_INIT([3.9])

AC_CONFIG_AUX_DIR(tclconfig)

#--------------------------------------------------------------------
# Load the tclConfig.sh file
#--------------------------------------------------------------------

TEA_PATH_TCLCONFIG
TEA_LOAD_TCLCONFIG

#--------------------------------------------------------------------
# Load the tkConfig.sh file if necessary (Tk extension)
#--------------------------------------------------------------------

#TEA_PATH_TKCONFIG
#TEA_LOAD_TKCONFIG

#-----------------------------------------------------------------------
# Handle the --prefix=... option by defaulting to what Tcl gave.
# Must be called after TEA_LOAD_TCLCONFIG and before TEA_SETUP_COMPILER.
#-----------------------------------------------------------------------

TEA_PREFIX

#-----------------------------------------------------------------------
# Standard compiler checks.
# This sets up CC by using the CC env var, or looks for gcc otherwise.
# This also calls AC_PROG_CC and a few others to create the basic setup
# necessary to compile executables.
#-----------------------------------------------------------------------

TEA_SETUP_COMPILER
AC_PROG_CC_STDC

#-----------------------------------------------------------------------
# __CHANGE__
# Specify the C source files to compile in TEA_ADD_SOURCES,
# public headers that need to be installed in TEA_ADD_HEADERS,
# stub library C source files to compile in TEA_ADD_STUB_SOURCES,
# and runtime Tcl library files in TEA_ADD_TCL_SOURCES.
# This defines PKG(_STUB)_SOURCES, PKG(_STUB)_OBJECTS, PKG_HEADERS
# and PKG_TCL_SOURCES.
#-----------------------------------------------------------------------

TEA_ADD_SOURCES([generic/libcindex.c])
TEA_ADD_HEADERS([])
TEA_ADD_INCLUDES([])
TEA_ADD_LIBS([])
TEA_ADD_CFLAGS([])
TEA_ADD_STUB_SOURCES([])
TEA_ADD_TCL_SOURCES([])

#--------------------------------------------------------------------
# __CHANGE__
#
# You can add more files to clean if your extension creates any extra
# files by extending CLEANFILES.
# Add pkgIndex.tcl if it is generated in the Makefile instead of ./configure
# and change Makefile.in to move it from CONFIG_CLEAN_FILES to BINARIES var.
#
# A few miscellaneous platform-specific items:
# TEA_ADD_* any platform specific compiler/build info here.
#--------------------------------------------------------------------

#CLEANFILES="$CLEANFILES pkgIndex.tcl"
if test "${TEA_PLATFORM}" = "windows" ; then
    # Ensure no empty if clauses
    :
    #TEA_ADD_SOURCES([win/winFile.c])
    #TEA_ADD_INCLUDES([-I\"$(${CYGPATH} ${srcdir}/win)\"])
else
    # Ensure no empty else clauses
    :
    #TEA_ADD_SOURCES([unix/unixFile.c])
    #TEA_ADD_LIBS([-lsuperfly])
fi

#--------------------------------------------------------------------
# __CHANGE__
# Choose which headers you need.  Extension authors should try very
# hard to only rely on the Tcl public header files.  Internal headers
# contain private data structures and are subject to change without
# notice.
# This MUST be called after TEA_LOAD_TCLCONFIG / TEA_LOAD_TKCONFIG
#--------------------------------------------------------------------

TEA_PUBLIC_TCL_HEADERS
#TEA_PRIVATE_TCL_HEADERS

#TEA_PUBLIC_TK_HEADERS
#TEA_PRIVATE_TK_HEADERS
#TEA_PATH_X

#--------------------------------------------------------------------
# Check whether --enable-threads or --disable-threads was given.
# This auto-enables if Tcl was compiled threaded.
#--------------------------------------------------------------------

TEA_ENABLE_THREADS

#--------------------------------------------------------------------
# The statement below defines a collection of symbols related to
# building as a shared library instead of a static library.
#--------------------------------------------------------------------

TEA_ENABLE_SHARED

#--------------------------------------------------------------------
# This macro figures out what flags to use with the compiler/linker
# when building shared/static debug/optimized objects.  This information
# can be taken from the tclConfig.sh file, but this figures it all out.
#--------------------------------------------------------------------

TEA_CONFIG_CFLAGS

#--------------------------------------------------------------------
# Set the default compiler switches based on the --enable-symbols option.
#--------------------------------------------------------------------

TEA_ENABLE_SYMBOLS

#--------------------------------------------------------------------
# Everyone should be linking against the Tcl stub library.  If you
# can't for some reason, remove this definition.  If you aren't using
# stubs, you also need to modify the SHLIB_LD_LIBS setting below to
# link against the non-stubbed Tcl library.  Add Tk too if necessary.
#--------------------------------------------------------------------

AC_DEFINE(USE_TCL_STUBS, 1, [Use Tcl stubs])
#AC_DEFINE(USE_TK_STUBS, 1, [Use Tk stubs])

#--------------------------------------------------------------------
# LLVM
#--------------------------------------------------------------------

AC_ARG_WITH(
    [llvm_config],
    AS_HELP_STRING([--with-llvm-config@<:@=DIR@:>@], [path to the llvm-config program]),
    [LLVM_CONFIG=$with_llvm_config],
    [llvm-config])
AC_PATH_PROG([LLVM_CONFIG], [$LLVM_CONFIG], [no])
AS_IF([test "x$LLVM_CONFIG" = "xno"],
      [AC_MSG_ERROR([Failed to locate the llvm-config program.])])
AC_MSG_CHECKING([for llvm-config usability])
LLVM_VERSION="$(${LLVM_CONFIG} --version)"
CLANG_CFLAGS="-I$(${LLVM_CONFIG} --includedir)"
CLANG_LIBS="-L$(${LLVM_CONFIG} --libdir) -lclang"
CLANG_BUILTIN_HEADER_INCLUDE_DIR="$(${LLVM_CONFIG} --libdir)/clang/${LLVM_VERSION}/include"
AC_SUBST([CLANG_CFLAGS])
AC_SUBST([CLANG_LIBS])
AC_SUBST([CLANG_BUILTIN_HEADER_INCLUDE_DIR])
AC_MSG_RESULT([yes])

#--------------------------------------------------------------------
# Documentation
#--------------------------------------------------------------------

# DITA-OT
AC_ARG_WITH(dita_ot,
            [AS_HELP_STRING(
                    [--with-dita-ot=DIR],
                    [directory containing DITA-OT files (default: /usr/share/dita-ot)])],
            [],
            [with_dita_ot=/usr/share/dita-ot])
AS_IF([test -d "$with_dita_ot"],
      [DITA_OT="$with_dita_ot"],
      [AC_MSG_WARN([Failed to locate DITA-OT.  Cannot build documentation.])])
AC_SUBST([DITA_OT])

# ANT
AC_PATH_PROG([ANT], [ant])
AS_IF([test "x$ANT" = "x"],
      [AC_MSG_WARN([Failed to locate the ant program.  Cannot build documentation.])])
AC_SUBST([ANT])

# Saxon
AC_ARG_WITH(saxon,
            [AS_HELP_STRING(
                    [--with-xml-impl=CLASS],
                    [XML implementation to use (default: com.icl.saxon.TransformerFactoryImpl)])],
            [],
            [with_saxon="com.icl.saxon.TransformerFactoryImpl"])
AC_SUBST([SAXON_IMPL], [$with_saxon])
# Classpath
AC_ARG_WITH(classpath,
            [AS_HELP_STRING(
                    [--with-classpath=CLASSPATH],
                    [Java classpath])])
AC_SUBST([CLASSPATH], [$with_classpath])

#--------------------------------------------------------------------
# This macro generates a line to use when building a library.  It
# depends on values set by the TEA_ENABLE_SHARED, TEA_ENABLE_SYMBOLS,
# and TEA_LOAD_TCLCONFIG macros above.
#--------------------------------------------------------------------

TEA_MAKE_LIB

#--------------------------------------------------------------------
# Determine the name of the tclsh and/or wish executables in the
# Tcl and Tk build directories or the location they were installed
# into. These paths are used to support running test cases only,
# the Makefile should not be making use of these paths to generate
# a pkgIndex.tcl file or anything else at extension build time.
#--------------------------------------------------------------------

TEA_PROG_TCLSH
#TEA_PROG_WISH

#--------------------------------------------------------------------
# Finally, substitute all of the various values into the Makefile.
# You may alternatively have a special pkgIndex.tcl.in or other files
# which require substituting th AC variables in.  Include these here.
#--------------------------------------------------------------------

AC_OUTPUT([Makefile pkgIndex.tcl])

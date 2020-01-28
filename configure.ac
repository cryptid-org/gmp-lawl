dnl  Process this file with autoconf to produce a configure script.


define(GMP_COPYRIGHT,[[

Copyright 1996-2016 Free Software Foundation, Inc.

This file is part of the GNU MP Library.

The GNU MP Library is free software; you can redistribute it and/or modify
it under the terms of either:

  * the GNU Lesser General Public License as published by the Free
    Software Foundation; either version 3 of the License, or (at your
    option) any later version.

or

  * the GNU General Public License as published by the Free Software
    Foundation; either version 2 of the License, or (at your option) any
    later version.

or both in parallel, as here.

The GNU MP Library is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
for more details.

You should have received copies of the GNU General Public License and the
GNU Lesser General Public License along with the GNU MP Library.  If not,
see https://www.gnu.org/licenses/.
]])

AC_COPYRIGHT(GMP_COPYRIGHT)
AH_TOP(/*GMP_COPYRIGHT*/)

AC_REVISION($Revision$)
AC_PREREQ(2.59)
AC_INIT(GNU MP, GMP_VERSION, [gmp-bugs@gmplib.org, see https://gmplib.org/manual/Reporting-Bugs.html], gmp)
AC_CONFIG_SRCDIR(gmp-impl.h)
m4_pattern_forbid([^[ \t]*GMP_])
m4_pattern_allow(GMP_LDFLAGS)
m4_pattern_allow(GMP_LIMB_BITS)
m4_pattern_allow(GMP_MPARAM_H_SUGGEST)
m4_pattern_allow(GMP_NAIL_BITS)
m4_pattern_allow(GMP_NUMB_BITS)
m4_pattern_allow(GMP_NONSTD_ABI)
m4_pattern_allow(GMP_CPU_TYPE)

# If --target is not used then $target_alias is empty, but if say
# "./configure athlon-pc-freebsd3.5" is used, then all three of
# $build_alias, $host_alias and $target_alias are set to
# "athlon-pc-freebsd3.5".
#
if test -n "$target_alias" && test "$target_alias" != "$host_alias"; then
  AC_MSG_ERROR([--target is not appropriate for GMP
Use --build=CPU-VENDOR-OS if you need to specify your CPU and/or system
explicitly.  Use --host if cross-compiling (see "Installing GMP" in the
manual for more on this).])
fi

GMP_INIT(config.m4)

AC_CANONICAL_HOST

dnl  Automake "no-dependencies" is used because include file dependencies
dnl  are not useful to us.  Pretty much everything depends just on gmp.h,
dnl  gmp-impl.h and longlong.h, and yet only rarely does everything need to
dnl  be rebuilt for changes to those files.
dnl
dnl  "no-dependencies" also helps with the way we're setup to run
dnl  AC_PROG_CXX only conditionally.  If dependencies are used then recent
dnl  automake (eg 1.7.2) appends an AM_CONDITIONAL to AC_PROG_CXX, and then
dnl  gets upset if it's not actually executed.
dnl
dnl  Note that there's a copy of these options in the top-level Makefile.am,
dnl  so update there too if changing anything.
dnl
AM_INIT_AUTOMAKE([1.8 gnu no-dependencies])
AC_CONFIG_HEADERS(config.h:config.in)
AM_MAINTAINER_MODE


AC_ARG_ENABLE(assert,
AC_HELP_STRING([--enable-assert],[enable ASSERT checking [default=no]]),
[case $enableval in
yes|no) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-assert, need yes or no]) ;;
esac],
[enable_assert=no])

if test "$enable_assert" = "yes"; then
  AC_DEFINE(WANT_ASSERT,1,
  [Define to 1 to enable ASSERT checking, per --enable-assert])
  want_assert_01=1
else
  want_assert_01=0
fi
GMP_DEFINE_RAW(["define(<WANT_ASSERT>,$want_assert_01)"])


AC_ARG_ENABLE(alloca,
AC_HELP_STRING([--enable-alloca],[how to get temp memory [default=reentrant]]),
[case $enableval in
alloca|malloc-reentrant|malloc-notreentrant) ;;
yes|no|reentrant|notreentrant) ;;
debug) ;;
*)
  AC_MSG_ERROR([bad value $enableval for --enable-alloca, need one of:
yes no reentrant notreentrant alloca malloc-reentrant malloc-notreentrant debug]) ;;
esac],
[enable_alloca=reentrant])


# IMPROVE ME: The default for C++ is disabled.  The tests currently
# performed below for a working C++ compiler are not particularly strong,
# and in general can't be expected to get the right setup on their own.  The
# most significant problem is getting the ABI the same.  Defaulting CXXFLAGS
# to CFLAGS takes only a small step towards this.  It's also probably worth
# worrying whether the C and C++ runtimes from say gcc and a vendor C++ can
# work together.  Some rather broken C++ installations were encountered
# during testing, and though such things clearly aren't GMP's problem, if
# --enable-cxx=detect were to be the default then some careful checks of
# which, if any, C++ compiler on the system is up to scratch would be
# wanted.
#
AC_ARG_ENABLE(cxx,
AC_HELP_STRING([--enable-cxx],[enable C++ support [default=no]]),
[case $enableval in
yes|no|detect) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-cxx, need yes/no/detect]) ;;
esac],
[enable_cxx=no])


AC_ARG_ENABLE(assembly,
AC_HELP_STRING([--enable-assembly],[enable the use of assembly loops [default=yes]]),
[case $enableval in
yes|no) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-assembly, need yes or no]) ;;
esac],
[enable_assembly=yes])


AC_ARG_ENABLE(fft,
AC_HELP_STRING([--enable-fft],[enable FFTs for multiplication [default=yes]]),
[case $enableval in
yes|no) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-fft, need yes or no]) ;;
esac],
[enable_fft=yes])

if test "$enable_fft" = "yes"; then
  AC_DEFINE(WANT_FFT,1,
  [Define to 1 to enable FFTs for multiplication, per --enable-fft])
fi


AC_ARG_ENABLE(old-fft-full,
AC_HELP_STRING([--enable-old-fft-full],[enable old mpn_mul_fft_full for multiplication [default=no]]),
[case $enableval in
yes|no) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-old-fft-full, need yes or no]) ;;
esac],
[enable_old_fft_full=no])

if test "$enable_old_fft_full" = "yes"; then
  AC_DEFINE(WANT_OLD_FFT_FULL,1,
  [Define to 1 to enable old mpn_mul_fft_full for multiplication, per --enable-old-fft-full])
fi


AC_ARG_ENABLE(nails,
AC_HELP_STRING([--enable-nails],[use nails on limbs [default=no]]),
[case $enableval in
[yes|no|[02468]|[0-9][02468]]) ;;
[*[13579]])
  AC_MSG_ERROR([bad value $enableval for --enable-nails, only even nail sizes supported]) ;;
*)
  AC_MSG_ERROR([bad value $enableval for --enable-nails, need yes/no/number]) ;;
esac],
[enable_nails=no])

case $enable_nails in
yes) GMP_NAIL_BITS=2 ;;
no)  GMP_NAIL_BITS=0 ;;
*)   GMP_NAIL_BITS=$enable_nails ;;
esac
AC_SUBST(GMP_NAIL_BITS)


AC_ARG_ENABLE(profiling,
AC_HELP_STRING([--enable-profiling],
               [build with profiler support [default=no]]),
[case $enableval in
no|prof|gprof|instrument) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-profiling, need no/prof/gprof/instrument]) ;;
esac],
[enable_profiling=no])

case $enable_profiling in
  prof)
    AC_DEFINE(WANT_PROFILING_PROF, 1,
              [Define to 1 if --enable-profiling=prof])
    ;;
  gprof)
    AC_DEFINE(WANT_PROFILING_GPROF, 1,
              [Define to 1 if --enable-profiling=gprof])
    ;;
  instrument)
    AC_DEFINE(WANT_PROFILING_INSTRUMENT, 1,
              [Define to 1 if --enable-profiling=instrument])
    ;;
esac

GMP_DEFINE_RAW(["define(<WANT_PROFILING>,<\`$enable_profiling'>)"])

# -fomit-frame-pointer is incompatible with -pg on some chips
if test "$enable_profiling" = gprof; then
  fomit_frame_pointer=
else
  fomit_frame_pointer="-fomit-frame-pointer"
fi


AC_ARG_WITH(readline,
AC_HELP_STRING([--with-readline],
               [readline support in demo programs [default=detect]]),
[case $withval in
yes|no|detect) ;;
*) AC_MSG_ERROR([bad value $withval for --with-readline, need yes/no/detect]) ;;
esac],
[with_readline=detect])


AC_ARG_ENABLE(fat,
AC_HELP_STRING([--enable-fat],
               [build fat libraries on systems that support it [default=no]]),
[case $enableval in
yes|no) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-fat, need yes or no]) ;;
esac],
[enable_fat=no])


AC_ARG_ENABLE(minithres,
AC_HELP_STRING([--enable-minithres],
               [choose minimal thresholds for testing [default=no]]),
[case $enableval in
yes|no) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-minithres, need yes or no]) ;;
esac],
[enable_minithres=no])


AC_ARG_ENABLE(fake-cpuid,
AC_HELP_STRING([--enable-fake-cpuid],[enable GMP_CPU_TYPE faking cpuid [default=no]]),
[case $enableval in
yes|no) ;;
*) AC_MSG_ERROR([bad value $enableval for --enable-fake-cpuid, need yes or no]) ;;
esac],
[enable_fake_cpuid=no])

if test "$enable_fake_cpuid" = "yes"; then
  AC_DEFINE(WANT_FAKE_CPUID,1,
  [Define to 1 to enable GMP_CPU_TYPE faking cpuid, per --enable-fake-cpuid])
fi


if test $enable_fat = yes && test $enable_assembly = no ; then
  AC_MSG_ERROR([when doing a fat build, disabling assembly will not work])
fi

if test $enable_fake_cpuid = yes && test $enable_fat = no ; then
  AC_MSG_ERROR([--enable-fake-cpuid requires --enable-fat])
fi


tmp_host=`echo $host_cpu | sed 's/\./_/'`
AC_DEFINE_UNQUOTED(HAVE_HOST_CPU_$tmp_host)
GMP_DEFINE_RAW("define_not_for_expansion(\`HAVE_HOST_CPU_$tmp_host')", POST)

dnl  The HAVE_HOST_CPU_ list here only needs to have entries for those which
dnl  are going to be tested, not everything that can possibly be selected.
dnl
dnl  The HAVE_HOST_CPU_FAMILY_ list similarly, and note that the AC_DEFINEs
dnl  for these are under the cpu specific setups below.

AH_VERBATIM([HAVE_HOST_CPU_1],
[/* Define one of these to 1 for the host CPU family.
   If your CPU is not in any of these families, leave all undefined.
   For an AMD64 chip, define "x86" in ABI=32, but not in ABI=64. */
#undef HAVE_HOST_CPU_FAMILY_alpha
#undef HAVE_HOST_CPU_FAMILY_m68k
#undef HAVE_HOST_CPU_FAMILY_power
#undef HAVE_HOST_CPU_FAMILY_powerpc
#undef HAVE_HOST_CPU_FAMILY_x86
#undef HAVE_HOST_CPU_FAMILY_x86_64

/* Define one of the following to 1 for the host CPU, as per the output of
   ./config.guess.  If your CPU is not listed here, leave all undefined.  */
#undef HAVE_HOST_CPU_alphaev67
#undef HAVE_HOST_CPU_alphaev68
#undef HAVE_HOST_CPU_alphaev7
#undef HAVE_HOST_CPU_m68020
#undef HAVE_HOST_CPU_m68030
#undef HAVE_HOST_CPU_m68040
#undef HAVE_HOST_CPU_m68060
#undef HAVE_HOST_CPU_m68360
#undef HAVE_HOST_CPU_powerpc604
#undef HAVE_HOST_CPU_powerpc604e
#undef HAVE_HOST_CPU_powerpc750
#undef HAVE_HOST_CPU_powerpc7400
#undef HAVE_HOST_CPU_supersparc
#undef HAVE_HOST_CPU_i386
#undef HAVE_HOST_CPU_i586
#undef HAVE_HOST_CPU_i686
#undef HAVE_HOST_CPU_pentium
#undef HAVE_HOST_CPU_pentiummmx
#undef HAVE_HOST_CPU_pentiumpro
#undef HAVE_HOST_CPU_pentium2
#undef HAVE_HOST_CPU_pentium3
#undef HAVE_HOST_CPU_s390_z900
#undef HAVE_HOST_CPU_s390_z990
#undef HAVE_HOST_CPU_s390_z9
#undef HAVE_HOST_CPU_s390_z10
#undef HAVE_HOST_CPU_s390_z196

/* Define to 1 iff we have a s390 with 64-bit registers.  */
#undef HAVE_HOST_CPU_s390_zarch])


# Table of compilers, options, and mpn paths.  This code has various related
# purposes
#
#   - better default CC/CFLAGS selections than autoconf otherwise gives
#   - default CC/CFLAGS selections for extra CPU types specific to GMP
#   - a few tests for known bad compilers
#   - choice of ABIs on suitable systems
#   - selection of corresponding mpn search path
#
# After GMP specific searches and tests, the standard autoconf AC_PROG_CC is
# called.  User selections of CC etc are respected.
#
# Care is taken not to use macros like AC_TRY_COMPILE during the GMP
# pre-testing, since they of course depend on AC_PROG_CC, and also some of
# them cache their results, which is not wanted.
#
# The ABI selection mechanism is unique to GMP.  All that reaches autoconf
# is a different selection of CC/CFLAGS according to the best ABI the system
# supports, and/or what the user selects.  Naturally the mpn assembler code
# selected is very dependent on the ABI.
#
# The closest the standard tools come to a notion of ABI is something like
# "sparc64" which encodes a CPU and an ABI together.  This doesn't seem to
# scale well for GMP, where exact CPU types like "ultrasparc2" are wanted,
# separate from the ABI used on them.
#
#
# The variables set here are
#
#   cclist              the compiler choices
#   xx_cflags           flags for compiler xx
#   xx_cflags_maybe     flags for compiler xx, if they work
#   xx_cppflags         cpp flags for compiler xx
#   xx_cflags_optlist   list of sets of optional flags
#   xx_cflags_yyy       set yyy of optional flags for compiler xx
#   xx_ldflags          -Wc,-foo flags for libtool linking with compiler xx
#   ar_flags            extra flags for $AR
#   nm_flags            extra flags for $NM
#   limb                limb size, can be "longlong"
#   path                mpn search path
#   extra_functions     extra mpn functions
#   fat_path            fat binary mpn search path [if fat binary desired]
#   fat_functions       fat functions
#   fat_thresholds      fat thresholds
#
# Suppose xx_cflags_optlist="arch", then flags from $xx_cflags_arch are
# tried, and the first flag that works will be used.  An optlist like "arch
# cpu optimize" can be used to get multiple independent sets of flags tried.
# The first that works from each will be used.  If no flag in a set works
# then nothing from that set is added.
#
# For multiple ABIs, the scheme extends as follows.
#
#   abilist               set of ABI choices
#   cclist_aa             compiler choices in ABI aa
#   xx_aa_cflags          flags for xx in ABI aa
#   xx_aa_cflags_maybe    flags for xx in ABI aa, if they work
#   xx_aa_cppflags        cpp flags for xx in ABI aa
#   xx_aa_cflags_optlist  list of sets of optional flags in ABI aa
#   xx_aa_cflags_yyy      set yyy of optional flags for compiler xx in ABI aa
#   xx_aa_ldflags         -Wc,-foo flags for libtool linking
#   ar_aa_flags           extra flags for $AR in ABI aa
#   nm_aa_flags           extra flags for $NM in ABI aa
#   limb_aa               limb size in ABI aa, can be "longlong"
#   path_aa               mpn search path in ABI aa
#   extra_functions_aa    extra mpn functions in ABI aa
#
# As a convenience, the unadorned xx_cflags (etc) are used for the last ABI
# in ablist, if an xx_aa_cflags for that ABI isn't given.  For example if
# abilist="64 32" then $cc_64_cflags will be used for the 64-bit ABI, but
# for the 32-bit either $cc_32_cflags or $cc_cflags is used, whichever is
# defined.  This makes it easy to add some 64-bit compilers and flags to an
# unadorned 32-bit set.
#
# limb=longlong (or limb_aa=longlong) applies to all compilers within that
# ABI.  It won't work to have some needing long long and some not, since a
# single instantiated gmp.h will be used by both.
#
# SPEED_CYCLECOUNTER, cyclecounter_size and CALLING_CONVENTIONS_OBJS are
# also set here, with an ABI suffix.
#
#
#
# A table-driven approach like this to mapping cpu type to good compiler
# options is a bit of a maintenance burden, but there's not much uniformity
# between options specifications on different compilers.  Some sort of
# separately updatable tool might be cute.
#
# The use of lots of variables like this, direct and indirect, tends to
# obscure when and how various things are done, but unfortunately it's
# pretty much the only way.  If shell subroutines were portable then actual
# code like "if this .. do that" could be written, but attempting the same
# with full copies of GMP_PROG_CC_WORKS etc expanded at every point would
# hugely bloat the output.


AC_ARG_VAR(ABI, [desired ABI (for processors supporting more than one ABI)])

# abilist needs to be non-empty, "standard" is just a generic name here
abilist="standard"

# FIXME: We'd like to prefer an ANSI compiler, perhaps by preferring
# c89 over cc here.  But note that on HP-UX c89 provides a castrated
# environment, and would want to be excluded somehow.  Maybe
# AC_PROG_CC_STDC already does enough to stick cc into ANSI mode and
# we don't need to worry.
#
cclist="gcc cc"

gcc_cflags="-O2 -pedantic"
gcc_64_cflags="-O2 -pedantic"
cc_cflags="-O"
cc_64_cflags="-O"

SPEED_CYCLECOUNTER_OBJ=
cyclecounter_size=2

AC_SUBST(HAVE_HOST_CPU_FAMILY_power,  0)
AC_SUBST(HAVE_HOST_CPU_FAMILY_powerpc,0)

# mingw can be built by the cygwin gcc if -mno-cygwin is added.  For
# convenience add this automatically if it works.  Actual mingw gcc accepts
# -mno-cygwin too, but of course is the default.  mingw only runs on the
# x86s, but allow any CPU here so as to catch "none" too.
#
case $host in
  *-*-mingw*)
    gcc_cflags_optlist="$gcc_cflags_optlist nocygwin"
    gcc_cflags_nocygwin="-mno-cygwin"
    ;;
esac


CFLAGS_or_unset=${CFLAGS-'(unset)'}
CPPFLAGS_or_unset=${CPPFLAGS-'(unset)'}

cat >&AC_FD_CC <<EOF
User:
ABI=$ABI
CC=$CC
CFLAGS=$CFLAGS_or_unset
CPPFLAGS=$CPPFLAGS_or_unset
MPN_PATH=$MPN_PATH
GMP:
abilist=$abilist
cclist=$cclist
EOF


test_CFLAGS=${CFLAGS+set}
test_CPPFLAGS=${CPPFLAGS+set}

for abi in $abilist; do
  abi_last="$abi"
done

# If the user specifies an ABI then it must be in $abilist, after that
# $abilist is restricted to just that choice.
#
if test -n "$ABI"; then
  found=no
  for abi in $abilist; do
    if test $abi = "$ABI"; then found=yes; break; fi
  done
  if test $found = no; then
    AC_MSG_ERROR([ABI=$ABI is not among the following valid choices: $abilist])
  fi
  abilist="$ABI"
fi

found_compiler=no

for abi in $abilist; do

  echo "checking ABI=$abi"

  # Suppose abilist="64 32", then for abi=64, will have abi1="_64" and
  # abi2="_64".  For abi=32, will have abi1="_32" and abi2="".  This is how
  # $gcc_cflags becomes a fallback for $gcc_32_cflags (the last in the
  # abilist), but there's no fallback for $gcc_64_cflags.
  #
  abi1=[`echo _$abi | sed 's/[.]//g'`]
  if test $abi = $abi_last; then abi2=; else abi2="$abi1"; fi

  # Compiler choices under this ABI
                              eval cclist_chosen=\"\$cclist$abi1\"
  test -n "$cclist_chosen" || eval cclist_chosen=\"\$cclist$abi2\"

  # If there's a user specified $CC then don't use a list for
  # $cclist_chosen, just a single value for $ccbase.
  #
  if test -n "$CC"; then

    # The first word of $CC, stripped of any directory.  For instance
    # CC="/usr/local/bin/gcc -pipe" will give "gcc".
    #
    for ccbase in $CC; do break; done
    ccbase=`echo $ccbase | sed 's:.*/::'`

    # If this $ccbase is in $cclist_chosen then it's a compiler we know and
    # we can do flags defaulting with it.  If not, then $cclist_chosen is
    # set to "unrecognised" so no default flags are used.
    #
    # "unrecognised" is used to avoid bad effects with eval if $ccbase has
    # non-symbol characters.  For instance ccbase=my+cc would end up with
    # something like cflags="$my+cc_cflags" which would give
    # cflags="+cc_cflags" rather than the intended empty string for an
    # unknown compiler.
    #
    found=unrecognised
    for i in $cclist_chosen; do
      if test "$ccbase" = $i; then
        found=$ccbase
        break
      fi
    done
    cclist_chosen=$found
  fi

  for ccbase in $cclist_chosen; do

    # When cross compiling, look for a compiler with the $host_alias as a
    # prefix, the same way that AC_CHECK_TOOL does.  But don't do this to a
    # user-selected $CC.
    #
    # $cross_compiling will be yes/no/maybe at this point.  Do the host
    # prefixing for "maybe" as well as "yes".
    #
    if test "$cross_compiling" != no && test -z "$CC"; then
      cross_compiling_prefix="${host_alias}-"
    fi

    for ccprefix in $cross_compiling_prefix ""; do

      cc="$CC"
      test -n "$cc" || cc="$ccprefix$ccbase"

      # If the compiler is gcc but installed under another name, then change
      # $ccbase so as to use the flags we know for gcc.  This helps for
      # instance when specifying CC=gcc272 on Debian GNU/Linux, or the
      # native cc which is really gcc on NeXT or MacOS-X.
      #
      # FIXME: There's a slight misfeature here.  If cc is actually gcc but
      # gcc is not a known compiler under this $abi then we'll end up
      # testing it with no flags and it'll work, but chances are it won't be
      # in the right mode for the ABI we desire.  Let's quietly hope this
      # doesn't happen.
      #
      if test $ccbase != gcc; then
        GMP_PROG_CC_IS_GNU($cc,ccbase=gcc)
      fi

      # Similarly if the compiler is IBM xlc but invoked as cc or whatever
      # then change $ccbase and make the default xlc flags available.
      if test $ccbase != xlc; then
        GMP_PROG_CC_IS_XLC($cc,ccbase=xlc)
      fi

      # acc was Sun's first unbundled compiler back in the SunOS days, or
      # something like that, but today its man page says it's not meant to
      # be used directly (instead via /usr/ucb/cc).  The options are pretty
      # much the same as the main SunPRO cc, so share those configs.
      #
      case $host in
        *sparc*-*-solaris* | *sparc*-*-sunos*)
          if test "$ccbase" = acc; then ccbase=cc; fi ;;
      esac

      for tmp_cflags_maybe in yes no; do
                             eval cflags=\"\$${ccbase}${abi1}_cflags\"
        test -n "$cflags" || eval cflags=\"\$${ccbase}${abi2}_cflags\"

	if test "$tmp_cflags_maybe" = yes; then
          # don't try cflags_maybe when the user set CFLAGS
          if test "$test_CFLAGS" = set; then continue; fi
                                     eval cflags_maybe=\"\$${ccbase}${abi1}_cflags_maybe\"
          test -n "$cflags_maybe" || eval cflags_maybe=\"\$${ccbase}${abi2}_cflags_maybe\"
          # don't try cflags_maybe if there's nothing set
          if test -z "$cflags_maybe"; then continue; fi
          cflags="$cflags_maybe $cflags"
        fi

        # Any user CFLAGS, even an empty string, takes precedence
        if test "$test_CFLAGS" = set; then cflags=$CFLAGS; fi

        # Any user CPPFLAGS, even an empty string, takes precedence
                               eval cppflags=\"\$${ccbase}${abi1}_cppflags\"
        test -n "$cppflags" || eval cppflags=\"\$${ccbase}${abi2}_cppflags\"
        if test "$test_CPPFLAGS" = set; then cppflags=$CPPFLAGS; fi

        # --enable-profiling adds -p/-pg even to user-specified CFLAGS.
        # This is convenient, but it's perhaps a bit naughty to modify user
        # CFLAGS.
        case "$enable_profiling" in
          prof)       cflags="$cflags -p" ;;
          gprof)      cflags="$cflags -pg" ;;
          instrument) cflags="$cflags -finstrument-functions" ;;
        esac

        GMP_PROG_CC_WORKS($cc $cflags $cppflags,,continue)

        # If we're supposed to be using a "long long" for a limb, check that
        # it works.
                                  eval limb_chosen=\"\$limb$abi1\"
        test -n "$limb_chosen" || eval limb_chosen=\"\$limb$abi2\"
        if test "$limb_chosen" = longlong; then
          GMP_PROG_CC_WORKS_LONGLONG($cc $cflags $cppflags,,continue)
        fi

        # The tests to perform on this $cc, if any
                               eval testlist=\"\$${ccbase}${abi1}_testlist\"
        test -n "$testlist" || eval testlist=\"\$${ccbase}${abi2}_testlist\"
        test -n "$testlist" || eval testlist=\"\$any${abi1}_testlist\"
        test -n "$testlist" || eval testlist=\"\$any${abi2}_testlist\"

        testlist_pass=yes
        for tst in $testlist; do
          case $tst in
          hpc-hppa-2-0)   GMP_HPC_HPPA_2_0($cc,,testlist_pass=no) ;;
          gcc-arm-umodsi) GMP_GCC_ARM_UMODSI($cc,,testlist_pass=no) ;;
          gcc-mips-o32)   GMP_GCC_MIPS_O32($cc,,testlist_pass=no) ;;
          hppa-level-2.0) GMP_HPPA_LEVEL_20($cc $cflags,,testlist_pass=no) ;;
          sizeof*)       GMP_C_TEST_SIZEOF($cc $cflags,$tst,,testlist_pass=no) ;;
          esac
          if test $testlist_pass = no; then break; fi
        done

        if test $testlist_pass = yes; then
          found_compiler=yes
          break
        fi
      done

      if test $found_compiler = yes; then break; fi
    done

    if test $found_compiler = yes; then break; fi
  done

  if test $found_compiler = yes; then break; fi
done


# If we recognised the CPU, as indicated by $path being set, then insist
# that we have a working compiler, either from our $cclist choices or from
# $CC.  We can't let AC_PROG_CC look around for a compiler because it might
# find one that we've rejected (for not supporting the modes our asm code
# demands, etc).
#
# If we didn't recognise the CPU (and this includes host_cpu=none), then
# fall through and let AC_PROG_CC look around for a compiler too.  This is
# mostly in the interests of following a standard autoconf setup, after all
# we've already tested cc and gcc adequately (hopefully).  As of autoconf
# 2.50 the only thing AC_PROG_CC really adds is a check for "cl" (Microsoft
# C on MS-DOS systems).
#
if test $found_compiler = no && test -n "$path"; then
  AC_MSG_ERROR([could not find a working compiler, see config.log for details])
fi

case $host in
  X86_PATTERN | X86_64_PATTERN)
    # If the user asked for a fat build, override the path and flags set above
    if test $enable_fat = yes; then
      gcc_cflags_cpu=""
      gcc_cflags_arch=""

      fat_functions="add_n addmul_1 bdiv_dbm1c com cnd_add_n cnd_sub_n
		     copyd copyi dive_1 divrem_1
		     gcd_1 lshift lshiftc mod_1 mod_1_1 mod_1_1_cps mod_1_2
		     mod_1_2_cps mod_1_4 mod_1_4_cps mod_34lsub1 mode1o mul_1
		     mul_basecase mullo_basecase pre_divrem_1 pre_mod_1 redc_1
		     redc_2 rshift sqr_basecase sub_n submul_1"

      if test "$abi" = 32; then
	extra_functions="$extra_functions fat fat_entry"
	path="x86/fat x86"
	fat_path="x86 x86/fat x86/i486
		  x86/k6 x86/k6/mmx x86/k6/k62mmx
		  x86/k7 x86/k7/mmx
		  x86/k8 x86/k10 x86/bobcat
		  x86/pentium x86/pentium/mmx
		  x86/p6 x86/p6/mmx x86/p6/p3mmx x86/p6/sse2
		  x86/pentium4 x86/pentium4/mmx x86/pentium4/sse2
		  x86/core2 x86/coreinhm x86/coreisbr
		  x86/atom x86/atom/mmx x86/atom/sse2 x86/nano"
      fi

      if test "$abi" = 64; then
	gcc_64_cflags=""
	extra_functions_64="$extra_functions_64 fat fat_entry"
	path_64="x86_64/fat x86_64"
	fat_path="x86_64 x86_64/fat
		  x86_64/k8 x86_64/k10 x86_64/bd1 x86_64/bobcat x86_64/pentium4
		  x86_64/core2 x86_64/coreinhm x86_64/coreisbr x86_64/coreihwl
		  x86_64/coreibwl x86_64/skylake x86_64/atom x86_64/silvermont
		  x86_64/nano"
	fat_functions="$fat_functions addmul_2 addlsh1_n addlsh2_n sublsh1_n"
      fi

      fat_thresholds="MUL_TOOM22_THRESHOLD MUL_TOOM33_THRESHOLD
		      SQR_TOOM2_THRESHOLD SQR_TOOM3_THRESHOLD
		      BMOD_1_TO_MOD_1_THRESHOLD"
    fi
    ;;
esac


if test $found_compiler = yes; then

  # If we're creating CFLAGS, then look for optional additions.  If the user
  # set CFLAGS then leave it alone.
  #
  if test "$test_CFLAGS" != set; then
                          eval optlist=\"\$${ccbase}${abi1}_cflags_optlist\"
    test -n "$optlist" || eval optlist=\"\$${ccbase}${abi2}_cflags_optlist\"

    for opt in $optlist; do
                             eval optflags=\"\$${ccbase}${abi1}_cflags_${opt}\"
      test -n "$optflags" || eval optflags=\"\$${ccbase}${abi2}_cflags_${opt}\"
      test -n "$optflags" || eval optflags=\"\$${ccbase}_cflags_${opt}\"

      for flag in $optflags; do

	# ~ represents a space in an option spec
        flag=`echo "$flag" | tr '~' ' '`

        case $flag in
          -march=pentium4 | -march=k8)
            # For -march settings which enable SSE2 we exclude certain bad
            # gcc versions and we need an OS knowing how to save xmm regs.
            #
            # This is only for ABI=32, any 64-bit gcc is good and any OS
            # knowing x86_64 will know xmm.
            #
            # -march=k8 was only introduced in gcc 3.3, so we shouldn't need
            # the GMP_GCC_PENTIUM4_SSE2 check (for gcc 3.2 and prior).  But
            # it doesn't hurt to run it anyway, sharing code with the
            # pentium4 case.
            #
            if test "$abi" = 32; then
              GMP_GCC_PENTIUM4_SSE2($cc $cflags $cppflags,, continue)
              GMP_OS_X86_XMM($cc $cflags $cppflags,, continue)
            fi
            ;;
          -no-cpp-precomp)
            # special check, avoiding a warning
            GMP_GCC_NO_CPP_PRECOMP($ccbase,$cc,$cflags,
                                   [cflags="$cflags $flag"
                                   break],
                                   [continue])
            ;;
          -Wa,-m*)
            case $host in
              alpha*-*-*)
                GMP_GCC_WA_MCPU($cc $cflags, $flag, , [continue])
              ;;
            esac
            ;;
          -Wa,-oldas)
            GMP_GCC_WA_OLDAS($cc $cflags $cppflags,
                             [cflags="$cflags $flag"
                             break],
                             [continue])
            ;;
        esac

        GMP_PROG_CC_WORKS($cc $cflags $cppflags $flag,
          [cflags="$cflags $flag"
          break])
      done
    done
  fi

  ABI="$abi"
  CC="$cc"
  CFLAGS="$cflags"
  CPPFLAGS="$cppflags"
  eval GMP_NONSTD_ABI=\"\$GMP_NONSTD_ABI_$ABI\"

  # Could easily have this in config.h too, if desired.
  ABI_nodots=`echo $ABI | sed 's/\./_/'`
  GMP_DEFINE_RAW("define_not_for_expansion(\`HAVE_ABI_$ABI_nodots')", POST)


  # GMP_LDFLAGS substitution, selected according to ABI.
  # These are needed on libgmp.la and libmp.la, but currently not on
  # convenience libraries like tune/libspeed.la or mpz/libmpz.la.
  #
                            eval GMP_LDFLAGS=\"\$${ccbase}${abi1}_ldflags\"
  test -n "$GMP_LDFLAGS" || eval GMP_LDFLAGS=\"\$${ccbase}${abi1}_ldflags\"
  AC_SUBST(GMP_LDFLAGS)
  AC_SUBST(LIBGMP_LDFLAGS)
  AC_SUBST(LIBGMPXX_LDFLAGS)

  # extra_functions, selected according to ABI
                    eval tmp=\"\$extra_functions$abi1\"
  test -n "$tmp" || eval tmp=\"\$extra_functions$abi2\"
  extra_functions="$tmp"


  # Cycle counter, selected according to ABI.
  #
                    eval tmp=\"\$SPEED_CYCLECOUNTER_OBJ$abi1\"
  test -n "$tmp" || eval tmp=\"\$SPEED_CYCLECOUNTER_OBJ$abi2\"
  SPEED_CYCLECOUNTER_OBJ="$tmp"
                    eval tmp=\"\$cyclecounter_size$abi1\"
  test -n "$tmp" || eval tmp=\"\$cyclecounter_size$abi2\"
  cyclecounter_size="$tmp"

  if test -n "$SPEED_CYCLECOUNTER_OBJ"; then
    AC_DEFINE_UNQUOTED(HAVE_SPEED_CYCLECOUNTER, $cyclecounter_size,
    [Tune directory speed_cyclecounter, undef=none, 1=32bits, 2=64bits)])
  fi
  AC_SUBST(SPEED_CYCLECOUNTER_OBJ)


  # Calling conventions checking, selected according to ABI.
  #
                    eval tmp=\"\$CALLING_CONVENTIONS_OBJS$abi1\"
  test -n "$tmp" || eval tmp=\"\$CALLING_CONVENTIONS_OBJS$abi2\"
  if test "$enable_assembly" = "yes"; then
     CALLING_CONVENTIONS_OBJS="$tmp"
  else
     CALLING_CONVENTIONS_OBJS=""
  fi

  if test -n "$CALLING_CONVENTIONS_OBJS"; then
    AC_DEFINE(HAVE_CALLING_CONVENTIONS,1,
    [Define to 1 if tests/libtests has calling conventions checking for the CPU])
  fi
  AC_SUBST(CALLING_CONVENTIONS_OBJS)

fi


# If the user gave an MPN_PATH, use that verbatim, otherwise choose
# according to the ABI and add "generic".
#
if test -n "$MPN_PATH"; then
  path="$MPN_PATH"
else
                    eval tmp=\"\$path$abi1\"
  test -n "$tmp" || eval tmp=\"\$path$abi2\"
  path="$tmp generic"
fi


# Long long limb setup for gmp.h.
case $limb_chosen in
longlong) DEFN_LONG_LONG_LIMB="#define _LONG_LONG_LIMB 1"    ;;
*)        DEFN_LONG_LONG_LIMB="/* #undef _LONG_LONG_LIMB */" ;;
esac
AC_SUBST(DEFN_LONG_LONG_LIMB)


# The C compiler and preprocessor, put into ANSI mode if possible.
AC_PROG_CC
AC_PROG_CC_STDC
AC_PROG_CPP


# The C compiler on the build system, and associated tests.
GMP_PROG_CC_FOR_BUILD
GMP_PROG_CPP_FOR_BUILD
GMP_PROG_EXEEXT_FOR_BUILD
GMP_C_FOR_BUILD_ANSI
GMP_CHECK_LIBM_FOR_BUILD


# How to assemble, used with CFLAGS etc, see mpn/Makeasm.am.
# Using the compiler is a lot easier than figuring out how to invoke the
# assembler directly.
#
test -n "$CCAS" || CCAS="$CC -c"
AC_SUBST(CCAS)


# The C++ compiler, if desired.
want_cxx=no
if test $enable_cxx != no; then
  test_CXXFLAGS=${CXXFLAGS+set}
  AC_PROG_CXX

  echo "CXXFLAGS chosen by autoconf: $CXXFLAGS" >&AC_FD_CC
  cxxflags_ac_prog_cxx=$CXXFLAGS
  cxxflags_list=ac_prog_cxx

  # If the user didn't specify $CXXFLAGS, then try $CFLAGS, with -g removed
  # if AC_PROG_CXX thinks that doesn't work.  $CFLAGS stands a good chance
  # of working, eg. on a GNU system where CC=gcc and CXX=g++.
  #
  if test "$test_CXXFLAGS" != set; then
    cxxflags_cflags=$CFLAGS
    cxxflags_list="cflags $cxxflags_list"
    if test "$ac_prog_cxx_g" = no; then
      cxxflags_cflags=`echo "$cxxflags_cflags" | sed -e 's/ -g //' -e 's/^-g //' -e 's/ -g$//'`
    fi
  fi

  # See if the C++ compiler works.  If the user specified CXXFLAGS then all
  # we're doing is checking whether AC_PROG_CXX succeeded, since it doesn't
  # give a fatal error, just leaves CXX set to a default g++.  If on the
  # other hand the user didn't specify CXXFLAGS then we get to try here our
  # $cxxflags_list alternatives.
  #
  # Automake includes $CPPFLAGS in a C++ compile, so we do the same here.
  #
  for cxxflags_choice in $cxxflags_list; do
    eval CXXFLAGS=\"\$cxxflags_$cxxflags_choice\"
    GMP_PROG_CXX_WORKS($CXX $CPPFLAGS $CXXFLAGS,
      [want_cxx=yes
      break])
  done

  # If --enable-cxx=yes but a C++ compiler can't be found, then abort.
  if test $want_cxx = no && test $enable_cxx = yes; then
    AC_MSG_ERROR([C++ compiler not available, see config.log for details])
  fi
fi

AM_CONDITIONAL(WANT_CXX, test $want_cxx = yes)

# FIXME: We're not interested in CXXCPP for ourselves, but if we don't do it
# here then AC_PROG_LIBTOOL will AC_REQUIRE it (via _LT_AC_TAGCONFIG) and
# hence execute it unconditionally, and that will fail if there's no C++
# compiler (and no generic /lib/cpp).
#
if test $want_cxx = yes; then
  AC_PROG_CXXCPP
fi


# Path setups for Cray, according to IEEE or CFP.  These must come after
# deciding the compiler.
#
GMP_CRAY_OPTIONS(
  [add_path="cray/ieee"],
  [add_path="cray/cfp"; extra_functions="mulwwc90"],
  [add_path="cray/cfp"; extra_functions="mulwwj90"])


if test -z "$MPN_PATH"; then
  path="$add_path $path"
fi

# For a nail build, also look in "nails" subdirectories.
#
if test $GMP_NAIL_BITS != 0 && test -z "$MPN_PATH"; then
  new_path=
  for i in $path; do
    case $i in
    generic) new_path="$new_path $i" ;;
    *)       new_path="$new_path $i/nails $i" ;;
    esac
  done
  path=$new_path
fi


# Put all directories into CPUVEC_list so as to get a full set of
# CPUVEC_SETUP_$tmp_suffix defines into config.h, even if some of them are
# empty because mmx and/or sse2 had to be dropped.
#
for i in $fat_path; do
  GMP_FAT_SUFFIX(tmp_suffix, $i)
  CPUVEC_list="$CPUVEC_list CPUVEC_SETUP_$tmp_suffix"
done


# If there's any sse2 or mmx in the path, check whether the assembler
# supports it, and remove if not.
#
# We only need this in ABI=32, for ABI=64 on x86_64 we can assume a new
# enough assembler.
#
case $host in
  X86_PATTERN | X86_64_PATTERN)
    if test "$ABI" = 32; then
      case "$path $fat_path" in
        *mmx*)   GMP_ASM_X86_MMX( , [GMP_STRIP_PATH(*mmx*)]) ;;
      esac
      case "$path $fat_path" in
        *sse2*)  GMP_ASM_X86_SSE2( , [GMP_STRIP_PATH(sse2)]) ;;
      esac
    fi
    case "$path $fat_path" in
      *mulx*)  GMP_ASM_X86_MULX( , [GMP_STRIP_PATH(mulx)]) ;;
    esac
    case "$path $fat_path" in
      *adx*)   GMP_ASM_X86_ADX( , [GMP_STRIP_PATH(adx)]) ;;
    esac
    ;;
esac


if test "$enable_assembly" = "no"; then
  path="generic"
  AC_DEFINE([NO_ASM],1,[Define to 1 to disable the use of inline assembly])
#  for abi in $abilist; do
#    eval unset "path_\$abi"
#    eval gcc_${abi}_cflags=\"\$gcc_${abi}_cflags -DNO_ASM\"
#  done
fi


cat >&AC_FD_CC <<EOF
Decided:
ABI=$ABI
CC=$CC
CFLAGS=$CFLAGS
CPPFLAGS=$CPPFLAGS
GMP_LDFLAGS=$GMP_LDFLAGS
CXX=$CXX
CXXFLAGS=$CXXFLAGS
path=$path
EOF
echo "using ABI=\"$ABI\""
echo "      CC=\"$CC\""
echo "      CFLAGS=\"$CFLAGS\""
echo "      CPPFLAGS=\"$CPPFLAGS\""
if test $want_cxx = yes; then
  echo "      CXX=\"$CXX\""
  echo "      CXXFLAGS=\"$CXXFLAGS\""
fi
echo "      MPN_PATH=\"$path\""


CL_AS_NOEXECSTACK

GMP_PROG_AR
GMP_PROG_NM

case $host in
  # FIXME: On AIX 3 and 4, $libname.a is included in libtool
  # $library_names_spec, so libgmp.a becomes a symlink to libgmp.so, making
  # it impossible to build shared and static libraries simultaneously.
  # Disable shared libraries by default, but let the user override with
  # --enable-shared --disable-static.
  #
  # FIXME: This $libname.a problem looks like it might apply to *-*-amigaos*
  # and *-*-os2* too, but wait for someone to test this before worrying
  # about it.  If there is a problem then of course libtool is the right
  # place to fix it.
  #
  [*-*-aix[34]*])
    if test -z "$enable_shared"; then enable_shared=no; fi ;;
esac


# Configs for Windows DLLs.

AC_LIBTOOL_WIN32_DLL

AC_SUBST(LIBGMP_DLL,0)
case $host in
  *-*-cygwin* | *-*-mingw* | *-*-pw32* | *-*-os2*)
    # By default, build only static.
    if test -z "$enable_shared"; then
      enable_shared=no
    fi
    # Don't allow both static and DLL.
    if test "$enable_shared" != no && test "$enable_static" != no; then
      AC_MSG_ERROR([cannot build both static and DLL, since gmp.h is different for each.
Use "--disable-static --enable-shared" to build just a DLL.])
    fi

    # "-no-undefined" is required when building a DLL, see documentation on
    # AC_LIBTOOL_WIN32_DLL.
    #
    # "-Wl,--export-all-symbols" is a bit of a hack, it gets all libgmp and
    # libgmpxx functions and variables exported.  This is what libtool did
    # in the past, and it's convenient for us in the test programs.
    #
    # Maybe it'd be prudent to check for --export-all-symbols before using
    # it, but it seems to have been in ld since at least 2000, and there's
    # not really any alternative we want to take up at the moment.
    #
    # "-Wl,output-def" is used to get a .def file for use by MS lib to make
    # a .lib import library, described in the manual.  libgmp-3.dll.def
    # corresponds to the libmp-3.dll.def generated by libtool (as a result
    # of -export-symbols on that library).
    #
    # Incidentally, libtool does generate an import library libgmp.dll.a,
    # but it's "ar" format and cannot be used by the MS linker.  There
    # doesn't seem to be any GNU tool for generating or converting to .lib.
    #
    # FIXME: The .def files produced by -Wl,output-def include isascii,
    # iscsym, iscsymf and toascii, apparently because mingw ctype.h doesn't
    # inline isascii (used in gmp).  It gives an extern inline for
    # __isascii, but for some reason not the plain isascii.
    #
    if test "$enable_shared" = yes; then
      GMP_LDFLAGS="$GMP_LDFLAGS -no-undefined -Wl,--export-all-symbols"
      LIBGMP_LDFLAGS="$LIBGMP_LDFLAGS -Wl,--output-def,.libs/libgmp-3.dll.def"
      LIBGMPXX_LDFLAGS="$LIBGMP_LDFLAGS -Wl,--output-def,.libs/libgmpxx-3.dll.def"
      LIBGMP_DLL=1
    fi
    ;;
esac


# Ensure that $CONFIG_SHELL is available for AC_LIBTOOL_SYS_MAX_CMD_LEN.
# It's often set already by _LT_AC_PROG_ECHO_BACKSLASH or
# _AS_LINENO_PREPARE, but not always.
#
# The symptom of CONFIG_SHELL unset is some "expr" errors during the test,
# and an empty result.  This only happens when invoked as "sh configure",
# ie. no path, and can be seen for instance on ia64-*-hpux*.
#
# FIXME: Newer libtool should have it's own fix for this.
#
if test -z "$CONFIG_SHELL"; then
  CONFIG_SHELL=$SHELL
fi

# Enable CXX in libtool only if we want it, and never enable GCJ, nor RC on
# mingw and cygwin.  Under --disable-cxx this avoids some error messages
# from libtool arising from the fact we didn't actually run AC_PROG_CXX.
# Notice that any user-supplied --with-tags setting takes precedence.
#
# FIXME: Is this the right way to get this effect?  Very possibly not, but
# the current _LT_AC_TAGCONFIG doesn't really suggest an alternative.
#
if test "${with_tags+set}" != set; then
  if test $want_cxx = yes; then
    with_tags=CXX
  else
    with_tags=
  fi
fi

# The dead hand of AC_REQUIRE makes AC_PROG_LIBTOOL expand and execute
# AC_PROG_F77, even when F77 is not in the selected with_tags.  This is
# probably harmless, but it's unsightly and bloats our configure, so pretend
# AC_PROG_F77 has been expanded already.
#
# FIXME: Rumour has it libtool will one day provide a way for a configure.in
# to say what it wants from among supported languages etc.
#
#AC_PROVIDE([AC_PROG_F77])

AC_PROG_LIBTOOL

# Generate an error here if attempting to build both shared and static when
# $libname.a is in $library_names_spec (as mentioned above), rather than
# wait for ar or ld to fail.
#
if test "$enable_shared" = yes && test "$enable_static" = yes; then
  case $library_names_spec in
    *libname.a*)
      AC_MSG_ERROR([cannot create both shared and static libraries on this system, --disable one of the two])
      ;;
  esac
fi

AM_CONDITIONAL(ENABLE_STATIC, test "$enable_static" = yes)


# Many of these library and header checks are for the benefit of
# supplementary programs.  libgmp doesn't use anything too weird.

AC_HEADER_STDC
AC_HEADER_TIME

# Reasons for testing:
#   float.h - not in SunOS bundled cc
#   invent.h - IRIX specific
#   langinfo.h - X/Open standard only, not in djgpp for instance
#   locale.h - old systems won't have this
#   nl_types.h - X/Open standard only, not in djgpp for instance
#       (usually langinfo.h gives nl_item etc, but not on netbsd 1.4.1)
#   sys/attributes.h - IRIX specific
#   sys/iograph.h - IRIX specific
#   sys/mman.h - not in Cray Unicos
#   sys/param.h - not in mingw
#   sys/processor.h - solaris specific, though also present in macos
#   sys/pstat.h - HPUX specific
#   sys/resource.h - not in mingw
#   sys/sysctl.h - not in mingw
#   sys/sysinfo.h - OSF specific
#   sys/syssgi.h - IRIX specific
#   sys/systemcfg.h - AIX specific
#   sys/time.h - autoconf suggests testing, don't know anywhere without it
#   sys/times.h - not in mingw
#   machine/hal_sysinfo.h - OSF specific
#
# inttypes.h, stdint.h, unistd.h and sys/types.h are already in the autoconf
# default tests
#
AC_CHECK_HEADERS(fcntl.h float.h invent.h langinfo.h locale.h nl_types.h sys/attributes.h sys/iograph.h sys/mman.h sys/param.h sys/processor.h sys/pstat.h sys/sysinfo.h sys/syssgi.h sys/systemcfg.h sys/time.h sys/times.h)

# On SunOS, sys/resource.h needs sys/time.h (for struct timeval)
AC_CHECK_HEADERS(sys/resource.h,,,
[#if TIME_WITH_SYS_TIME
# include <sys/time.h>
# include <time.h>
#else
# if HAVE_SYS_TIME_H
#  include <sys/time.h>
# else
#  include <time.h>
# endif
#endif])

# On NetBSD and OpenBSD, sys/sysctl.h needs sys/param.h for various constants
AC_CHECK_HEADERS(sys/sysctl.h,,,
[#if HAVE_SYS_PARAM_H
# include <sys/param.h>
#endif])

# On OSF 4.0, <machine/hal_sysinfo.h> must have <sys/sysinfo.h> for ulong_t
AC_CHECK_HEADERS(machine/hal_sysinfo.h,,,
[#if HAVE_SYS_SYSINFO_H
# include <sys/sysinfo.h>
#endif])

# Reasons for testing:
#   optarg - not declared in mingw
#   fgetc, fscanf, ungetc, vfprintf - not declared in SunOS 4
#   sys_errlist, sys_nerr - not declared in SunOS 4
#
# optarg should be in unistd.h and the rest in stdio.h, both of which are
# in the autoconf default includes.
#
# sys_errlist and sys_nerr are supposed to be in <errno.h> on SunOS according
# to the man page (but aren't), in glibc they're in stdio.h.
#
AC_CHECK_DECLS([fgetc, fscanf, optarg, ungetc, vfprintf])
AC_CHECK_DECLS([sys_errlist, sys_nerr], , ,
[#include <stdio.h>
#include <errno.h>])

AC_TYPE_SIGNAL

# Reasons for testing:
#   intmax_t       - C99
#   long double    - not in the HP bundled K&R cc
#   long long      - only in reasonably recent compilers
#   ptrdiff_t      - seems to be everywhere, maybe don't need to check this
#   quad_t         - BSD specific
#   uint_least32_t - C99
#
# the default includes are sufficient for all these types
#
AC_CHECK_TYPES([intmax_t, long double, long long, ptrdiff_t, quad_t,
		uint_least32_t, intptr_t])

# FIXME: Really want #ifndef __cplusplus around the #define volatile
# replacement autoconf gives, since volatile is always available in C++.
# But we don't use it in C++ currently.
AC_C_VOLATILE

AC_C_RESTRICT

# GMP_C_STDARG
GMP_C_ATTRIBUTE_CONST
GMP_C_ATTRIBUTE_MALLOC
GMP_C_ATTRIBUTE_MODE
GMP_C_ATTRIBUTE_NORETURN
GMP_C_HIDDEN_ALIAS

GMP_H_EXTERN_INLINE

# from libtool
AC_CHECK_LIBM
AC_SUBST(LIBM)

GMP_FUNC_ALLOCA
GMP_OPTION_ALLOCA

GMP_H_HAVE_FILE

AC_C_BIGENDIAN(
  [AC_DEFINE(HAVE_LIMB_BIG_ENDIAN, 1)
   GMP_DEFINE_RAW("define_not_for_expansion(\`HAVE_LIMB_BIG_ENDIAN')", POST)],
  [AC_DEFINE(HAVE_LIMB_LITTLE_ENDIAN, 1)
   GMP_DEFINE_RAW("define_not_for_expansion(\`HAVE_LIMB_LITTLE_ENDIAN')", POST)
  ], [:])
AH_VERBATIM([HAVE_LIMB],
[/* Define one of these to 1 for the endianness of `mp_limb_t'.
   If the endianness is not a simple big or little, or you don't know what
   it is, then leave both undefined. */
#undef HAVE_LIMB_BIG_ENDIAN
#undef HAVE_LIMB_LITTLE_ENDIAN])

GMP_C_DOUBLE_FORMAT


# Reasons for testing:
#   alarm - not in mingw
#   attr_get - IRIX specific
#   clock_gettime - not in glibc 2.2.4, only very recent systems
#   cputime - not in glibc
#   getsysinfo - OSF specific
#   getrusage - not in mingw
#   gettimeofday - not in mingw
#   mmap - not in mingw, djgpp
#   nl_langinfo - X/Open standard only, not in djgpp for instance
#   obstack_vprintf - glibc specific
#   processor_info - solaris specific
#   pstat_getprocessor - HPUX specific (10.x and up)
#   raise - an ANSI-ism, though probably almost universal by now
#   read_real_time - AIX specific
#   sigaction - not in mingw
#   sigaltstack - not in mingw, or old AIX (reputedly)
#   sigstack - not in mingw
#   strerror - not in SunOS
#   strnlen - glibc extension (some other systems too)
#   syssgi - IRIX specific
#   times - not in mingw
#
# AC_FUNC_STRNLEN is not used because we don't want the AC_LIBOBJ
# replacement setups it gives.  It detects a faulty strnlen on AIX, but
# missing out on that test is ok since our only use of strnlen is in
# __gmp_replacement_vsnprintf which is not required on AIX since it has a
# vsnprintf.
#
AC_CHECK_FUNCS(alarm attr_get clock cputime getpagesize getrusage gettimeofday getsysinfo localeconv memset mmap mprotect nl_langinfo obstack_vprintf popen processor_info pstat_getprocessor raise read_real_time sigaction sigaltstack sigstack syssgi strchr strerror strnlen strtol strtoul sysconf sysctl sysctlbyname times)

# Reasons for checking:
#   pst_processor psp_iticksperclktick - not in hpux 9
#
AC_CHECK_MEMBER(struct pst_processor.psp_iticksperclktick,
                [AC_DEFINE(HAVE_PSP_ITICKSPERCLKTICK, 1,
[Define to 1 if <sys/pstat.h> `struct pst_processor' exists
and contains `psp_iticksperclktick'.])],,
                [#include <sys/pstat.h>])

# C++ tests, when required
#
if test $enable_cxx = yes; then
  AC_LANG_PUSH(C++)

  # Reasons for testing:
  #   <sstream> - not in g++ 2.95.2
  #   std::locale - not in g++ 2.95.4
  #
  AC_CHECK_HEADERS([sstream])
  AC_CHECK_TYPES([std::locale],,,[#include <locale>])

  AC_LANG_POP(C++)
fi


# Pick the correct source files in $path and link them to mpn/.
# $gmp_mpn_functions lists all functions we need.
#
# The rule is to find a file with the function name and a .asm, .S,
# .s, or .c extension.  Certain multi-function files with special names
# can provide some functions too.  (mpn/Makefile.am passes
# -DOPERATION_<func> to get them to generate the right code.)

# Note: $gmp_mpn_functions must have mod_1 before pre_mod_1 so the former
#       can optionally provide the latter as an extra entrypoint.  Likewise
#       divrem_1 and pre_divrem_1.

gmp_mpn_functions_optional="umul udiv					\
  invert_limb sqr_diagonal sqr_diag_addlsh1				\
  mul_2 mul_3 mul_4 mul_5 mul_6						\
  addmul_2 addmul_3 addmul_4 addmul_5 addmul_6 addmul_7 addmul_8	\
  addlsh1_n sublsh1_n rsblsh1_n rsh1add_n rsh1sub_n			\
  addlsh2_n sublsh2_n rsblsh2_n						\
  addlsh_n sublsh_n rsblsh_n						\
  add_n_sub_n addaddmul_1msb0"

gmp_mpn_functions="$extra_functions					   \
  add add_1 add_n sub sub_1 sub_n cnd_add_n cnd_sub_n cnd_swap neg com	   \
  mul_1 addmul_1 submul_1						   \
  add_err1_n add_err2_n add_err3_n sub_err1_n sub_err2_n sub_err3_n	   \
  lshift rshift dive_1 diveby3 divis divrem divrem_1 divrem_2		   \
  fib2_ui mod_1 mod_34lsub1 mode1o pre_divrem_1 pre_mod_1 dump		   \
  mod_1_1 mod_1_2 mod_1_3 mod_1_4 lshiftc				   \
  mul mul_fft mul_n sqr mul_basecase sqr_basecase nussbaumer_mul	   \
  mulmid_basecase toom42_mulmid mulmid_n mulmid				   \
  random random2 pow_1							   \
  rootrem sqrtrem sizeinbase get_str set_str				   \
  scan0 scan1 popcount hamdist cmp zero_p				   \
  perfsqr perfpow							   \
  gcd_1 gcd gcdext_1 gcdext gcd_subdiv_step				   \
  gcdext_lehmer								   \
  div_q tdiv_qr jacbase jacobi_2 jacobi get_d				   \
  matrix22_mul matrix22_mul1_inverse_vector				   \
  hgcd_matrix hgcd2 hgcd_step hgcd_reduce hgcd hgcd_appr		   \
  hgcd2_jacobi hgcd_jacobi						   \
  mullo_n mullo_basecase sqrlo sqrlo_basecase				   \
  toom22_mul toom32_mul toom42_mul toom52_mul toom62_mul		   \
  toom33_mul toom43_mul toom53_mul toom54_mul toom63_mul		   \
  toom44_mul								   \
  toom6h_mul toom6_sqr toom8h_mul toom8_sqr				   \
  toom_couple_handling							   \
  toom2_sqr toom3_sqr toom4_sqr						   \
  toom_eval_dgr3_pm1 toom_eval_dgr3_pm2					   \
  toom_eval_pm1 toom_eval_pm2 toom_eval_pm2exp toom_eval_pm2rexp	   \
  toom_interpolate_5pts toom_interpolate_6pts toom_interpolate_7pts	   \
  toom_interpolate_8pts toom_interpolate_12pts toom_interpolate_16pts	   \
  invertappr invert binvert mulmod_bnm1 sqrmod_bnm1			   \
  div_qr_1 div_qr_1n_pi1						   \
  div_qr_2 div_qr_2n_pi1 div_qr_2u_pi1					   \
  sbpi1_div_q sbpi1_div_qr sbpi1_divappr_q				   \
  dcpi1_div_q dcpi1_div_qr dcpi1_divappr_q				   \
  mu_div_qr mu_divappr_q mu_div_q					   \
  bdiv_q_1								   \
  sbpi1_bdiv_q sbpi1_bdiv_qr						   \
  dcpi1_bdiv_q dcpi1_bdiv_qr						   \
  mu_bdiv_q mu_bdiv_qr							   \
  bdiv_q bdiv_qr broot brootinv bsqrt bsqrtinv				   \
  divexact bdiv_dbm1c redc_1 redc_2 redc_n powm powlo sec_powm		   \
  sec_mul sec_sqr sec_div_qr sec_div_r sec_pi1_div_qr sec_pi1_div_r	   \
  sec_add_1 sec_sub_1 sec_invert					   \
  trialdiv remove							   \
  and_n andn_n nand_n ior_n iorn_n nior_n xor_n xnor_n			   \
  copyi copyd zero sec_tabselect					   \
  comb_tables								   \
  $gmp_mpn_functions_optional"

define(GMP_MULFUNC_CHOICES,
[# functions that can be provided by multi-function files
tmp_mulfunc=
case $tmp_fn in
  add_n|sub_n)       tmp_mulfunc="aors_n"    ;;
  add_err1_n|sub_err1_n)
		     tmp_mulfunc="aors_err1_n" ;;
  add_err2_n|sub_err2_n)
		     tmp_mulfunc="aors_err2_n" ;;
  add_err3_n|sub_err3_n)
		     tmp_mulfunc="aors_err3_n" ;;
  cnd_add_n|cnd_sub_n) tmp_mulfunc="cnd_aors_n"   ;;
  sec_add_1|sec_sub_1) tmp_mulfunc="sec_aors_1"   ;;
  addmul_1|submul_1) tmp_mulfunc="aorsmul_1" ;;
  mul_2|addmul_2)    tmp_mulfunc="aormul_2" ;;
  mul_3|addmul_3)    tmp_mulfunc="aormul_3" ;;
  mul_4|addmul_4)    tmp_mulfunc="aormul_4" ;;
  popcount|hamdist)  tmp_mulfunc="popham"    ;;
  and_n|andn_n|nand_n | ior_n|iorn_n|nior_n | xor_n|xnor_n)
                     tmp_mulfunc="logops_n"  ;;
  lshift|rshift)     tmp_mulfunc="lorrshift";;
  addlsh1_n)
		     tmp_mulfunc="aorslsh1_n aorrlsh1_n aorsorrlsh1_n";;
  sublsh1_n)
		     tmp_mulfunc="aorslsh1_n sorrlsh1_n aorsorrlsh1_n";;
  rsblsh1_n)
		     tmp_mulfunc="aorrlsh1_n sorrlsh1_n aorsorrlsh1_n";;
  addlsh2_n)
		     tmp_mulfunc="aorslsh2_n aorrlsh2_n aorsorrlsh2_n";;
  sublsh2_n)
		     tmp_mulfunc="aorslsh2_n sorrlsh2_n aorsorrlsh2_n";;
  rsblsh2_n)
		     tmp_mulfunc="aorrlsh2_n sorrlsh2_n aorsorrlsh2_n";;
  addlsh_n)
		     tmp_mulfunc="aorslsh_n aorrlsh_n aorsorrlsh_n";;
  sublsh_n)
		     tmp_mulfunc="aorslsh_n sorrlsh_n aorsorrlsh_n";;
  rsblsh_n)
		     tmp_mulfunc="aorrlsh_n sorrlsh_n aorsorrlsh_n";;
  rsh1add_n|rsh1sub_n)
		     tmp_mulfunc="rsh1aors_n";;
  sec_div_qr|sec_div_r)
		     tmp_mulfunc="sec_div";;
  sec_pi1_div_qr|sec_pi1_div_r)
		     tmp_mulfunc="sec_pi1_div";;
esac
])

# the list of all object files used by mpn/Makefile.in and the
# top-level Makefile.in, respectively
mpn_objects=
mpn_objs_in_libgmp=

# links from the sources, to be removed by "make distclean"
gmp_srclinks=


# mpn_relative_top_srcdir is $top_srcdir, but for use from within the mpn
# build directory.  If $srcdir is relative then we use a relative path too,
# so the two trees can be moved together.
case $srcdir in
  [[\\/]* | ?:[\\/]*])  # absolute, as per autoconf
    mpn_relative_top_srcdir=$srcdir ;;
  *)                    # relative
    mpn_relative_top_srcdir=../$srcdir ;;
esac


define(MPN_SUFFIXES,[asm S s c])

dnl  Usage: GMP_FILE_TO_FUNCTION_BASE(func,file)
dnl
dnl  Set $func to the function base name for $file, eg. dive_1 gives
dnl  divexact_1.
dnl
define(GMP_FILE_TO_FUNCTION,
[case $$2 in
  dive_1)	$1=divexact_1 ;;
  diveby3)	$1=divexact_by3c ;;
  pre_divrem_1) $1=preinv_divrem_1 ;;
  mode1o)	$1=modexact_1c_odd ;;
  pre_mod_1)	$1=preinv_mod_1 ;;
  mod_1_1)	$1=mod_1_1p ;;
  mod_1_1_cps)	$1=mod_1_1p_cps ;;
  mod_1_2)	$1=mod_1s_2p ;;
  mod_1_2_cps)	$1=mod_1s_2p_cps ;;
  mod_1_3)	$1=mod_1s_3p ;;
  mod_1_3_cps)	$1=mod_1s_3p_cps ;;
  mod_1_4)	$1=mod_1s_4p ;;
  mod_1_4_cps)	$1=mod_1s_4p_cps ;;
  *)		$1=$$2 ;;
esac
])

# Fat binary setups.
#
# We proceed through each $fat_path directory, and look for $fat_function
# routines there.  Those found are incorporated in the build by generating a
# little mpn/<foo>.asm or mpn/<foo>.c file in the build directory, with
# suitable function renaming, and adding that to $mpn_objects (the same as a
# normal mpn file).
#
# fat.h is generated with macros to let internal calls to each $fat_function
# go directly through __gmpn_cpuvec, plus macros and declarations helping to
# setup that structure, on a per-directory basis ready for
# mpn/<cpu>/fat/fat.c.
#
# fat.h includes thresholds listed in $fat_thresholds, extracted from
# gmp-mparam.h in each directory.  An overall maximum for each threshold is
# established, for use in making fixed size arrays of temporary space.
# (Eg. MUL_TOOM33_THRESHOLD_LIMIT used by mpn/generic/mul.c.)
#
# It'd be possible to do some of this manually, but when there's more than a
# few functions and a few directories it becomes very tedious, and very
# prone to having some routine accidentally omitted.  On that basis it seems
# best to automate as much as possible, even if the code to do so is a bit
# ugly.
#

if test -n "$fat_path"; then
  # Usually the mpn build directory is created with mpn/Makefile
  # instantiation, but we want to write to it sooner.
  mkdir mpn 2>/dev/null

  echo "/* fat.h - setups for fat binaries." >fat.h
  echo "   Generated by configure - DO NOT EDIT.  */" >>fat.h

  AC_DEFINE(WANT_FAT_BINARY, 1, [Define to 1 when building a fat binary.])
  GMP_DEFINE(WANT_FAT_BINARY, yes)

  # Don't want normal copies of fat functions
  for tmp_fn in $fat_functions; do
    GMP_REMOVE_FROM_LIST(gmp_mpn_functions, $tmp_fn)
    GMP_REMOVE_FROM_LIST(gmp_mpn_functions_optional, $tmp_fn)
  done

  for tmp_fn in $fat_functions; do
    GMP_FILE_TO_FUNCTION(tmp_fbase,tmp_fn)
    echo "
#ifndef OPERATION_$tmp_fn
#undef  mpn_$tmp_fbase
#define mpn_$tmp_fbase  (*__gmpn_cpuvec.$tmp_fbase)
#endif
DECL_$tmp_fbase (__MPN(${tmp_fbase}_init));" >>fat.h
    # encourage various macros to use fat functions
    AC_DEFINE_UNQUOTED(HAVE_NATIVE_mpn_$tmp_fbase)
  done

  echo "" >>fat.h
  echo "/* variable thresholds */" >>fat.h
  for tmp_tn in $fat_thresholds; do
    echo "#undef  $tmp_tn" >>fat.h
    echo "#define $tmp_tn  CPUVEC_THRESHOLD (`echo $tmp_tn | tr [A-Z] [a-z]`)" >>fat.h
  done

  echo "
/* Copy all fields into __gmpn_cpuvec.
   memcpy is not used because it might operate byte-wise (depending on its
   implementation), and we need the function pointer writes to be atomic.
   "volatile" discourages the compiler from trying to optimize this.  */
#define CPUVEC_INSTALL(vec) \\
  do { \\
    volatile struct cpuvec_t *p = &__gmpn_cpuvec; \\" >>fat.h
  for tmp_fn in $fat_functions; do
    GMP_FILE_TO_FUNCTION(tmp_fbase,tmp_fn)
    echo "    p->$tmp_fbase = vec.$tmp_fbase; \\" >>fat.h
  done
  for tmp_tn in $fat_thresholds; do
    tmp_field_name=`echo $tmp_tn | tr [[A-Z]] [[a-z]]`
    echo "    p->$tmp_field_name = vec.$tmp_field_name; \\" >>fat.h
  done
  echo "  } while (0)" >>fat.h

  echo "
/* A helper to check all fields are filled. */
#define ASSERT_CPUVEC(vec) \\
  do { \\" >>fat.h
  for tmp_fn in $fat_functions; do
    GMP_FILE_TO_FUNCTION(tmp_fbase,tmp_fn)
    echo "    ASSERT (vec.$tmp_fbase != NULL); \\" >>fat.h
  done
  for tmp_tn in $fat_thresholds; do
    tmp_field_name=`echo $tmp_tn | tr [[A-Z]] [[a-z]]`
    echo "    ASSERT (vec.$tmp_field_name != 0); \\" >>fat.h
  done
  echo "  } while (0)" >>fat.h

  echo "
/* Call ITERATE(field) for each fat threshold field. */
#define ITERATE_FAT_THRESHOLDS() \\
  do { \\" >>fat.h
  for tmp_tn in $fat_thresholds; do
    tmp_field_name=`echo $tmp_tn | tr [[A-Z]] [[a-z]]`
    echo "    ITERATE ($tmp_tn, $tmp_field_name); \\" >>fat.h
  done
  echo "  } while (0)" >>fat.h

  for tmp_dir in $fat_path; do
    CPUVEC_SETUP=
    THRESH_ASM_SETUP=
    echo "" >>fat.h
    GMP_FAT_SUFFIX(tmp_suffix, $tmp_dir)

    # In order to keep names unique on a DOS 8.3 filesystem, use a prefix
    # (rather than a suffix) for the generated file names, and abbreviate.
    case $tmp_suffix in
      pentium)       tmp_prefix=p   ;;
      pentium_mmx)   tmp_prefix=pm  ;;
      p6_mmx)        tmp_prefix=p2  ;;
      p6_p3mmx)      tmp_prefix=p3  ;;
      pentium4)      tmp_prefix=p4  ;;
      pentium4_mmx)  tmp_prefix=p4m ;;
      pentium4_sse2) tmp_prefix=p4s ;;
      k6_mmx)        tmp_prefix=k6m ;;
      k6_k62mmx)     tmp_prefix=k62 ;;
      k7_mmx)        tmp_prefix=k7m ;;
      *)             tmp_prefix=$tmp_suffix ;;
    esac

    # Extract desired thresholds from gmp-mparam.h file in this directory,
    # if present.
    tmp_mparam=$srcdir/mpn/$tmp_dir/gmp-mparam.h
    if test -f $tmp_mparam; then
      for tmp_tn in $fat_thresholds; do
        tmp_thresh=`sed -n "s/^#define $tmp_tn[ 	]*\\([0-9][0-9]*\\).*$/\\1/p" $tmp_mparam`
        if test -n "$tmp_thresh"; then
          THRESH_ASM_SETUP=["${THRESH_ASM_SETUP}define($tmp_tn,$tmp_thresh)
"]
          CPUVEC_SETUP="$CPUVEC_SETUP    decided_cpuvec.`echo $tmp_tn | tr [[A-Z]] [[a-z]]` = $tmp_thresh; \\
"
          eval tmp_limit=\$${tmp_tn}_LIMIT
          if test -z "$tmp_limit"; then
            tmp_limit=0
          fi
          if test $tmp_thresh -gt $tmp_limit; then
            eval ${tmp_tn}_LIMIT=$tmp_thresh
          fi
        fi
      done
    fi

    for tmp_fn in $fat_functions; do
      GMP_MULFUNC_CHOICES

      for tmp_base in $tmp_fn $tmp_mulfunc; do
        for tmp_ext in MPN_SUFFIXES; do
          tmp_file=$srcdir/mpn/$tmp_dir/$tmp_base.$tmp_ext
          if test -f $tmp_file; then

	    # If the host uses a non-standard ABI, check if tmp_file supports it
	    #
	    if test -n "$GMP_NONSTD_ABI" && test $tmp_ext != "c"; then
	      abi=[`sed -n 's/^[ 	]*ABI_SUPPORT(\(.*\))/\1/p' $tmp_file `]
	      if echo "$abi" | grep -q "\\b${GMP_NONSTD_ABI}\\b"; then
		true
	      else
		continue
	      fi
	    fi

            mpn_objects="$mpn_objects ${tmp_prefix}_$tmp_fn.lo"
            mpn_objs_in_libgmp="$mpn_objs_in_libgmp mpn/${tmp_prefix}_$tmp_fn.lo"

            GMP_FILE_TO_FUNCTION(tmp_fbase,tmp_fn)

            # carry-in variant, eg. divrem_1c or modexact_1c_odd
            case $tmp_fbase in
              *_1*) tmp_fbasec=`echo $tmp_fbase | sed 's/_1/_1c/'` ;;
              *)    tmp_fbasec=${tmp_fbase}c ;;
            esac

            # Create a little file doing an include from srcdir.  The
            # OPERATION and renamings aren't all needed all the time, but
            # they don't hurt if unused.
            #
            # FIXME: Should generate these via config.status commands.
            # Would need them all in one AC_CONFIG_COMMANDS though, since
            # that macro doesn't accept a set of separate commands generated
            # by shell code.
            #
            case $tmp_ext in
              asm)
                # hide the d-n-l from autoconf's error checking
                tmp_d_n_l=d""nl
                echo ["$tmp_d_n_l  mpn_$tmp_fbase - from $tmp_dir directory for fat binary.
$tmp_d_n_l  Generated by configure - DO NOT EDIT.

define(OPERATION_$tmp_fn)
define(__gmpn_$tmp_fbase, __gmpn_${tmp_fbase}_$tmp_suffix)
define(__gmpn_$tmp_fbasec,__gmpn_${tmp_fbasec}_${tmp_suffix})
define(__gmpn_preinv_${tmp_fbase},__gmpn_preinv_${tmp_fbase}_${tmp_suffix})
define(__gmpn_${tmp_fbase}_cps,__gmpn_${tmp_fbase}_cps_${tmp_suffix})

$tmp_d_n_l  For k6 and k7 gcd_1 calling their corresponding mpn_modexact_1_odd
ifdef(\`__gmpn_modexact_1_odd',,
\`define(__gmpn_modexact_1_odd,__gmpn_modexact_1_odd_${tmp_suffix})')

$THRESH_ASM_SETUP
include][($mpn_relative_top_srcdir/mpn/$tmp_dir/$tmp_base.asm)
"] >mpn/${tmp_prefix}_$tmp_fn.asm
                ;;
              c)
                echo ["/* mpn_$tmp_fbase - from $tmp_dir directory for fat binary.
   Generated by configure - DO NOT EDIT. */

#define OPERATION_$tmp_fn 1
#define __gmpn_$tmp_fbase           __gmpn_${tmp_fbase}_$tmp_suffix
#define __gmpn_$tmp_fbasec          __gmpn_${tmp_fbasec}_${tmp_suffix}
#define __gmpn_preinv_${tmp_fbase}  __gmpn_preinv_${tmp_fbase}_${tmp_suffix}
#define __gmpn_${tmp_fbase}_cps     __gmpn_${tmp_fbase}_cps_${tmp_suffix}

#include \"$mpn_relative_top_srcdir/mpn/$tmp_dir/$tmp_base.c\"
"] >mpn/${tmp_prefix}_$tmp_fn.c
                ;;
            esac

            # Prototype, and append to CPUVEC_SETUP for this directory.
            echo "DECL_$tmp_fbase (__gmpn_${tmp_fbase}_$tmp_suffix);" >>fat.h
            CPUVEC_SETUP="$CPUVEC_SETUP    decided_cpuvec.$tmp_fbase = __gmpn_${tmp_fbase}_${tmp_suffix}; \\
"
            # Ditto for any preinv variant (preinv_divrem_1, preinv_mod_1).
            if grep "^PROLOGUE(mpn_preinv_$tmp_fn)" $tmp_file >/dev/null; then
              echo "DECL_preinv_$tmp_fbase (__gmpn_preinv_${tmp_fbase}_$tmp_suffix);" >>fat.h
              CPUVEC_SETUP="$CPUVEC_SETUP    decided_cpuvec.preinv_$tmp_fbase = __gmpn_preinv_${tmp_fbase}_${tmp_suffix}; \\
"
            fi

            # Ditto for any mod_1...cps variant
            if grep "^PROLOGUE(mpn_${tmp_fbase}_cps)" $tmp_file >/dev/null; then
              echo "DECL_${tmp_fbase}_cps (__gmpn_${tmp_fbase}_cps_$tmp_suffix);" >>fat.h
              CPUVEC_SETUP="$CPUVEC_SETUP    decided_cpuvec.${tmp_fbase}_cps = __gmpn_${tmp_fbase}_cps_${tmp_suffix}; \\
"
            fi
          fi
        done
      done
    done

    # Emit CPUVEC_SETUP for this directory
    echo "" >>fat.h
    echo "#define CPUVEC_SETUP_$tmp_suffix \\" >>fat.h
    echo "  do { \\" >>fat.h
    echo "$CPUVEC_SETUP  } while (0)" >>fat.h
  done

  # Emit threshold limits
  echo "" >>fat.h
  for tmp_tn in $fat_thresholds; do
    eval tmp_limit=\$${tmp_tn}_LIMIT
    echo "#define ${tmp_tn}_LIMIT  $tmp_limit" >>fat.h
  done
fi


# Normal binary setups.
#

for tmp_ext in MPN_SUFFIXES; do
  eval found_$tmp_ext=no
done

for tmp_fn in $gmp_mpn_functions; do
  for tmp_ext in MPN_SUFFIXES; do
    test "$no_create" = yes || rm -f mpn/$tmp_fn.$tmp_ext
  done

  # mpn_preinv_divrem_1 might have been provided by divrem_1.asm, likewise
  # mpn_preinv_mod_1 by mod_1.asm.
  case $tmp_fn in
  pre_divrem_1)
    if test "$HAVE_NATIVE_mpn_preinv_divrem_1" = yes; then continue; fi ;;
  pre_mod_1)
    if test "$HAVE_NATIVE_mpn_preinv_mod_1" = yes; then continue; fi ;;
  esac

  GMP_MULFUNC_CHOICES

  found=no
  for tmp_dir in $path; do
    for tmp_base in $tmp_fn $tmp_mulfunc; do
      for tmp_ext in MPN_SUFFIXES; do
        tmp_file=$srcdir/mpn/$tmp_dir/$tmp_base.$tmp_ext
        if test -f $tmp_file; then

          # For a nails build, check if the file supports our nail bits.
          # Generic code always supports all nails.
          #
          # FIXME: When a multi-function file is selected to provide one of
          # the nails-neutral routines, like logops_n for and_n, the
          # PROLOGUE grepping will create HAVE_NATIVE_mpn_<foo> defines for
          # all functions in that file, even if they haven't all been
          # nailified.  Not sure what to do about this, it's only really a
          # problem for logops_n, and it's not too terrible to insist those
          # get nailified always.
          #
          if test $GMP_NAIL_BITS != 0 && test $tmp_dir != generic; then
            case $tmp_fn in
              and_n | ior_n | xor_n | andn_n | \
              copyi | copyd | \
              popcount | hamdist | \
              udiv | udiv_w_sdiv | umul | \
              cntlz | invert_limb)
                # these operations are either unaffected by nails or defined
                # to operate on full limbs
                ;;
              *)
                nails=[`sed -n 's/^[ 	]*NAILS_SUPPORT(\(.*\))/\1/p' $tmp_file `]
                for n in $nails; do
                  case $n in
                  *-*)
                    n_start=`echo "$n" | sed -n 's/\(.*\)-.*/\1/p'`
                    n_end=`echo "$n" | sed -n 's/.*-\(.*\)/\1/p'`
                    ;;
                  *)
                    n_start=$n
                    n_end=$n
                    ;;
                  esac
                  if test $GMP_NAIL_BITS -ge $n_start && test $GMP_NAIL_BITS -le $n_end; then
                    found=yes
                    break
                  fi
                done
                if test $found != yes; then
                  continue
                fi
                ;;
            esac
          fi

	  # If the host uses a non-standard ABI, check if tmp_file supports it
	  #
	  if test -n "$GMP_NONSTD_ABI" && test $tmp_ext != "c"; then
	    abi=[`sed -n 's/^[ 	]*ABI_SUPPORT(\(.*\))/\1/p' $tmp_file `]
	    if echo "$abi" | grep -q "\\b${GMP_NONSTD_ABI}\\b"; then
	      true
	    else
	      continue
	    fi
	  fi

          found=yes
          eval found_$tmp_ext=yes

          if test $tmp_ext = c; then
            tmp_u='$U'
          else
            tmp_u=
          fi

          mpn_objects="$mpn_objects $tmp_fn$tmp_u.lo"
          mpn_objs_in_libgmp="$mpn_objs_in_libgmp mpn/$tmp_fn$tmp_u.lo"
          AC_CONFIG_LINKS(mpn/$tmp_fn.$tmp_ext:mpn/$tmp_dir/$tmp_base.$tmp_ext)
          gmp_srclinks="$gmp_srclinks mpn/$tmp_fn.$tmp_ext"

          # Duplicate AC_DEFINEs are harmless, so it doesn't matter
          # that multi-function files get grepped here repeatedly.
          # The PROLOGUE pattern excludes the optional second parameter.
          gmp_ep=[`
            sed -n 's/^[ 	]*MULFUNC_PROLOGUE(\(.*\))/\1/p' $tmp_file ;
            sed -n 's/^[ 	]*PROLOGUE(\([^,]*\).*)/\1/p' $tmp_file
          `]
          for gmp_tmp in $gmp_ep; do
            AC_DEFINE_UNQUOTED(HAVE_NATIVE_$gmp_tmp)
            eval HAVE_NATIVE_$gmp_tmp=yes
          done

          case $tmp_fn in
          sqr_basecase) sqr_basecase_source=$tmp_file ;;
          esac

          break
        fi
      done
      if test $found = yes; then break ; fi
    done
    if test $found = yes; then break ; fi
  done

  if test $found = no; then
    for tmp_optional in $gmp_mpn_functions_optional; do
      if test $tmp_optional = $tmp_fn; then
        found=yes
      fi
    done
    if test $found = no; then
      AC_MSG_ERROR([no version of $tmp_fn found in path: $path])
    fi
  fi
done

# All cycle counters are .asm files currently
if test -n "$SPEED_CYCLECOUNTER_OBJ"; then
  found_asm=yes
fi

dnl  The following list only needs to have templates for those defines which
dnl  are going to be tested by the code, there's no need to have every
dnl  possible mpn routine.

AH_VERBATIM([HAVE_NATIVE],
[/* Define to 1 each of the following for which a native (ie. CPU specific)
    implementation of the corresponding routine exists.  */
#undef HAVE_NATIVE_mpn_add_n
#undef HAVE_NATIVE_mpn_add_n_sub_n
#undef HAVE_NATIVE_mpn_add_nc
#undef HAVE_NATIVE_mpn_addaddmul_1msb0
#undef HAVE_NATIVE_mpn_addlsh1_n
#undef HAVE_NATIVE_mpn_addlsh2_n
#undef HAVE_NATIVE_mpn_addlsh_n
#undef HAVE_NATIVE_mpn_addlsh1_nc
#undef HAVE_NATIVE_mpn_addlsh2_nc
#undef HAVE_NATIVE_mpn_addlsh_nc
#undef HAVE_NATIVE_mpn_addlsh1_n_ip1
#undef HAVE_NATIVE_mpn_addlsh2_n_ip1
#undef HAVE_NATIVE_mpn_addlsh_n_ip1
#undef HAVE_NATIVE_mpn_addlsh1_nc_ip1
#undef HAVE_NATIVE_mpn_addlsh2_nc_ip1
#undef HAVE_NATIVE_mpn_addlsh_nc_ip1
#undef HAVE_NATIVE_mpn_addlsh1_n_ip2
#undef HAVE_NATIVE_mpn_addlsh2_n_ip2
#undef HAVE_NATIVE_mpn_addlsh_n_ip2
#undef HAVE_NATIVE_mpn_addlsh1_nc_ip2
#undef HAVE_NATIVE_mpn_addlsh2_nc_ip2
#undef HAVE_NATIVE_mpn_addlsh_nc_ip2
#undef HAVE_NATIVE_mpn_addmul_1c
#undef HAVE_NATIVE_mpn_addmul_2
#undef HAVE_NATIVE_mpn_addmul_3
#undef HAVE_NATIVE_mpn_addmul_4
#undef HAVE_NATIVE_mpn_addmul_5
#undef HAVE_NATIVE_mpn_addmul_6
#undef HAVE_NATIVE_mpn_addmul_7
#undef HAVE_NATIVE_mpn_addmul_8
#undef HAVE_NATIVE_mpn_addmul_2s
#undef HAVE_NATIVE_mpn_and_n
#undef HAVE_NATIVE_mpn_andn_n
#undef HAVE_NATIVE_mpn_bdiv_dbm1c
#undef HAVE_NATIVE_mpn_bdiv_q_1
#undef HAVE_NATIVE_mpn_pi1_bdiv_q_1
#undef HAVE_NATIVE_mpn_cnd_add_n
#undef HAVE_NATIVE_mpn_cnd_sub_n
#undef HAVE_NATIVE_mpn_com
#undef HAVE_NATIVE_mpn_copyd
#undef HAVE_NATIVE_mpn_copyi
#undef HAVE_NATIVE_mpn_div_qr_1n_pi1
#undef HAVE_NATIVE_mpn_div_qr_2
#undef HAVE_NATIVE_mpn_divexact_1
#undef HAVE_NATIVE_mpn_divexact_by3c
#undef HAVE_NATIVE_mpn_divrem_1
#undef HAVE_NATIVE_mpn_divrem_1c
#undef HAVE_NATIVE_mpn_divrem_2
#undef HAVE_NATIVE_mpn_gcd_1
#undef HAVE_NATIVE_mpn_hamdist
#undef HAVE_NATIVE_mpn_invert_limb
#undef HAVE_NATIVE_mpn_ior_n
#undef HAVE_NATIVE_mpn_iorn_n
#undef HAVE_NATIVE_mpn_lshift
#undef HAVE_NATIVE_mpn_lshiftc
#undef HAVE_NATIVE_mpn_lshsub_n
#undef HAVE_NATIVE_mpn_mod_1
#undef HAVE_NATIVE_mpn_mod_1_1p
#undef HAVE_NATIVE_mpn_mod_1c
#undef HAVE_NATIVE_mpn_mod_1s_2p
#undef HAVE_NATIVE_mpn_mod_1s_4p
#undef HAVE_NATIVE_mpn_mod_34lsub1
#undef HAVE_NATIVE_mpn_modexact_1_odd
#undef HAVE_NATIVE_mpn_modexact_1c_odd
#undef HAVE_NATIVE_mpn_mul_1
#undef HAVE_NATIVE_mpn_mul_1c
#undef HAVE_NATIVE_mpn_mul_2
#undef HAVE_NATIVE_mpn_mul_3
#undef HAVE_NATIVE_mpn_mul_4
#undef HAVE_NATIVE_mpn_mul_5
#undef HAVE_NATIVE_mpn_mul_6
#undef HAVE_NATIVE_mpn_mul_basecase
#undef HAVE_NATIVE_mpn_mullo_basecase
#undef HAVE_NATIVE_mpn_nand_n
#undef HAVE_NATIVE_mpn_nior_n
#undef HAVE_NATIVE_mpn_popcount
#undef HAVE_NATIVE_mpn_preinv_divrem_1
#undef HAVE_NATIVE_mpn_preinv_mod_1
#undef HAVE_NATIVE_mpn_redc_1
#undef HAVE_NATIVE_mpn_redc_2
#undef HAVE_NATIVE_mpn_rsblsh1_n
#undef HAVE_NATIVE_mpn_rsblsh2_n
#undef HAVE_NATIVE_mpn_rsblsh_n
#undef HAVE_NATIVE_mpn_rsblsh1_nc
#undef HAVE_NATIVE_mpn_rsblsh2_nc
#undef HAVE_NATIVE_mpn_rsblsh_nc
#undef HAVE_NATIVE_mpn_rsh1add_n
#undef HAVE_NATIVE_mpn_rsh1add_nc
#undef HAVE_NATIVE_mpn_rsh1sub_n
#undef HAVE_NATIVE_mpn_rsh1sub_nc
#undef HAVE_NATIVE_mpn_rshift
#undef HAVE_NATIVE_mpn_sqr_basecase
#undef HAVE_NATIVE_mpn_sqr_diagonal
#undef HAVE_NATIVE_mpn_sqr_diag_addlsh1
#undef HAVE_NATIVE_mpn_sub_n
#undef HAVE_NATIVE_mpn_sub_nc
#undef HAVE_NATIVE_mpn_sublsh1_n
#undef HAVE_NATIVE_mpn_sublsh2_n
#undef HAVE_NATIVE_mpn_sublsh_n
#undef HAVE_NATIVE_mpn_sublsh1_nc
#undef HAVE_NATIVE_mpn_sublsh2_nc
#undef HAVE_NATIVE_mpn_sublsh_nc
#undef HAVE_NATIVE_mpn_sublsh1_n_ip1
#undef HAVE_NATIVE_mpn_sublsh2_n_ip1
#undef HAVE_NATIVE_mpn_sublsh_n_ip1
#undef HAVE_NATIVE_mpn_sublsh1_nc_ip1
#undef HAVE_NATIVE_mpn_sublsh2_nc_ip1
#undef HAVE_NATIVE_mpn_sublsh_nc_ip1
#undef HAVE_NATIVE_mpn_submul_1c
#undef HAVE_NATIVE_mpn_tabselect
#undef HAVE_NATIVE_mpn_udiv_qrnnd
#undef HAVE_NATIVE_mpn_udiv_qrnnd_r
#undef HAVE_NATIVE_mpn_umul_ppmm
#undef HAVE_NATIVE_mpn_umul_ppmm_r
#undef HAVE_NATIVE_mpn_xor_n
#undef HAVE_NATIVE_mpn_xnor_n])


# Don't demand an m4 unless it's actually needed.
if test $found_asm = yes; then
  GMP_PROG_M4
  GMP_M4_M4WRAP_SPURIOUS
# else
# It's unclear why this m4-not-needed stuff was ever done.
#  if test -z "$M4" ; then
#    M4=m4-not-needed
#  fi
fi

# Only do the GMP_ASM checks if there's a .S or .asm wanting them.
if test $found_asm = no && test $found_S = no; then
  gmp_asm_syntax_testing=no
fi

if test "$gmp_asm_syntax_testing" != no; then
  GMP_ASM_TEXT
  GMP_ASM_DATA
  GMP_ASM_LABEL_SUFFIX
  GMP_ASM_GLOBL
  GMP_ASM_GLOBL_ATTR
  GMP_ASM_UNDERSCORE
  GMP_ASM_RODATA
  GMP_ASM_TYPE
  GMP_ASM_SIZE
  GMP_ASM_LSYM_PREFIX
  GMP_ASM_W32
  GMP_ASM_ALIGN_LOG

  case $host in
    arm*-*-* | aarch64*-*-*)
      case $ABI in
        32)
	  GMP_INCLUDE_MPN(arm/arm-defs.m4) ;;
      esac
      ;;
    hppa*-*-*)
      # for both pa32 and pa64
      GMP_INCLUDE_MPN(pa32/pa-defs.m4)
      ;;
    IA64_PATTERN)
      GMP_ASM_IA64_ALIGN_OK
      ;;
    M68K_PATTERN)
      GMP_ASM_M68K_INSTRUCTION
      GMP_ASM_M68K_ADDRESSING
      GMP_ASM_M68K_BRANCHES
      ;;
    [powerpc*-*-* | power[3-9]-*-*])
      GMP_ASM_POWERPC_PIC_ALWAYS
      GMP_ASM_POWERPC_R_REGISTERS
      GMP_INCLUDE_MPN(powerpc32/powerpc-defs.m4)

      # Check for Linux ELFv2 ABI
      AC_EGREP_CPP(yes,
[#if _CALL_ELF == 2
yes
#endif],
      [GMP_DEFINE_RAW(["define(<ELFv2_ABI>)"])])

      case $host in
        *-*-aix*)
	  case $ABI in
	    mode64)      GMP_INCLUDE_MPN(powerpc64/aix.m4) ;;
            *)           GMP_INCLUDE_MPN(powerpc32/aix.m4) ;;
          esac
          ;;
        *-*-linux* | *-*-*bsd*)
	  case $ABI in
	    mode64)      GMP_INCLUDE_MPN(powerpc64/elf.m4) ;;
	    mode32 | 32) GMP_INCLUDE_MPN(powerpc32/elf.m4) ;;
          esac
          ;;
        *-*-darwin*)
	  case $ABI in
	    mode64)      GMP_INCLUDE_MPN(powerpc64/darwin.m4) ;;
	    mode32 | 32) GMP_INCLUDE_MPN(powerpc32/darwin.m4) ;;
          esac
          ;;
        *)
	  # Assume unrecognized operating system is the powerpc eABI
          GMP_INCLUDE_MPN(powerpc32/eabi.m4)
	  ;;
      esac
      ;;
    power*-*-aix*)
      GMP_INCLUDE_MPN(powerpc32/aix.m4)
      ;;
    *sparc*-*-*)
      case $ABI in
        64)
          GMP_ASM_SPARC_REGISTER
          ;;
      esac
      GMP_ASM_SPARC_GOTDATA
      GMP_ASM_SPARC_SHARED_THUNKS
      ;;
    X86_PATTERN | X86_64_PATTERN)
      GMP_ASM_ALIGN_FILL_0x90
      case $ABI in
        32)
          GMP_INCLUDE_MPN(x86/x86-defs.m4)
          AC_DEFINE(HAVE_HOST_CPU_FAMILY_x86)
          GMP_ASM_COFF_TYPE
          GMP_ASM_X86_GOT_UNDERSCORE
          GMP_ASM_X86_SHLDL_CL
	  case $enable_profiling in
	    prof | gprof)  GMP_ASM_X86_MCOUNT ;;
	  esac
	  case $host in
	    *-*-darwin*)
	      GMP_INCLUDE_MPN(x86/darwin.m4) ;;
	  esac
          ;;
        64|x32)
          GMP_INCLUDE_MPN(x86_64/x86_64-defs.m4)
          AC_DEFINE(HAVE_HOST_CPU_FAMILY_x86_64)
	  case $host in
	    *-*-darwin*)
	      GMP_INCLUDE_MPN(x86_64/darwin.m4) ;;
	    *-*-mingw* | *-*-cygwin)
	      GMP_INCLUDE_MPN(x86_64/dos64.m4) ;;
	    *-openbsd*)
	      GMP_DEFINE_RAW(["define(<OPENBSD>,1)"]) ;;
	    *-linux*)
	      GMP_DEFINE_RAW(["define(<LINUX>,1)"]) ;;
	  esac
          ;;
      esac
      ;;
  esac
fi

# For --enable-minithres, prepend "minithres" to path so that its special
# gmp-mparam.h will be used.
if test $enable_minithres = yes; then
  path="minithres $path"
fi

# Create link for gmp-mparam.h.
gmp_mparam_source=
for gmp_mparam_dir in $path; do
  test "$no_create" = yes || rm -f gmp-mparam.h
  tmp_file=$srcdir/mpn/$gmp_mparam_dir/gmp-mparam.h
  if test -f $tmp_file; then
    AC_CONFIG_LINKS(gmp-mparam.h:mpn/$gmp_mparam_dir/gmp-mparam.h)
    gmp_srclinks="$gmp_srclinks gmp-mparam.h"
    gmp_mparam_source=$tmp_file
    break
  fi
done
if test -z "$gmp_mparam_source"; then
  AC_MSG_ERROR([no version of gmp-mparam.h found in path: $path])
fi

# Copy relevant parameters from gmp-mparam.h to config.m4.
# We only do this for parameters that are used by some assembly files.
# Fat binaries do this on a per-file basis, so skip in that case.
#
if test -z "$fat_path"; then
  for i in SQR_TOOM2_THRESHOLD BMOD_1_TO_MOD_1_THRESHOLD SHLD_SLOW SHRD_SLOW; do
    value=`sed -n 's/^#define '$i'[ 	]*\([0-9][0-9]*\).*$/\1/p' $gmp_mparam_source`
    if test -n "$value"; then
      GMP_DEFINE_RAW(["define(<$i>,<$value>)"])
    fi
  done
fi


# Sizes of some types, needed at preprocessing time.
#
# FIXME: The assumption that GMP_LIMB_BITS is 8*sizeof(mp_limb_t) might
# be slightly rash, but it's true everywhere we know of and ought to be true
# of any sensible system.  In a generic C build, grepping LONG_BIT out of
# <limits.h> might be an alternative, for maximum portability.
#
AC_CHECK_SIZEOF(void *)
AC_CHECK_SIZEOF(unsigned short)
AC_CHECK_SIZEOF(unsigned)
AC_CHECK_SIZEOF(unsigned long)
AC_CHECK_SIZEOF(mp_limb_t, , GMP_INCLUDE_GMP_H)
if test "$ac_cv_sizeof_mp_limb_t" = 0; then
  AC_MSG_ERROR([Oops, mp_limb_t doesn't seem to work])
fi
AC_SUBST(GMP_LIMB_BITS, `expr 8 \* $ac_cv_sizeof_mp_limb_t`)
GMP_DEFINE_RAW(["define(<SIZEOF_UNSIGNED>,<$ac_cv_sizeof_unsigned>)"])

# Check compiler limb size matches gmp-mparam.h
#
# FIXME: Some of the cycle counter objects in the tune directory depend on
# the size of ulong, it'd be possible to check that here, though a mismatch
# probably wouldn't want to be fatal, none of the libgmp assembler code
# depends on ulong.
#
mparam_bits=[`sed -n 's/^#define GMP_LIMB_BITS[ 	][ 	]*\([0-9]*\).*$/\1/p' $gmp_mparam_source`]
if test -n "$mparam_bits" && test "$mparam_bits" -ne $GMP_LIMB_BITS; then
  if test "$test_CFLAGS" = set; then
    AC_MSG_ERROR([Oops, mp_limb_t is $GMP_LIMB_BITS bits, but the assembler code
in this configuration expects $mparam_bits bits.
You appear to have set \$CFLAGS, perhaps you also need to tell GMP the
intended ABI, see "ABI and ISA" in the manual.])
  else
    AC_MSG_ERROR([Oops, mp_limb_t is $GMP_LIMB_BITS bits, but the assembler code
in this configuration expects $mparam_bits bits.])
  fi
fi

GMP_DEFINE_RAW(["define(<GMP_LIMB_BITS>,$GMP_LIMB_BITS)"])
GMP_DEFINE_RAW(["define(<GMP_NAIL_BITS>,$GMP_NAIL_BITS)"])
GMP_DEFINE_RAW(["define(<GMP_NUMB_BITS>,eval(GMP_LIMB_BITS-GMP_NAIL_BITS))"])


AC_SUBST(mpn_objects)
AC_SUBST(mpn_objs_in_libgmp)
AC_SUBST(gmp_srclinks)


# Configs for demos/pexpr.c.
#

GMP_SUBST_CHECK_FUNCS(clock, cputime, getrusage, gettimeofday, sigaction, sigaltstack, sigstack)
GMP_SUBST_CHECK_HEADERS(sys/resource.h)
AC_CHECK_TYPES([stack_t], HAVE_STACK_T_01=1, HAVE_STACK_T_01=0,
               [#include <signal.h>])
AC_SUBST(HAVE_STACK_T_01)

# Configs for demos/calc directory
#
# AC_SUBST+AC_CONFIG_FILES is used for calc-config.h, rather than AC_DEFINE+
# AC_CONFIG_HEADERS, since with the latter automake (1.8) will then put the
# directory (ie. demos/calc) into $(DEFAULT_INCLUDES) for every Makefile.in,
# which would look very strange.
#
# -lcurses is required by libreadline.  On a typical SVR4 style system this
# normally doesn't have to be given explicitly, since libreadline.so will
# have a NEEDED record for it.  But if someone for some reason is using only
# a static libreadline.a then we must give -lcurses.  Readline (as of
# version 4.3) doesn't use libtool, so we can't rely on a .la to cover
# necessary dependencies.
#
# On a couple of systems we've seen libreadline available, but the headers
# not in the default include path, so check for readline/readline.h.  We've
# also seen readline/history.h missing, not sure if that's just a broken
# install or a very old version, but check that too.
#

LIBCURSES=
if test $with_readline != no; then
  AC_CHECK_LIB(ncurses, tputs, [LIBCURSES=-lncurses],
    [AC_CHECK_LIB(curses, tputs, [LIBCURSES=-lcurses])])
fi
AC_SUBST(LIBCURSES)
use_readline=$with_readline
if test $with_readline = detect; then
  use_readline=no
  AC_CHECK_LIB(readline, readline,
    [AC_CHECK_HEADER(readline/readline.h,
      [AC_CHECK_HEADER(readline/history.h, use_readline=yes)])],
    , $LIBCURSES)
  AC_MSG_CHECKING(readline detected)
  AC_MSG_RESULT($use_readline)
fi
if test $use_readline = yes; then
  AC_SUBST(WITH_READLINE_01, 1)
  AC_SUBST(LIBREADLINE, -lreadline)
else
  WITH_READLINE_01=0
fi
AC_PROG_YACC
AM_PROG_LEX

# Configs for demos/expr directory
#
# Libtool already runs an AC_CHECK_TOOL for ranlib, but we give
# AC_PROG_RANLIB anyway since automake is supposed to complain if it's not
# called.  (Automake 1.8.4 doesn't, at least not when the only library is in
# an EXTRA_LIBRARIES.)
#
AC_PROG_RANLIB


# Create config.m4.
GMP_FINISH

# Create Makefiles
# FIXME: Upcoming version of autoconf/automake may not like broken lines.
#        Right now automake isn't accepting the new AC_CONFIG_FILES scheme.

AC_OUTPUT(Makefile							\
  mpn/Makefile mpq/Makefile				\
  mpz/Makefile gmp.h:gmp-h.in)

AC_MSG_NOTICE([summary of build options:

  Version:           ${PACKAGE_STRING}
  Host type:         ${host}
  ABI:               ${ABI}
  Install prefix:    ${prefix}
  Compiler:          ${CC}
  Static libraries:  ${enable_static}
  Shared libraries:  ${enable_shared}
])

if test x$cross_compiling = xyes ; then
   case "$host" in
     *-*-mingw* | *-*-cygwin)
     if test x$ABI = x64 ; then
	AC_MSG_NOTICE([If wine64 is installed, use make check TESTS_ENVIRONMENT=wine64.])
     else
	AC_MSG_NOTICE([If wine is installed, use make check TESTS_ENVIRONMENT=wine.])
     fi
     ;;
   esac
fi

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


CFLAGS_or_unset=${CFLAGS-'(unset)'}
CPPFLAGS_or_unset=${CPPFLAGS-'(unset)'}

cat >&AC_FD_CC <<EOF
User:
ABI=$ABI
CC=$CC
CFLAGS=$CFLAGS_or_unset
CPPFLAGS=$CPPFLAGS_or_unset
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
path=$path
EOF
echo "using ABI=\"$ABI\""
echo "      CC=\"$CC\""
echo "      CFLAGS=\"$CFLAGS\""
echo "      CPPFLAGS=\"$CPPFLAGS\""



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

GMP_FUNC_VSNPRINTF
GMP_FUNC_SSCANF_WRITABLE_INPUT

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
GMP_DEFINE_RAW(["define(<SIZEOF_UNSIGNED>,<$ac_cv_sizeof_unsigned>)"])

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
  mpz/Makefile \
  gmp.h:gmp-h.in)

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

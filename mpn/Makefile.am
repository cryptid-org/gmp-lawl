## Process this file with automake to generate Makefile.in

# Copyright 1996, 1998-2002, 2005, 2011, 2013 Free Software Foundation, Inc.
#
#  This file is part of the GNU MP Library.
#
#  The GNU MP Library is free software; you can redistribute it and/or modify
#  it under the terms of either:
#
#    * the GNU Lesser General Public License as published by the Free
#      Software Foundation; either version 3 of the License, or (at your
#      option) any later version.
#
#  or
#
#    * the GNU General Public License as published by the Free Software
#      Foundation; either version 2 of the License, or (at your option) any
#      later version.
#
#  or both in parallel, as here.
#
#  The GNU MP Library is distributed in the hope that it will be useful, but
#  WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
#  or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
#  for more details.
#
#  You should have received copies of the GNU General Public License and the
#  GNU Lesser General Public License along with the GNU MP Library.  If not,
#  see https://www.gnu.org/licenses/.


AM_CPPFLAGS = -D__GMP_WITHIN_GMP -I$(top_srcdir) \
  -DOPERATION_`echo $* | sed 's/_$$//'`

OFILES = @mpn_objects@

noinst_LTLIBRARIES = libmpn.la
nodist_libmpn_la_SOURCES = fib_table.c mp_bases.c
libmpn_la_LIBADD = $(OFILES)
libmpn_la_DEPENDENCIES = $(OFILES)

TARG_DIST = generic

EXTRA_DIST = asm-defs.m4 cpp-ccas m4-ccas $(TARG_DIST)


# These are BUILT_SOURCES at the top-level, so normally they're built before
# recursing into this directory.
#
fib_table.c:
	cd ..; $(MAKE) $(AM_MAKEFLAGS) mpn/fib_table.c
mp_bases.c:
	cd ..; $(MAKE) $(AM_MAKEFLAGS) mpn/mp_bases.c
perfsqr.h:
	cd ..; $(MAKE) $(AM_MAKEFLAGS) mpn/perfsqr.h

include Makeasm.am

## Process this file with automake to generate Makefile.in

# Copyright 1996, 1998-2003, 2012 Free Software Foundation, Inc.
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


AM_CPPFLAGS = -D__GMP_WITHIN_GMP -I$(top_srcdir)

noinst_LTLIBRARIES = libmpz.la
libmpz_la_SOURCES = aors.h aors_ui.h fits_s.h mul_i.h \
  add.c add_ui.c cdiv_q.c cdiv_q_ui.c cfdiv_q_2exp.c \
  clear.c clears.c cmp.c cmp_si.c cmp_ui.c \
  divexact.c dive_ui.c export.c fdiv_q_ui.c \
  gcdext.c get_str.c get_ui.c getlimbn.c \
  import.c init.c inits.c \
  invert.c iset.c iset_ui.c \
  mod.c mul.c mul_ui.c n_pow_ui.c neg.c \
  pow_ui.c powm.c powm_ui.c \
  realloc.c realloc2.c \
  scan1.c set.c set_si.c set_str.c \
  set_ui.c sizeinbase.c sub.c sub_ui.c \
  tdiv_q_2exp.c tdiv_qr.c \
  tdiv_r.c tstbit.c ui_pow_ui.c
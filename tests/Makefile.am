# Copyright (C) 2011-2017 Simon Josefsson

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

AM_CPPFLAGS = -I$(top_srcdir) -I$(top_srcdir)/lib -I$(top_builddir)/lib -I$(srcdir) -I$(top_builddir)
AM_CPPFLAGS += -I$(top_builddir)/gl -I$(top_srcdir)/gl
AM_CPPFLAGS += -DSRCDIR=\"$(srcdir)\"
AM_CFLAGS = $(WARN_CFLAGS)
AM_LDFLAGS = -no-install

if !HAVE_LIBUNISTRING
AM_CPPFLAGS += -I$(top_srcdir)/unistring/ -I$(top_builddir)/unistring/
endif

LDADD = ../lib/libidn2.la ../gl/libgnu.la $(LTLIBUNISTRING)

if !HAVE_LIBUNISTRING
LDADD += ../unistring/libunistring.la
endif

TESTS = test-punycode test-lookup test-register test-strerror test-tounicode \
  test-glibc
check_PROGRAMS = $(TESTS)

test_punycode_LDADD = ../lib/puny_decode.o ../lib/puny_encode.o $(LDADD) $(CODE_COVERAGE_LIBS)

IDNA_TXT = IdnaTest.txt
IDNA_INC = IdnaTest.inc
test_lookup_SOURCES = test-lookup.c

BUILT_SOURCES = $(IDNA_TXT) $(IDNA_INC)
MAINTAINERCLEANFILES = $(BUILT_SOURCES)
EXTRA_DIST = gen-utc-test.pl $(IDNA_TXT) $(IDNA_INC) suppressions.valgrind

$(IDNA_TXT):
	rm -f $(IDNA_TXT)
	wget http://www.unicode.org/Public/idna/6.3.0/IdnaTest.txt
	echo "d3690c2bb64ea003ac481bbd50c3330eaf1cf691 IdnaTest.txt" | sha1sum -c -

$(IDNA_INC): $(IDNA_TXT) $(srcdir)/gen-utc-test.pl
	$(srcdir)/gen-utc-test.pl < $< > $@

LOG_COMPILER = $(VALGRIND)

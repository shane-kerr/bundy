AM_CPPFLAGS = -I$(top_srcdir)/src/lib -I$(top_builddir)/src/lib
AM_CPPFLAGS += -I$(top_builddir)/src/lib/dns -I$(top_srcdir)/src/bin
AM_CPPFLAGS += -I$(top_builddir)/src/lib/cc
AM_CPPFLAGS += -DTEST_DATA_DIR=\"$(top_srcdir)/src/lib/testutils/testdata\"
AM_CPPFLAGS += -DTEST_DATA_BUILDDIR=\"$(abs_top_builddir)/src/lib/testutils/testdata\"
AM_CPPFLAGS += $(BOOST_INCLUDES)

AM_CXXFLAGS = $(B10_CXXFLAGS)

if USE_STATIC_LINK
AM_LDFLAGS = -static
endif

CLEANFILES = *.gcno *.gcda

TESTS =
if HAVE_GTEST
TESTS += run_unittests
run_unittests_SOURCES = $(top_srcdir)/src/lib/dns/tests/unittest_util.h
run_unittests_SOURCES += $(top_srcdir)/src/lib/dns/tests/unittest_util.cc
run_unittests_SOURCES += ../resolver.h ../resolver.cc
run_unittests_SOURCES += ../response_scrubber.h ../response_scrubber.cc
run_unittests_SOURCES += resolver_unittest.cc
run_unittests_SOURCES += resolver_config_unittest.cc
run_unittests_SOURCES += response_scrubber_unittest.cc
run_unittests_SOURCES += run_unittests.cc
run_unittests_CPPFLAGS = $(AM_CPPFLAGS) $(GTEST_INCLUDES)
run_unittests_LDFLAGS = $(AM_LDFLAGS) $(GTEST_LDFLAGS)
run_unittests_LDADD = $(GTEST_LDADD)
run_unittests_LDADD += $(SQLITE_LIBS)
run_unittests_LDADD += $(top_builddir)/src/lib/testutils/libtestutils.la
run_unittests_LDADD += $(top_builddir)/src/lib/datasrc/libdatasrc.la
run_unittests_LDADD += $(top_builddir)/src/lib/dns/libdns++.la
run_unittests_LDADD += $(top_builddir)/src/lib/asiolink/libasiolink.la
run_unittests_LDADD += $(top_builddir)/src/lib/config/libcfgclient.la
run_unittests_LDADD += $(top_builddir)/src/lib/cc/libcc.la
run_unittests_LDADD += $(top_builddir)/src/lib/exceptions/libexceptions.la
run_unittests_LDADD += $(top_builddir)/src/lib/xfr/libxfr.la
run_unittests_LDADD += $(top_builddir)/src/lib/log/liblog.la

# Note the ordering matters: -Wno-... must follow -Wextra (defined in
# B10_CXXFLAGS
run_unittests_CXXFLAGS = $(AM_CXXFLAGS)
if USE_GXX
run_unittests_CXXFLAGS += -Wno-unused-parameter
endif
endif



noinst_PROGRAMS = $(TESTS)
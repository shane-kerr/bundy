SUBDIRS = testdata
PYCOVERAGE_RUN = @PYCOVERAGE_RUN@
PYTESTS = builder_tests.py datasrc_info_tests.py
EXTRA_DIST = $(PYTESTS)

# If necessary (rare cases), explicitly specify paths to dynamic libraries
# required by loadable python modules.
if SET_ENV_LIBRARY_PATH
LIBRARY_PATH_PLACEHOLDER = $(ENV_LIBRARY_PATH)=$(abs_top_builddir)/src/lib/cryptolink/.libs:$(abs_top_builddir)/src/lib/dns/.libs:$(abs_top_builddir)/src/lib/dns/python/.libs:$(abs_top_builddir)/src/lib/cc/.libs:$(abs_top_builddir)/src/lib/config/.libs:$(abs_top_builddir)/src/lib/log/.libs:$(abs_top_builddir)/src/lib/util/.libs:$(abs_top_builddir)/src/lib/util/threads/.libs:$(abs_top_builddir)/src/lib/acl/.libs:$(abs_top_builddir)/src/lib/exceptions/.libs:$(abs_top_builddir)/src/lib/datasrc/.libs:$$$(ENV_LIBRARY_PATH)
endif

# Some tests require backend shared memory support
if USE_SHARED_MEMORY
HAVE_SHARED_MEMORY=yes
else
HAVE_SHARED_MEMORY=no
endif

# test using command-line arguments, so use check-local target instead of TESTS
# BUNDY_FROM_BUILD is necessary to load data source backend from the build tree.
check-local:
if ENABLE_PYTHON_COVERAGE
	touch $(abs_top_srcdir)/.coverage
	rm -f .coverage
	${LN_S} $(abs_top_srcdir)/.coverage .coverage
endif
	for pytest in $(PYTESTS) ; do \
	echo Running test: $$pytest ; \
	$(LIBRARY_PATH_PLACEHOLDER) \
	TESTDATA_PATH=$(abs_srcdir)/testdata \
	TESTDATA_WRITE_PATH=$(builddir) \
	BUNDY_FROM_BUILD=$(abs_top_builddir) \
	HAVE_SHARED_MEMORY=$(HAVE_SHARED_MEMORY) \
	PYTHONPATH=$(COMMON_PYTHON_PATH):$(abs_top_builddir)/src/lib/dns/python/.libs \
	$(PYCOVERAGE_RUN) $(abs_srcdir)/$$pytest || exit ; \
	done

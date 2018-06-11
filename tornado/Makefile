PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

TORNADOVERSION=5.0.2

ROOT=$(abspath .)

SRC=$(ROOT)/tornado-$(TORNADOVERSION)
BUILD=$(SRC)/tornado
TARBALL=$(ROOT)/downloads/tornado-$(TORNADOVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/cf/d1/3be271ae5eba9fb59df63c9891fdc7d8044b999e8ac145994cdbfd2ae66a/tornado-$(TORNADOVERSION).tar.gz

all: $(BUILD)/__init__.py


clean: 
	-rm -fr downloads
	-rm -fr $(SRC)


$(TARBALL):
	[ -d $(ROOT)/downloads ] || mkdir $(ROOT)/downloads
	wget -q -O $@ $(URL)
	sha256sum --quiet --check checksum || (rm $@; false)

$(SRC)/setup.py: $(TARBALL)
	tar -C . -xf $(TARBALL)
	touch $(SRC)/setup.py

$(BUILD)/__init__.py: $(ROOT)/.patched
	( \
		cd $(SRC) ; \
		$(HOSTPYTHON) setup.py build ; \
		touch $(BUILD)/__init__.py \
	)

$(ROOT)/.patched: $(SRC)/setup.py
	cat patches/*.patch | (cd $(SRC) ; patch -p1)
	touch $@

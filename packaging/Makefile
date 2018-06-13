PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

PACKAGINGVERSION=17.1

ROOT=$(abspath .)

SRC=$(ROOT)/packaging-$(PACKAGINGVERSION)
BUILD=$(SRC)/build/lib/packaging
TARBALL=$(ROOT)/downloads/packaging-$(PACKAGINGVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/77/32/439f47be99809c12ef2da8b60a2c47987786d2c6c9205549dd6ef95df8bd/packaging-$(PACKAGINGVERSION).tar.gz

all: $(BUILD)/__init__.py


clean:
	-rm -fr downloads
	-rm -fr $(SRC)

$(TARBALL):
	[ -d $(ROOT)/downloads ] || mkdir $(ROOT)/downloads
	wget -q -O $@ $(URL)
	sha256sum --quiet --check checksums || (rm $@; false)

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
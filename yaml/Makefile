PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

PYYAMLVERSION=3.12

ROOT=$(abspath .)

SRC=$(ROOT)/PyYAML-$(PYYAMLVERSION)
BUILD=$(SRC)/build/lib.$(PLATFORMSLUG)/yaml
TARBALL=$(ROOT)/downloads/PyYAML-$(PYYAMLVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/4a/85/db5a2df477072b2902b0eb892feb37d88ac635d36245a72a6a69b23b383a/PyYAML-$(PYYAMLVERSION).tar.gz

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
		$(HOSTPYTHON) setup.py --without-libyaml build ; \
		touch build/lib.$(PLATFORMSLUG)/yaml/__init__.py \
	)


$(ROOT)/.patched: $(SRC)/setup.py
	cat patches/*.patch | (cd $(SRC) ; patch -p1)
	touch $@
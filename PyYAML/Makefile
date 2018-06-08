PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

PYYAMLVERSION=3.12

ROOT=$(abspath .)

SRC=$(ROOT)/PyYAML-$(PYYAMLVERSION)
BUILD=$(SRC)/build/lib/PyYAML
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

$(BUILD)/__init__.py: $(SRC)/setup.py
	( \
		cd $(SRC) ; \
		$(HOSTPYTHON) setup.py build ; \
		touch build/lib/PyYAML/__init__.py \
	)
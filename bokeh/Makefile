PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

BOKEHVERSION=0.12.16

ROOT=$(abspath .)

SRC=$(ROOT)/bokeh-$(BOKEHVERSION)
BUILD=$(SRC)/build/lib/bokeh
TARBALL=$(ROOT)/downloads/bokeh-$(BOKEHVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/cd/47/201408029628164342e65a4552ee00abc79ea7be1b64031281b81b0e2f4d/bokeh-0.12.16.tar.gz

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
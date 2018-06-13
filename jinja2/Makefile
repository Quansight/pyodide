PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

JINJA2VERSION=2.10

ROOT=$(abspath .)

SRC=$(ROOT)/Jinja2-$(JINJA2VERSION)
BUILD=$(SRC)/build/lib/jinja2
TARBALL=$(ROOT)/downloads/Jinja2-$(JINJA2VERSION).tar.gz
URL=https://files.pythonhosted.org/packages/56/e6/332789f295cf22308386cf5bbd1f4e00ed11484299c5d7383378cf48ba47/Jinja2-2.10.tar.gz

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

PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

PYYAMLVERSION=3.12

ROOT=$(abspath .)

HOSTROOT=$(ROOT)/host
HOSTDIR=$(HOSTROOT)/PyYAML-$(PYYAMLVERSION)
HOSTBUILD=$(HOSTDIR)/build
HOSTLIB=$(HOSTDIR)/lib/yaml

BUILD=$(ROOT)/build/yaml

TARBALL=$(ROOT)/downloads/PyYAML-$(PYYAMLVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/4a/85/db5a2df477072b2902b0eb892feb37d88ac635d36245a72a6a69b23b383a/PyYAML-$(PYYAMLVERSION).tar.gz

all: $(BUILD)/__init__.py 

clean: 
	-rm -fr downloads
	-rm -fr $(HOSTROOT)
	-rm -fr $(BUILD)


$(TARBALL):
	[ -d $(ROOT)/downloads ] || mkdir $(ROOT)/downloads
	wget -q -O $@ $(URL)
	sha256sum --quiet --check checksums || (rm $@; false)

$(HOSTDIR)/setup.py: $(TARBALL)
	[ -d $(HOSTROOT) ] || mkdir $(HOSTROOT)
	tar -xf $(TARBALL) -C $(HOSTROOT)
	touch $(HOSTDIR)/setup.py

$(HOSTBUILD)/lib.$(PLATFORMSLUG)/yaml/__init__.py: $(ROOT)/.patched
	( \
		cd $(HOSTDIR); \
	$(HOSTPYTHON) setup.py --without-libyaml build \
	)

$(BUILD)/__init__.py: $(HOSTBUILD)/lib.$(PLATFORMSLUG)/yaml/__init__.py
	[ -d $(ROOT)/build ] || mkdir $(ROOT)/build
	cp -r $(HOSTBUILD)/lib.$(PLATFORMSLUG)/yaml $(ROOT)/build

$(ROOT)/.patched: $(HOSTDIR)/setup.py
	cat patches/*.patch | (cd $(SRC) ; patch -p1)
	touch $@
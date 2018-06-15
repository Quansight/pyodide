PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

TORNADOVERSION=5.0.2

ROOT=$(abspath .)

HOSTROOT=$(ROOT)/host
HOSTDIR=$(HOSTROOT)/tornado-$(TORNADOVERSION)
HOSTBUILD=$(HOSTDIR)/build
HOSTLIB=$(HOSTDIR)/lib/tornado

BUILD=$(ROOT)/build/tornado

TARBALL=$(ROOT)/downloads/tornado-$(TORNADOVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/cf/d1/3be271ae5eba9fb59df63c9891fdc7d8044b999e8ac145994cdbfd2ae66a/tornado-$(TORNADOVERSION).tar.gz

all: $(BUILD)/__init__.py


clean: 
	-rm -fr downloads
	-rm -fr $(HOSTROOT)
	-rm -fr $(BUILD)
	-rm -fr $(HOSTDIR)


$(TARBALL):
	[ -d $(ROOT)/downloads ] || mkdir $(ROOT)/downloads
	wget -q -O $@ $(URL)
	sha256sum --quiet --check checksum || (rm $@; false)

$(HOSTDIR)/setup.py: $(TARBALL)
	[ -d $(HOSTROOT) ] || mkdir $(HOSTROOT)
	tar -xf $(TARBALL) -C $(HOSTROOT)
	touch $(HOSTDIR)/setup.py

$(HOSTBUILD)/lib.$(PLATFORMSLUG)/tornado/__init__.py: $(ROOT)/.patched
	( \
		cd $(HOSTDIR); \
	$(HOSTPYTHON) setup.py build \
	)

$(BUILD)/__init__.py: $(HOSTBUILD)/lib.$(PLATFORMSLUG)/tornado/__init__.py
	[ -d $(ROOT)/build ] || mkdir $(ROOT)/build
	cp -r $(HOSTBUILD)/lib.$(PLATFORMSLUG)/tornado $(ROOT)/build

$(ROOT)/.patched: $(HOSTDIR)/setup.py
	cat patches/*.patch | (cd $(HOSTDIR) ; patch -p1)
	touch $@


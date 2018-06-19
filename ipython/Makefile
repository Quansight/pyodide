PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

IPYTHONVERSION=6.4.0

ROOT=$(abspath .)

HOSTROOT=$(ROOT)/host
HOSTDIR=$(HOSTROOT)/ipython-$(IPYTHONVERSION)
HOSTBUILD=$(HOSTDIR)/build
HOSTLIB=$(HOSTDIR)/lib/ipython

BUILD=$(ROOT)/build/ipython

TARBALL=$(ROOT)/downloads/ipython-$(IPYTHONVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/ee/01/2a85cd07f5a43fa2e86d60001c213647252662d44a0c2e3d69471a058f1b/ipython-6.4.0.tar.gz

all: $(BUILD)/__init__.py

clean:
	-rm -fr downloads
	-rm -fr $(HOSTROOT)
	-rm -fr $(BUILD)
	-rm -fr $(HOSTDIR)


$(TARBALL):
	[ -d $(ROOT)/downloads ] || mkdir $(ROOT)/downloads
	wget -q -O $@ $(URL)
	sha256sum --quiet --check checksums || (rm $@; false)

$(HOSTDIR)/setup.py: $(TARBALL)
	[ -d $(HOSTROOT) ] || mkdir $(HOSTROOT)
	tar -xf $(TARBALL) -C $(HOSTROOT)
	touch $(HOSTDIR)/setup.py

$(HOSTBUILD)/lib/ipython/__init__.py: $(ROOT)/.patched
	( \
		cd $(HOSTDIR); \
	$(HOSTPYTHON) setup.py build \
	)

$(BUILD)/__init__.py: $(HOSTBUILD)/lib/ipython/__init__.py
	[ -d $(ROOT)/build ] || mkdir $(ROOT)/build
	cp -r $(HOSTBUILD)/lib/ipython $(ROOT)/build

$(ROOT)/.patched: $(HOSTDIR)/setup.py
	cat patches/*.patch | (cd $(HOSTDIR) ; patch -p1)
	touch $@

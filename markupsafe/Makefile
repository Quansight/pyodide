PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

MARKUPSAFEVERSION=1.0

ROOT=$(abspath .)

HOSTROOT=$(ROOT)/host
HOSTDIR=$(HOSTROOT)/MarkupSafe-$(MARKUPSAFEVERSION)
HOSTBUILD=$(HOSTDIR)/build
HOSTLIB=$(HOSTDIR)/lib/markupsafe

BUILD=$(ROOT)/build/markupsafe

TARBALL=$(ROOT)/downloads/MarkupSafe-$(MARKUPSAFEVERSION).tar.gz
URL=https://files.pythonhosted.org/packages/4d/de/32d741db316d8fdb7680822dd37001ef7a448255de9699ab4bfcbdf4172b/MarkupSafe-1.0.tar.gz

all: $(BUILD)/__init__.py


clean:
	-rm -fr downloads
	-rm -fr build
	-rm -fr MarkupSafe-$(MARKUPSAFEVERSION)
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


$(HOSTBUILD)/lib.$(PLATFORMSLUG)/markupsafe/__init__.py: $(ROOT)/.patched
	( \
		cd $(HOSTDIR); \
		$(HOSTPYTHON) setup.py build \
	)

$(BUILD)/__init__.py: $(HOSTBUILD)/lib.$(PLATFORMSLUG)/markupsafe/__init__.py
	[ -d $(ROOT)/build ] || mkdir $(ROOT)/build
	cp -r $(HOSTBUILD)/lib.$(PLATFORMSLUG)/markupsafe $(ROOT)/build

$(ROOT)/.patched: $(HOSTDIR)/setup.py
	cat patches/*.patch | (cd $(HOSTDIR) ; patch -p1)
	touch $@
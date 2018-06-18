PYODIDE_ROOT=$(abspath ..)
include ../Makefile.envs

SETUPTOOLSVERSION=39.2.0

ROOT=$(abspath .)

HOSTROOT=$(ROOT)/host
HOSTDIR=$(HOSTROOT)/setuptools-$(SETUPTOOLSVERSION)
HOSTBUILD=$(HOSTDIR)/build
HOSTLIB=$(HOSTDIR)/lib/setuptools

BUILD=$(ROOT)/build/lib
BUILDPKGRESOURCES=$(BUILD)/pkg_resources
BUILDSETUPTOOLS=$(BUILD)/setuptools

ZIPFILE=$(ROOT)/downloads/setuptools-$(SETUPTOOLSVERSION).zip
URL=https://files.pythonhosted.org/packages/1a/04/d6f1159feaccdfc508517dba1929eb93a2854de729fa68da9d5c6b48fa00/setuptools-39.2.0.zip

all: $(BUILD)/setuptools/__init__.py


clean: 
	-rm -fr downloads
	-rm -fr $(HOSTROOT)
	-rm -fr $(BUILD)
	-rm -fr $(HOSTDIR)


$(ZIPFILE):
	[ -d $(ROOT)/downloads ] || mkdir $(ROOT)/downloads
	wget -q -O $@ $(URL)
	sha256sum --quiet --check checksums || (rm $@; false)

$(HOSTDIR)/setup.py: $(ZIPFILE)
	[ -d $(HOSTROOT) ] || mkdir $(HOSTROOT)
	unzip $(ZIPFILE) -d $(HOSTROOT)
	touch $(HOSTDIR)/setup.py

$(HOSTBUILD)/lib/setuptools/__init__.py: $(ROOT)/.patched
	( \
		cd $(HOSTDIR); \
	$(HOSTPYTHON) setup.py build \
	)

$(BUILD)/setuptools/__init__.py: $(HOSTBUILD)/lib/setuptools/__init__.py
	[ -d $(ROOT)/build ] || mkdir $(ROOT)/build
	cp -r $(HOSTBUILD)/lib $(ROOT)/build

$(ROOT)/.patched: $(HOSTDIR)/setup.py
	cat patches/*.patch | (cd $(HOSTDIR) ; patch -p1 ; pwd)
	touch $@
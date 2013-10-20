DISTDIR = $(HOME)/distfiles
WORKDIR = $(HOME)/work
GAUCHE_DIST = Gauche-0.9.3.3.tgz
GAUCHE_DISTFILE = $(DISTDIR)/$(GAUCHE_DIST)
GAUCHE_SRCDIR := $(WORKDIR)/gauche-$(shell date +%Y%m%d-%H%M)/
GAUCHE_BUILDDIR = $(GAUCHE_SRCDIR)/Gauche-0.9.3.3

gauche: gauche-build

$(GAUCHE_DISTFILE):
	[ -d $(DISTDIR) ] || mkdir $(DISTDIR)
	cd $(DISTDIR) && wget 'http://prdownloads.sourceforge.net/gauche/$(GAUCHE_DIST)'

gauche-src: $(GAUCHE_DISTFILE)
	mkdir -p $(GAUCHE_SRCDIR)
	cd $(GAUCHE_SRCDIR) && tar xvfz $(GAUCHE_DISTFILE)

gauche-build: gauche-src
	cd $(GAUCHE_BUILDDIR) && ./configure --prefix=$(HOME)/local/gauche && make && make check && make install

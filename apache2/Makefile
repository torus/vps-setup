DISTDIR = $(HOME)/distfiles
WORKDIR = $(HOME)/work
APACHE_DIST = httpd-2.2.17.tar.bz2
APACHE_DISTFILE = $(DISTDIR)/$(APACHE_DIST)
APACHE_SRCDIR = $(WORKDIR)/apache-$(shell date +%Y%m%d-%H%M)/
APACHE_BUILDDIR = $(APACHE_SRCDIR)/httpd-2.2.17

apache: apache-build

$(APACHE_DISTFILE):
	[ -d $(DISTDIR) ] || mkdir $(DISTDIR)
	cd $(DISTDIR) && wget 'http://apache.cyberuse.com/httpd/$(APACHE_DIST)'

apache-src: $(APACHE_DISTFILE)
	mkdir -p $(APACHE_SRCDIR)
	cd $(APACHE_SRCDIR) && tar xvfj $(APACHE_DISTFILE)

apache-build: apache-src
	cd $(APACHE_BUILDDIR) && ./configure --prefix=$$HOME/local/apache2 --enable-rewrite && make && make install
	mv $$HOME/local/apache2/conf/httpd.conf $$HOME/local/apache2/conf/httpd.conf.orig
	cp $$HOME/scripts/httpd.conf $$HOME/local/apache2/conf/httpd.conf

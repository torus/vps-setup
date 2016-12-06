DISTDIR = $(HOME)/distfiles
WORKDIR = $(HOME)/work
APACHE_DIST = httpd-2.4.7.tar.bz2
APACHE_DISTFILE = $(DISTDIR)/$(APACHE_DIST)
APACHE_SRCDIR := $(WORKDIR)/apache-$(shell date +%Y%m%d-%H%M)/
APACHE_BUILDDIR = $(APACHE_SRCDIR)/httpd-2.4.7

apache: apache-build apache-run

$(APACHE_DISTFILE):
	[ -d $(DISTDIR) ] || mkdir $(DISTDIR)
	cd $(DISTDIR) && wget 'http://ftp.yz.yamagata-u.ac.jp/pub/network/apache/httpd/$(APACHE_DIST)'

apache-src: $(APACHE_DISTFILE)
	mkdir -p $(APACHE_SRCDIR)
	cd $(APACHE_SRCDIR) && tar xvfj $(APACHE_DISTFILE)

apache-build: apache-src
	cd $(APACHE_BUILDDIR) && ./configure --prefix=$(HOME)/local/apache2 --enable-proxy --enable-rewrite && make && make install
	mv $(HOME)/local/apache2/conf/httpd.conf $(HOME)/local/apache2/conf/httpd.conf.orig
	cp httpd.conf $(HOME)/local/apache2/conf/httpd.conf
	mkdir -p $(HOME)/var/www/html
	sudo chown -R daemon:daemon $(HOME)/local/apache2/logs

apache-run:
	sudo $(HOME)/local/apache2/bin/apachectl start
	echo '0 * * * * pidof httpd > /dev/null || sudo $(HOME)/local/apache2/bin/apachectl start' | sudo crontab -

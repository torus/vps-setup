DISTDIR = $(HOME)/distfiles
WORKDIR = $(HOME)/work
PREFIX = $(HOME)/local/postfix
POSTFIX_DIST = postfix-2.8.0.tar.gz
POSTFIX_DISTFILE = $(DISTDIR)/$(POSTFIX_DIST)
POSTFIX_SRCDIR := $(WORKDIR)/postfix-$(shell date +%Y%m%d-%H%M)/
POSTFIX_BUILDDIR = $(POSTFIX_SRCDIR)/postfix-2.8.0

# POSTFIX_BUILTIN_PARAMETERS = CCARGS='\
#     -DDEF_COMMAND_DIR=\"$(PREFIX)/usr/sbin\" \
#     -DDEF_CONFIG_DIR=\"$(PREFIX)/etc/postfix\" \
#     -DDEF_DAEMON_DIR=\"$(PREFIX)/usr/libexec/postfix\" \
#     -DDEF_DATA_DIR=\"$(PREFIX)/var/lib/postfix\" \
#     -DDEF_MAILQ_PATH=\"$(PREFIX)/usr/bin/mailq\" \
#     -DDEF_MANPAGE_DIR=\"$(PREFIX)/usr/local/man\" \
#     -DDEF_NEWALIAS_PATH=\"$(PREFIX)/usr/bin/newaliases\" \
#     -DDEF_QUEUE_DIR=\"$(PREFIX)/var/spool/postfix\" \
#     -DDEF_SENDMAIL_PATH=\"$(PREFIX)/usr/sbin/sendmail\" \
#     '



postfix: postfix-build postfix-create-account postfix-config postfix-install postfix-run dot-forward

$(POSTFIX_DISTFILE):
	[ -d $(DISTDIR) ] || mkdir $(DISTDIR)
	cd $(DISTDIR) && wget 'http://www.tigertech.net/mirrors/postfix-release/official/$(POSTFIX_DIST)'

postfix-src: $(POSTFIX_DISTFILE)
	mkdir -p $(POSTFIX_SRCDIR)
	cd $(POSTFIX_SRCDIR) && tar xvfz $(POSTFIX_DISTFILE)

postfix-build: postfix-src
	make -C$(POSTFIX_BUILDDIR) makefiles $(POSTFIX_BUILTIN_PARAMETERS) && \
	make -C$(POSTFIX_BUILDDIR)

postfix-create-account:
	sudo /usr/sbin/useradd -r -U -s /no/shell -d /no/where postfix || echo 'user postfix may exist.'
	sudo /usr/sbin/groupadd postdrop || echo 'group postdrop may exist.'

postfix-install:
	sudo make -C$(POSTFIX_BUILDDIR) upgrade

postfix-config:
	[ -d /etc/postfix ] || sudo mkdir /etc/postfix
	[ -f /etc/postfix/main.cf ] && sudo cp /etc/postfix/main.cf /etc/postfix/main.cf.orig.`date +%Y-%m-%d` || echo 'no mail.cf'
	sudo cp main.cf /etc/postfix/

postfix-apt:
	sudo apt-get install postfix

postfix-run:
	sudo /usr/bin/newaliases
	echo '0 * * * * /usr/sbin/postfix status > /dev/null 2>&1 || /usr/sbin/postfix start' | sudo crontab -
	sudo /usr/sbin/postfix start

dot-forward:
	echo 'toru.gwaihir@gmail.com' >> $(HOME)/.forward
	echo '~/Maildir/' >> $(HOME)/.forward

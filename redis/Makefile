DISTDIR = $(HOME)/distfiles
WORKDIR = $(HOME)/work
REDIS_DIST = redis-2.6.16.tar.gz
REDIS_DISTFILE = $(DISTDIR)/$(REDIS_DIST)
REDIS_SRCDIR := $(WORKDIR)/redis-$(shell date +%Y%m%d-%H%M)/
REDIS_BUILDDIR = $(REDIS_SRCDIR)/redis-2.6.16

redis: redis-build

$(REDIS_DISTFILE):
	[ -d $(DISTDIR) ] || mkdir $(DISTDIR)
	cd $(DISTDIR) && wget 'http://download.redis.io/releases/$(REDIS_DIST)'

redis-src: $(REDIS_DISTFILE)
	mkdir -p $(REDIS_SRCDIR)
	cd $(REDIS_SRCDIR) && tar xvfz $(REDIS_DISTFILE)

redis-build: redis-src
	cd $(REDIS_BUILDDIR) && make && make test && make install PREFIX=$(HOME)/local/redis

LDFLAGS = -ltcl8.5
CFLAGS = -Wall -W
BINS = rivet.cgi

all: $(BINS)

rivet.cgi: rivet
	rm -f rivet.cgi
	ln rivet rivet.cgi

rivet-render-static: Makefile rivet-render-static.in $(shell find ../packages/tclrivet -type f -name '*.tcl')
	rm -f rivet-render-static.new rivet-render-static.head rivet-render-static.body
	echo '#! /usr/bin/env tclsh' > rivet-render-static.head
	@set -x; for file in $(shell find ../packages/tclrivet -type f -name '*.tcl' | egrep -v '/(rivet_ncgi|commserver)/' | egrep -v '/(pkgIndex|parse)\.tcl$$') rivet-render-static.in; do \
		grep 'package provide' "$${file}" >> rivet-render-static.head; \
		cat "$${file}" | grep -v '\bsource ' | grep -v ' auto_path ' >> rivet-render-static.body || exit 1; \
	done
	cat rivet-render-static.head rivet-render-static.body > rivet-render-static.new
	rm -f rivet-render-static.head rivet-render-static.body
	chmod +x rivet-render-static.new
	mv rivet-render-static.new rivet-render-static

rivet: rivet.o
rivet.o: rivet.c

.PHONY: clean

clean:
	rm -f $(BINS) rivet *.o
	rm -f rivet-render-static.new rivet-render-static.head rivet-render-static.body
	rm -f rivet-render-static

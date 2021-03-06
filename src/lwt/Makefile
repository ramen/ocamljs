# Lightweight thread library for Objective Caml
# http://www.ocsigen.org/lwt
# Makefile
# Copyright (C) 2008 St�phane Glondu
# Laboratoire PPS - CNRS Universit� Paris Diderot
#               2009 J�r�mie Dimino
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU Lesser General Public License as
# published by the Free Software Foundation, with linking exception;
# either version 2.1 of the License, or (at your option) any later
# version.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA
# 02111-1307, USA.

# +------------------------------------------------------------------+
# | Configuration                                                    |
# +------------------------------------------------------------------+

# Tools
OCAMLFIND := ocamlfind
OCAMLBUILD := ocamlbuild

# Use classic-display when compiling under a terminal which does not
# support ANSI sequence:
ifeq ($(TERM),dumb)
OCAMLBUILD += -classic-display
endif

# Avoid compilation of native plugin if ocamlopt is not available
ifeq ($(shell if which ocamlopt >/dev/null; then echo yes; fi),)
OCAMLBUILD += -byte-plugin
endif

# General project parameters:
NAME := lwt
VERSION := $(shell head -n 1 VERSION)
DESTDIR := $(shell $(OCAMLFIND) printconf destdir)

# +------------------------------------------------------------------+
# | Rules                                                            |
# +------------------------------------------------------------------+

all:
	$(OCAMLBUILD) all
	$(OCAMLFIND) remove -destdir ../../stage $(NAME)
	$(OCAMLFIND) install -destdir ../../stage $(NAME) _build/META \
	  _build/src/core/*.mli \
	  _build/src/core/*.cmi \
	  _build/src/core/*.cmjsa

byte:
	$(OCAMLBUILD) byte

native:
	$(OCAMLBUILD) native

opt: native

tests:
	$(OCAMLBUILD) test_programs

doc:
	$(OCAMLBUILD) doc

examples:
	$(MAKE) -C examples

dist:
	DARCS_REPO=$(PWD) darcs dist -d $(NAME)-$(VERSION)

install:
	v=`ocamlfind -query -format %v lwt`; \
	if [ "x$$v" = "x" ]; then \
	  mkdir -p "$(DESTDIR)"; \
	  $(OCAMLFIND) install $(NAME) -destdir "$(DESTDIR)" _build/META \
	    $(wildcard _build/src/*/*.mli) \
	    $(wildcard _build/src/*/*.cmi) \
	    $(wildcard _build/src/*/*.cmx) \
	    $(wildcard _build/src/*/*.cma) \
	    $(wildcard _build/src/*/*.cmxa) \
	    $(wildcard _build/src/*/*.cmjsa) \
	    $(wildcard _build/src/*/*.so) \
	    $(wildcard _build/src/*/*.a) \
	    $(wildcard _build/src/*/stubs/*.so) \
	    $(wildcard _build/src/*/stubs/*.a) \
	    $(wildcard _build/src/*/stubs/*.dll) \
	    $(wildcard _build/src/top/private/toplevel.top) \
	    _build/syntax/pa_lwt.cmo \
	    _build/syntax/pa_log.cmo; \
	elif [ "x$$v" = "x2.1.1" ]; then \
	  cp $(wildcard _build/src/*/*.cmjsa) $(DESTDIR)/lwt; \
          cp _build/META $(DESTDIR)/lwt; \
        else \
	  echo "Lwt installed but not 2.1.1, not installing"; \
	fi

uninstall:
	echo "Not uninstalling Lwt" # $(OCAMLFIND) remove $(NAME) -destdir "$(DESTDIR)"

reinstall: uninstall install

clean:
	$(OCAMLBUILD) -clean
	rm -Rf *~ src/*~ $(NAME)-*.tar.gz
	$(MAKE) -C examples clean


.PHONY: sanitize all byte native opt examples install uninstall clean tests

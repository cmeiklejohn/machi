REBAR_BIN := $(shell which rebar)
ifeq ($(REBAR_BIN),)
REBAR_BIN = ./rebar
endif

.PHONY: rel deps package pkgclean

all: deps compile

compile:
	$(REBAR_BIN) compile

deps:
	$(REBAR_BIN) get-deps

clean:
	$(REBAR_BIN) -r clean

test: deps compile eunit

eunit:
	$(REBAR_BIN) -v skip_deps=true eunit

pulse: compile
	env USE_PULSE=1 $(REBAR_BIN) skip_deps=true clean compile
	env USE_PULSE=1 $(REBAR_BIN) skip_deps=true -D PULSE eunit

APPS = kernel stdlib sasl erts ssl compiler eunit
PLT = $(HOME)/.tango_dialyzer_plt

build_plt: deps compile
	dialyzer --build_plt --output_plt $(PLT) --apps $(APPS) deps/*/ebin

dialyzer: deps compile
	dialyzer -Wno_return --plt $(PLT) ebin

clean_plt:
	rm $(PLT)

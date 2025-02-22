SHELL := /bin/bash

include ../../common/options.mk

# Original options used by Vince
EMOPTIONS = -s TOTAL_MEMORY=400000000
NB_PAGES = "5000"
NB_ITERATIONS = "10"
THRESHOLD = "0.00000001"
DIVISOR = "100000"

ASMJS_RUN_ARGUMENT = "'-n', '"$(NB_PAGES)"', '-i', '"$(NB_ITERATIONS)"', '-t', '"$(THRESHOLD)"', '-q', '"$(DIVISOR)"'"
C_RUN_ARGUMENT = "-n "$(NB_PAGES)" -i "$(NB_ITERATIONS)" -t "$(THRESHOLD)" -q "$(DIVISOR)
JS_RUN_ARGUMENT = "runPageRank("$(NB_PAGES)", "$(NB_ITERATIONS)", "$(THRESHOLD)", "$(DIVISOR)")"
WEBCL_CALL = "webclPR("$(OPENCL_PLATFORM)", "$(OPENCL_DEVICE)", "$(NB_PAGES)", "$(NB_ITERATIONS)", "$(THRESHOLD)", "$(DIVISOR)")"
FILENAME = "pagerank"

build: build/js/run.html

build/wasm:
	mkdir -p build/wasm

build/wasm/run.html: build/wasm \
                    common/common.c \
                    c/pagerank.c
	$(EMCC)  -I../../common/ \
             -Icommon/ \
             ../../common/common_rand.c \
             common/common.c \
             c/pagerank.c \
             $(EMOPTIONS) \
			 -lm -o build/wasm/run.html

build/js:
	mkdir -p build/js

build/js/pagerank.js:
	cp js/pagerank.js build/js/

build/js/run.html: build/js build/js/pagerank.js
	cat ../../utils/js/run_template.html > build/js/run.html
	sed -i.bak s/FILENAME/$(FILENAME)/g build/js/run.html
	sed -i.bak s/RUN_ARGUMENT/$(JS_RUN_ARGUMENT)/g build/js/run.html

clean:
	rm -rf build

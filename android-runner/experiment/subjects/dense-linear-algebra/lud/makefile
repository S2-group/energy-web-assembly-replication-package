SHELL := /bin/bash

include ../../common/options.mk

EMOPTIONS = -s TOTAL_MEMORY=399966208 #400000000

SIZE = "1024"
ASMJS_RUN_ARGUMENT = "'-s', '"$(SIZE)"'"
C_RUN_ARGUMENT = "-s "$(SIZE)
JS_RUN_ARGUMENT = "ludRun("$(SIZE)")"
WEBCL_CALL = "webclLUD("$(OPENCL_PLATFORM)", "$(OPENCL_DEVICE)", "$(SIZE)")"
FILENAME = "lud"

build: build/wasm/run.html build/js/run.html

build/wasm:
	mkdir -p build/wasm

build/wasm/run.html: build/wasm \
            c/lud.c \
			c/lud_base.c \
            common/common.c
	$(EMCC) c/lud.c \
            c/lud_base.c \
			common/common.c \
			../../common/common_rand.c \
			-Icommon \
			-I../../common \
			-lm $(EMOPTIONS) -s WASM=1 -o build/wasm/lud.html

build/js:
	mkdir -p build/js

build/js/lud.js:
	cp js/lud.js build/js/$(FILENAME).js

build/js/run.html: build/js build/js/lud.js
	cat ../../utils/js/run_template.html > build/js/run.html
	sed -i.bak s/FILENAME/$(FILENAME)/g build/js/run.html
	sed -i.bak s/RUN_ARGUMENT/$(JS_RUN_ARGUMENT)/g build/js/run.html

clean:
	rm -rf build

#
# Based on linux/samples/bpf/Makefile
#

# kbuild trick to avoid linker error. Can be omitted if a module is built.
obj- := dummy.o

# List of programs to build
hostprogs-y := libextfuse.so

# External-module Kbuild on the preserved 5.2 tree may expose srctree/objtree
# as '.'.  Fig. 9 passes both absolute paths explicitly; the defaults preserve
# the original in-tree behavior for other callers.
EXTFUSE_KERNEL_SOURCE ?= $(srctree)
EXTFUSE_KERNEL_BUILD ?= $(objtree)

libextfuse.so-objs := \
	src/ebpf.o src/libbpf.o src/bpf_load.o

# Generate .c files based on kernel source
%.c:
	cp $(EXTFUSE_KERNEL_SOURCE)/samples/bpf/bpf_load.c $(PWD)/src/.
	if [ -f $(EXTFUSE_KERNEL_SOURCE)/samples/bpf/libbpf.c ]; then \
		cp $(EXTFUSE_KERNEL_SOURCE)/samples/bpf/libbpf.c $(PWD)/src/.; \
	else \
		cp $(EXTFUSE_KERNEL_SOURCE)/tools/lib/bpf/bpf.c $(PWD)/src/libbpf.c; \
	fi;

# Tell kbuild to always build the programs
always := $(hostprogs-y)
always += bpf/extfuse.o

EXTRA_CFLAGS += -I$(PWD)/include -I$(EXTFUSE_KERNEL_SOURCE)/samples/bpf
ifneq ("$(wildcard $(EXTFUSE_KERNEL_SOURCE)/samples/bpf/asm_goto_workaround.h)","")
	EXTRA_CFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/samples/bpf/ -include asm_goto_workaround.h
endif

ifdef HOSTCFLAGS
HOSTCFLAGS += -fPIC -I$(PWD)/include
HOSTCFLAGS += -I$(EXTFUSE_KERNEL_BUILD)/usr/include
HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/samples/bpf
HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/tools/lib/bpf
HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/tools/lib -I$(EXTFUSE_KERNEL_SOURCE)/tools/include
HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/tools/perf
HOSTLOADLIBES_libextfuse.so += -shared -lelf -lpthread
else
KBUILD_HOSTCFLAGS += -fPIC -I$(PWD)/include
KBUILD_HOSTCFLAGS += -I$(EXTFUSE_KERNEL_BUILD)/usr/include
KBUILD_HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/samples/bpf
KBUILD_HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/tools/lib/bpf
KBUILD_HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/tools/testing/selftests/bpf/
KBUILD_HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/tools/lib -I$(EXTFUSE_KERNEL_SOURCE)/tools/include
KBUILD_HOSTCFLAGS += -I$(EXTFUSE_KERNEL_SOURCE)/tools/perf
HOSTLDLIBS_libextfuse.so += -shared -lelf -lpthread
endif

HOSTCFLAGS_bpf_load.o += -I$(EXTFUSE_KERNEL_BUILD)/usr/include -Wno-unused-variable
HOSTCFLAGS_libbpf.o += -I$(EXTFUSE_KERNEL_BUILD)/usr/include -Wno-unused-variable

# Allows pointing LLC/CLANG to a LLVM backend with bpf support, redefine on cmdline:
#  make samples/bpf/ LLC=~/git/llvm/build/bin/llc CLANG=~/git/llvm/build/bin/clang
LLC ?= llc
CLANG ?= clang

# Trick to allow make to be run from this directory
all:
	$(MAKE) -C /lib/modules/`uname -r`/build M=${PWD}

clean:
	$(MAKE) -C /lib/modules/`uname -r`/build M=${PWD} clean
	rm -f src/*.o bpf/*.o *.so
	rm -f src/libbpf.c src/bpf_load.c

# Verify LLVM compiler tools are available and bpf target is supported by llc
.PHONY: verify_cmds verify_target_bpf $(CLANG) $(LLC)

verify_cmds: $(CLANG) $(LLC)
	@for TOOL in $^ ; do \
		if ! (which -- "$${TOOL}" > /dev/null 2>&1); then \
			echo "*** ERROR: Cannot find LLVM tool $${TOOL}" ;\
			exit 1; \
		else true; fi; \
	done

verify_target_bpf: verify_cmds
	@if ! (${LLC} -march=bpf -mattr=help > /dev/null 2>&1); then \
		echo "*** ERROR: LLVM (${LLC}) does not support 'bpf' target" ;\
		echo "   NOTICE: LLVM version >= 3.7.1 required" ;\
		exit 2; \
	else true; fi

$(src)/*.c: verify_target_bpf

# asm/sysreg.h - inline assembly used by it is incompatible with llvm.
# But, there is no easy way to fix it, so just exclude it since it is
# useless for BPF samples.
$(obj)/%.o: $(src)/%.c
	$(CLANG) $(NOSTDINC_FLAGS) $(LINUXINCLUDE) $(EXTRA_CFLAGS) \
		-I$(EXTFUSE_KERNEL_SOURCE)/tools/testing/selftests/bpf/ \
		-I$(EXTFUSE_KERNEL_SOURCE)/fs/fuse/ \
		-D__KERNEL__ -D__BPF_TRACING__ -Wno-unused-value -Wno-pointer-sign \
		-D__TARGET_ARCH_$(ARCH) -Wno-compare-distinct-pointer-types \
		-Wno-gnu-variable-sized-type-not-at-end \
		-Wno-address-of-packed-member -Wno-tautological-compare \
		-Wno-unknown-warning-option $(CLANG_ARCH_ARGS) \
		-O2 -emit-llvm -c $< -o -| $(LLC) -march=bpf $(LLC_FLAGS) -filetype=obj -o $@

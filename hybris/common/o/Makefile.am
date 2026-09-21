bionic = $(top_srcdir)/common/o/bionic
bionic_libc = $(bionic)/libc

modlibexecdir = $(libdir)/libhybris/linker
modlibexec_LTLIBRARIES = o.la

ARCH_FLAGS =

AM_CFLAGS = \
	-std=gnu99

AM_CPPFLAGS = \
	-fno-stack-protector \
	-Wstrict-overflow=5

AM_CXXFLAGS = \
	-std=gnu++11 \
	-Wold-style-cast

o_la_SOURCES = \
	hybris_compat.cpp \
	dlfcn.cpp \
	linker_allocator.cpp \
	linker_block_allocator.cpp \
	linker.cpp \
	linker_cfi.cpp \
	linker_config.cpp \
	linker_dlwarning.cpp \
	linker_gdb_support.cpp \
	linker_globals.cpp \
	linker_logger.cpp \
	linker_main.cpp \
	linker_mapped_file_fragment.cpp \
	linker_memory.cpp \
	linker_namespaces.cpp \
	linker_phdr.cpp \
	linker_sdk_versions.cpp \
	linker_soinfo.cpp \
	linker_utils.cpp \
	rt.cpp \
	../strlcpy.c \
	../strlcat.c
o_la_CPPFLAGS = \
	$(AM_CPPFLAGS) \
	-I$(top_srcdir)/include \
	-I$(top_srcdir)/common \
	-I$(bionic_libc)/ \
	-I$(bionic_libc)/include \
	-DDEFAULT_HYBRIS_LD_LIBRARY_PATH="\"@DEFAULT_HYBRIS_LD_LIBRARY_PATH@\"" \
	$(ARCH_FLAGS) \
	$(ANDROID_HEADERS_CFLAGS)

if HAS_ANDROID_9_0_0
o_la_CPPFLAGS += -DHAS_ANDROID_9_0_0
endif

o_la_LDFLAGS = \
	-lsupc++ \
	-module \
	-avoid-version

if WANT_DEBUG
o_la_CPPFLAGS += \
	-DTRACE_DEBUG=1
else
o_la_CPPFLAGS += \
	-DTRACE_DEBUG=1
endif

if WANT_ARM_TRACING
o_la_CPPFLAGS += \
	-DWANT_ARM_TRACING
endif

# Allow LD_CONFIG_FILE to point the linker at an alternative ld.config.txt.
#
# AOSP gates this off outside debug builds because there the linker config is
# a system integrity boundary. Under libhybris the linker runs inside a glibc
# process the user already controls, next to LD_PRELOAD and LD_LIBRARY_PATH,
# so it crosses no boundary those do not.
#
# Without it, a port that needs to extend a namespace -- typically adding VNDK
# libraries to sphal.link.vndk.shared_libs so a vendor HAL can resolve its
# dependencies -- has to bind-mount over /linkerconfig/ld.config.txt, which is
# a runtime-generated tmpfs. That is a system-wide change made to fix a single
# process. With this, the same port sets LD_CONFIG_FILE for that one process.
o_la_CPPFLAGS += \
	-DUSE_LD_CONFIG_FILE


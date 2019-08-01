################################################################################
#                                                                              #
#                            M A K E F I L E                                   #
#                                                                              #
################################################################################

GPACLIB_NAME = libgpac.dylib

UNAME := $(shell uname)

GPAC_OBJS = av_parsers.o      \
            avc_ext.o         \
            avilib.o          \
            base_encoding.o   \
            bitstream.o       \
            box_code_3gpp.o   \
            box_code_adobe.o  \
            box_code_apple.o  \
            box_code_base.o   \
            box_code_drm.o    \
            box_code_meta.o   \
            box_funcs.o       \
            configfile.o      \
            data_map.o        \
            desc_private.o    \
            descriptors.o     \
            drm_sample.o      \
            error.o           \
            gpac_ogg.o        \
            hinting.o         \
            ipmpx_code.o      \
            ipmpx_parse.o     \
            isom_intern.o     \
            isom_read.o       \
            isom_store.o      \
            isom_write.o      \
            list.o            \
            math.o            \
            media.o           \
            media_odf.o       \
            meta.o            \
            movie_fragments.o \
            odf_code.o        \
            odf_codec.o       \
            odf_command.o     \
            os_config_init.o  \
            os_divers.o       \
            os_file.o         \
            qos.o             \
            sample_descs.o    \
            slc.o             \
            stbl_read.o       \
            stbl_write.o      \
            track.o           \
            tx3g.o            \
            url.o             \
            utf.o
GPAC_OBJS_WITH_PATH = $(foreach GPAC_OBJS, $(GPAC_OBJS), ./$(GPAC_OBJS))
GPAC_OBJS_IN_OBJ_DIR = $(foreach GPAC_OBJS, $(GPAC_OBJS), ../obj/$(GPAC_OBJS))

ifeq ($(UNAME), Linux)
C_FLAGS = -Wpedantic -Wno-deprecated-declarations -Wall -fno-strict-aliasing -fPIC -DPIC  -DGPAC_CONFIG_LINUX -DGPAC_DISABLE_3D -DGPAC_64_BITS
SO_FLAGS = -shared -W -L/usr/local/lib -lavformat -lavcodec -lavutil -luuid
LD_FLAGS = -L/usr/local/lib -luuid -lm -lz
LD_FLAGS_FFMPEG = -lavformat -lavcodec -lavutil
else
C_FLAGS = -Wpedantic -Wno-deprecated-declarations -DGPAC_CONFIG_DARWIN -D_FILE_OFFSET_BITS=64 -DVERSION_FILE_PRESENT -Dfopen64=fopen -Dopen64=open -Dlseek64=lseek
SO_FLAGS = -dynamiclib -undefined suppress -flat_namespace -L/usr/local/lib -lavformat -lavcodec -lavutil
LD_FLAGS = -L/usr/local/lib -L/Users/rtaylo231/robt/vx-caption-inspector/src -lz
LD_FLAGS_FFMPEG = -lavformat -lavcodec -lavutil
endif

-include ../obj/$(GPAC_OBJS:.o=.d)

all: gpac

gpac: $(GPAC_OBJS_WITH_PATH)
	gcc $(SO_FLAGS) -o ../${GPACLIB_NAME} $(GPAC_OBJS_IN_OBJ_DIR)

clean:
	@echo "*** Cleaning Target Files. ***"
	rm -f ../$(GPACLIB_NAME)
	@echo "\n*** Cleaning Object Files. ***"
	for i in $(OBJS) ; do ( rm -f ../obj/$$i ) ; done
	rm -f ../obj/main.o
	@echo "\n*** Cleaning Dependency Files. ***"
	for i in $(OBJS:.o=.d) ; do ( rm -f ../obj/$$i ) ; done
	rm -f ../obj/main.d

%.o: %.c
	@if ! [ -d "../obj" ]; then mkdir ../obj; fi
	gcc -c $(C_FLAGS) -I ../include $*.c -o ../obj/$(notdir $*.o)
	gcc -MM $(C_FLAGS) -I ../include $*.c > ../obj/$(notdir $*.d)

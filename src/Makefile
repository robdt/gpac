################################################################################
#                                                                              #
#                            M A K E F I L E                                   #
#                                                                              #
################################################################################

GPACLIB_NAME = libgpac.so

UNAME := $(shell uname)

OBJS = av_parsers.o      \
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
OBJS_WITH_PATH = $(foreach OBJS, $(OBJS), ./$(OBJS))
OBJS_IN_OBJ_DIR = $(foreach OBJS, $(OBJS), ../obj/$(OBJS))

ifeq ($(UNAME), Linux)
C_FLAGS = -Wpedantic -Wno-deprecated-declarations -Wall -fno-strict-aliasing -fPIC -DPIC  -DGPAC_CONFIG_LINUX -DGPAC_DISABLE_3D -DGPAC_64_BITS
SO_FLAGS = -shared -W -L/usr/local/lib
LD_FLAGS = -L/usr/local/lib -lm -lz
else
C_FLAGS = -Wpedantic -Wno-deprecated-declarations -DGPAC_CONFIG_DARWIN -D_FILE_OFFSET_BITS=64 -DVERSION_FILE_PRESENT -Dfopen64=fopen -Dopen64=open -Dlseek64=lseek
SO_FLAGS = -dynamiclib -undefined suppress -flat_namespace -L/usr/local/lib
LD_FLAGS = -L/usr/local/lib -lz
endif

-include ../obj/$(OBJS:.o=.d)

all: gpac

gpac: $(OBJS_WITH_PATH)
	clang $(SO_FLAGS) -o ../${GPACLIB_NAME} $(OBJS_IN_OBJ_DIR)

install: gpac
	cp ../${GPACLIB_NAME} /usr/local/lib/${GPACLIB_NAME}
	cp -r ../include/gpac /usr/local/include

clean:
	@echo "*** Cleaning Target Files. ***"
	rm -f ../$(GPACLIB_NAME)
	rm -f /usr/local/lib/$(GPACLIB_NAME)
	rm -rf /usr/local/include/gpac
	@echo "\n*** Cleaning Object Files. ***"
	for i in $(OBJS) ; do ( rm -f ../obj/$$i ) ; done
	@echo "\n*** Cleaning Dependency Files. ***"
	for i in $(OBJS:.o=.d) ; do ( rm -f ../obj/$$i ) ; done

%.o: %.c
	@if ! [ -d "../obj" ]; then mkdir ../obj; fi
	clang -c $(C_FLAGS) -I ../include $*.c -o ../obj/$(notdir $*.o)
	clang -MM $(C_FLAGS) -I ../include $*.c > ../obj/$(notdir $*.d)

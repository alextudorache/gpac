include ../../config.mak

vpath %.c $(SRC_PATH)/modules/aac_in

CFLAGS= $(OPTFLAGS) -I"$(SRC_PATH)/include"

ifeq ($(DEBUGBUILD), yes)
CFLAGS+=-g
LDFLAGS+=-g
endif

ifeq ($(GPROFBUILD), yes)
CFLAGS+=-pg
LDFLAGS+=-pg
endif

#common obj
OBJS= aac_in.o

SRCS := $(OBJS:.o=.c) 

#faad config
ifeq ($(CONFIG_FAAD), no)
else
OBJS+=faad_dec.o
CFLAGS+=-DGPAC_HAS_FAAD
#local faad lib
ifeq ($(CONFIG_FAAD), local)
EXTRALIBS+=-L../../extra_lib/lib/gcc
CFLAGS+= -I"$(LOCAL_INC_PATH)/faad"
endif
EXTRALIBS+= -lfaad
endif

LIB=gm_aac_in$(DYN_LIB_SUFFIX)
ifeq ($(CONFIG_WIN32),yes)
#LDFLAGS+=-export-symbols aac_in.def 
endif


all: $(LIB)


$(LIB): $(OBJS)
	$(CC) $(SHFLAGS) -o ../../bin/gcc/$@ $(OBJS) -L../../bin/gcc -lgpac $(EXTRALIBS) $(LDFLAGS)
ifeq ($(STATICBUILD),yes)
	$(CC) $(SHFLAGS) -o ../../bin/gcc/gm_aac_in-static$(DYN_LIB_SUFFIX) $(OBJS) -L../../bin/gcc -lgpac_static $(EXTRALIBS) $(LDFLAGS)
endif

clean: 
	rm -f $(OBJS) ../../bin/gcc/$(LIB)

dep: depend

depend:
	rm -f .depend	
	$(CC) -MM $(CFLAGS) $(SRCS) 1>.depend

distclean: clean
	rm -f Makefile.bak .depend

-include .depend

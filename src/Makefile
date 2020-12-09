include ../Makefile.config

CC=x86_64-rumprun-netbsd-gcc
LDFLAGS=-L../obj/CLHT/external/lib -L../obj/CLHT -Wl,--whole-archive  -lsspfd -lssmem -lclht -Wl,--no-whole-archive  -lm -lpapi -m64 -lpthread
CFLAGS=-I../include/ -I../obj/CLHT/include/ -I../obj/CLHT/external/include/ -D_GNU_SOURCE -fPIC -Wall -Werror -O3 -g -D_REENTRANT -DOS_NETBSD -DLEVELDB_PLATFORM_POSIX -O2 -DNDEBUG

# Keep objects files
.PRECIOUS: %.o
.SECONDARY: $(OBJS)

.SECONDEXPANSION:
../obj/%.o: $$(lastword $$(subst /, ,%)).c $$(lastword $$(subst /, ,%)).h
	$(eval $@_TMP := $(shell echo $@ | cut -d/ -f3 | cut -d_ -f1))
	$(CC) $(CFLAGS) -D$$(echo $@ | cut -d/ -f3 | cut -d_ -f1 | tr '[a-z]' '[A-Z]') -DCOND_VAR=$(COND_VAR) -DFCT_LINK_SUFFIX=$($@_TMP) -DWAITING_$$(echo $@ | cut -d/ -f3 | cut -d_ -f2- | tr '[a-z]' '[A-Z]') -o $@ -c $<

.SECONDEXPANSION:
../obj/%.o: $$(firstword $$(subst _, , $$(lastword $$(subst /, ,%)))).c ../include/$$(firstword $$(subst _, , $$(lastword $$(subst /, ,%)))).h
	$(eval $@_TMP := $(shell echo $@ | cut -d/ -f3 | cut -d_ -f1))
	$(CC) $(CFLAGS) -D$$(echo $@ | cut -d/ -f3 | cut -d_ -f1 | tr '[a-z]' '[A-Z]') -DCOND_VAR=$(COND_VAR) -DFCT_LINK_SUFFIX=$($@_TMP) -DWAITING_$$(echo $@ | cut -d/ -f3 | cut -d_ -f2- | tr '[a-z]' '[A-Z]') -o $@ -c $<

.SECONDEXPANSION:
../lib/lib%.so: ../obj/%/interpose.o ../obj/%/utils.o $$(subst algo,%,../obj/algo/algo.o)
	$(CC) -shared -o $@ $^ $(LDFLAGS)

SECONDEXPANSION:
../lib/lib%.a: ../obj/%/interpose.o ../obj/%/utils.o $$(subst algo,%,../obj/algo/algo.o)
	ar crus $@ $^

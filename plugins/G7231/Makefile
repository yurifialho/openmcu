
AC_PLUGIN_DIR=codecs/audio
prefix=/opt/openmcu-10rm/lib/ptlib

SONAME	= g7231

SRCDIR	= ./src
OBJDIR	= ./obj

CC	 	=gcc
CXX	 	=g++
LDSO	 	=-shared -Wl,-soname,$(SONAME)
PLUGINEXT	=so
CFLAGS	       +=-g -O2 
STDCCFLAGS     +=-fPIC -D__STDC_CONSTANT_MACROS
LDFLAGS	       +=-L/opt/openmcu-10rm/lib -lavutil -lavcodec
CPPFLAGS       +=-I/opt/openmcu-10rm/include
OPTCCFLAGS      =$(CPPFLAGS)

SRCS	+= g7231codec.cxx

vpath	%.o $(OBJDIR)
vpath	%.cxx $(SRCDIR)

$(OBJDIR)/%.o : %.cxx
	@mkdir -p $(OBJDIR) >/dev/null 2>&1
	$(CXX) -I${prefix}/include $(STDCCFLAGS) $(OPTCCFLAGS) $(CFLAGS) -c $< -o $@

PLUGIN	= ./g7231_audio_pwplugin.$(PLUGINEXT)

OBJECTS = $(addprefix $(OBJDIR)/,$(patsubst %.cxx,%.o,$(notdir $(SRCS))))

$(PLUGIN): $(OBJECTS)
	$(CXX) $(LDSO) -o $@ $^ $(EXTRALIBS) $(LDFLAGS)

install:
	mkdir -p $(DESTDIR)$(prefix)/$(AC_PLUGIN_DIR)
	cp $(PLUGIN) $(DESTDIR)$(prefix)/$(AC_PLUGIN_DIR)

uninstall:
	rm -f $(DESTDIR)$(prefix)/$(AC_PLUGIN_DIR)/$(PLUGIN)

clean:
	rm -f $(OBJECTS) $(PLUGIN)

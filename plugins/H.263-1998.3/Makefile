#
# Makefile for GSM 06.10
#
# Copyright (C) 2004 Post Increment, All Rights Reserved
#
# The contents of this file are subject to the Mozilla Public License
# Version 1.0 (the "License"); you may not use this file except in
# compliance with the License. You may obtain a copy of the License at
# http://www.mozilla.org/MPL/
#
# Software distributed under the License is distributed on an "AS IS"
# basis, WITHOUT WARRANTY OF ANY KIND, either express or implied. See
# the License for the specific language governing rights and limitations
# under the License.
#
# The Original Code is Open H323 library.
#
# The Initial Developer of the Original Code is Post Increment
#
# Contributor(s): Matthias Schneider.
#

VC_PLUGIN_DIR=codecs/video
prefix=/opt/openmcu-10rm/lib/ptlib
exec_prefix=${prefix}
target_os=linux-gnu

DL_LIBS =-ldl

BASENAME	=h263-1998
SONAME		=$(BASENAME)
COMMONDIR	=./opal
PLUGINDIR	=./opal
OBJDIR 		=./obj

CC	 	=gcc
CXX	 	=g++
LDSO	 	=-shared -Wl,-soname,$(SONAME)
PLUGINEXT	=so
CFLAGS	       +=-g -O2 
STDCCFLAGS     +=-fPIC -D__STDC_CONSTANT_MACROS
LDFLAGS	       +=-L/opt/openmcu-10rm/lib -lavcodec -lavutil
CPPFLAGS       +=-I/opt/openmcu-10rm/include
OPTCCFLAGS      =$(CPPFLAGS)
EXTRACFLAGS	= -I$(PLUGINDIR)
LIBAVCODEC_CFLAGS=-I${prefix}/include

# HAVE_LIBAVCODEC_RTP_MODE=1

ifdef HAVE_LIBAVCODEC_RTP_MODE
STDCCFLAGS += -DLIBAVCODEC_RTP_MODE=1
endif

# TRACE_FILE=1

ifdef TRACE_FILE
CFLAGS         += -DTRACE_FILE=1
endif

EXTRACCFLAGS    += $(LIBAVCODEC_CFLAGS) -I$(COMMONDIR) -I$(PLUGINDIR)

vpath   %.cxx $(COMMONDIR)
vpath   %.o   $(OBJDIR)


SRCS	+= h263-1998.cxx \
	   h263pframe.cxx \
	   rfc2190.cxx \
	   $(COMMONDIR)/trace.cxx \
	   $(COMMONDIR)/mpi.cxx

$(OBJDIR)/%.o : %.cxx
	@mkdir -p $(OBJDIR) >/dev/null 2>&1
	$(Q_CC)$(CXX) $(EXTRACCFLAGS) $(STDCCFLAGS) $(OPTCCFLAGS) $(CFLAGS) $(EXTRACFLAGS) -c $< -o $@

PLUGIN	= ./$(BASENAME)_video_pwplugin.$(PLUGINEXT)
STATIC	= ./lib$(BASENAME)_video_s.a

OBJECTS = $(addprefix $(OBJDIR)/,$(patsubst %.cxx,%.o,$(notdir $(SRCS))))

$(PLUGIN): $(OBJECTS)
ifeq (,$(findstring solaris,$(target_os)))
	$(Q_LD)$(CXX) $(LDSO) -o $@ $^ $(LDFLAGS)
else	
	$(Q_LD)$(CXX) $(LDSO) $(DL_LIBS) $@ -o $@ $^ $(LDFLAGS)
endif
    
install: $(PLUGIN)
	mkdir -p $(DESTDIR)$(prefix)/$(VC_PLUGIN_DIR)
	install $(PLUGIN) $(DESTDIR)$(prefix)/$(VC_PLUGIN_DIR)

uninstall:
	rm -f $(DESTDIR)$(prefix)/$(VC_PLUGIN_DIR)/$(PLUGIN)

clean:
	rm -f $(OBJECTS) $(PLUGIN)

###########################################

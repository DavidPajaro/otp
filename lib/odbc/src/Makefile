#
# %CopyrightBegin%
# 
# Copyright Ericsson AB 1999-2009. All Rights Reserved.
# 
# The contents of this file are subject to the Erlang Public License,
# Version 1.1, (the "License"); you may not use this file except in
# compliance with the License. You should have received a copy of the
# Erlang Public License along with this software. If not, it can be
# retrieved online at http://www.erlang.org/.
# 
# Software distributed under the License is distributed on an "AS IS"
# basis, WITHOUT WARRANTY OF ANY KIND, either express or implied. See
# the License for the specific language governing rights and limitations
# under the License.
# 
# %CopyrightEnd%
#

#
include $(ERL_TOP)/make/target.mk

ifeq ($(TYPE),debug)
ERL_COMPILE_FLAGS += -Ddebug -W
endif

include $(ERL_TOP)/make/$(TARGET)/otp.mk

# ----------------------------------------------------
# Application version
# ----------------------------------------------------
include ../vsn.mk
VSN=$(ODBC_VSN)
APP_VSN = "odbc-$(VSN)"
# ----------------------------------------------------
# Release directory specification
# ----------------------------------------------------
RELSYSDIR = $(RELEASE_PATH)/lib/odbc-$(VSN)

# ----------------------------------------------------
# Target Specs
# ----------------------------------------------------
MODULES= \
	odbc \
	odbc_app \
	odbc_sup

HRLS =  odbc_internal 

INCLUDE=../include
ODBC_HRL=$(INCLUDE)/odbc.hrl

EXT_HRL_FILES= $(ODBC_HRL)
HRL_FILES = $(HRLS:%=%.hrl)
ERL_FILES= $(MODULES:%=%.erl)	

APP_FILE = odbc.app
APP_SRC = $(APP_FILE).src
APP_TARGET = $(EBIN)/$(APP_FILE)

APPUP_FILE = odbc.appup
APPUP_SRC = $(APPUP_FILE).src
APPUP_TARGET = $(EBIN)/$(APPUP_FILE)

TARGET_FILES = $(MODULES:%=$(EBIN)/%.$(EMULATOR))

# ----------------------------------------------------
# ODBC FLAGS
# ----------------------------------------------------
ODBC_FLAGS = -D'SERVER_SOFTWARE="odbc/$(VSN)"' \

# ----------------------------------------------------
# FLAGS
# ----------------------------------------------------
ERL_COMPILE_FLAGS += -I$(INCLUDE) \
		     $(ODBC_FLAGS) \
                     +'{parse_transform,sys_pre_attributes}' \
                     +'{attribute,insert,app_vsn,$(APP_VSN)}'
# ----------------------------------------------------
# Targets
# ----------------------------------------------------
#debug:
#	@${MAKE} TYPE=debug opt

debug opt:	$(TARGET_FILES) $(APP_TARGET) $(APPUP_TARGET)

clean:
	rm -f $(TARGET_FILES)
	rm -f $(APP_TARGET)
	rm -f $(APPUP_TARGET)
	rm -f errs core *~
# ---------------------------------------------------
# Special Target
# ---------------------------------------------------


$(APP_TARGET): $(APP_SRC) ../vsn.mk
	sed -e 's;%VSN%;$(VSN);' $< > $@
$(APPUP_TARGET): $(APPUP_SRC) ../vsn.mk
	sed -e 's;%VSN%;$(VSN);' $< > $@

docs:

# ----------------------------------------------------
# Release Target
# ---------------------------------------------------- 
include $(ERL_TOP)/make/otp_release_targets.mk


release_spec: opt
	$(INSTALL_DIR) "$(RELSYSDIR)/src"
	$(INSTALL_DATA) $(ERL_FILES) $(HRL_FILES) "$(RELSYSDIR)/src"
	$(INSTALL_DIR) "$(RELSYSDIR)/include"
	$(INSTALL_DATA) $(EXT_HRL_FILES) "$(RELSYSDIR)/include"
	$(INSTALL_DIR) "$(RELSYSDIR)/ebin"
	$(INSTALL_DATA) $(TARGET_FILES) $(APP_TARGET) $(APPUP_TARGET) \
	"$(RELSYSDIR)/ebin"


release_docs_spec:



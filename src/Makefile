CC = gcc
LIBS := dbus-1
CFLAGS = $(shell pkg-config --cflags dbus-1)

LIBS_INC := $(foreach lib,$(LIBS),-l$(lib))

PKG_NAME = polybar-spotifyd-module
BASE_INSTALL_PREFIX = 

IDIR = ../include
ODIR = ../obj
BIN_DIR = ../bin

_DEPS = utils.h
DEPS = $(patsubst %,$(IDIR)/%,$(_DEPS))

_OBJS = utils.o
OBJS = $(patsubst %,$(ODIR)/%,$(_OBJS))

_EXE_DEPS = spotifyd-listener.h spotifydctl.h
EXE_DEPS = $(patsubst %,$(IDIR)/%,$(_EXE_DEPS))

_EXE_OBJS = spotifyd-listener.o spotifydctl.o
EXE_OBJS = $(patsubst %,$(ODIR)/%,$(_EXE_OBJS))

_EXES = spotifyd-listener spotifydctl
EXES = $(patsubst %,$(BIN_DIR)/%,$(_EXES))

LICENSE_FILE = ../LICENSE
README_FILE = ../README.md
SERVICE_FILE_NAME = spotifyd-listener.service

BIN_INSTALL_DIR = /usr/bin
LICENSE_INSTALL_PATH = /usr/share/licenses/$(PKG_NAME)/LICENSE
README_INSTALL_PATH = /usr/share/doc/$(PKG_NAME)/README.md
SERVICE_INSTALL_PATH = /usr/lib/systemd/user/spotifyd-listener.service

debug: CFLAGS += -g


all: spotifydctl spotifyd-listener

debug: spotifydctl spotifyd-listener

install: spotifydctl spotify-listener
	install -Dm755 -t $(BASE_INSTALL_PREFIX)$(BIN_INSTALL_DIR) $(EXES) 
	install -Dm644 $(LICENSE_FILE) $(BASE_INSTALL_PREFIX)$(LICENSE_INSTALL_PATH)
	install -Dm644 $(README_FILE) $(BASE_INSTALL_PREFIX)$(README_INSTALL_PATH)
	install -Dm644 $(SERVICE_FILE_NAME) $(BASE_INSTALL_PREFIX)$(SERVICE_INSTALL_PATH)
	@echo
	@echo "You can follow the below steps if you are using systemd"
	@echo "To enable spotifyd-listener at startup run:"
	@echo "systemd --user enable spotifyd-listener"
	@echo "To start spotifyd-listener right now, run:"
	@echo "systemd --user start spotifyd-listener"

uninstall:
	rm $(addprefix $(BIN_INSTALL_DIR)/,$(_EXES))
	rm $(LICENSE_INSTALL_PATH)
	rm $(README_INSTALL_PATH)
	rm $(SERVICE_INSTALL_PATH)

spotify-listener: $(OBJS) $(ODIR)/spotifyd-listener.o
	mkdir -p $(BIN_DIR)
	$(CC) -o $(BIN_DIR)/spotifyd-listener $^ $(CFLAGS) $(LIBS_INC)

spotifyctl: $(OBJS) $(ODIR)/spotifydctl.o
	mkdir -p $(BIN_DIR)
	$(CC) -o $(BIN_DIR)/spotifydctl $^ $(CFLAGS) $(LIBS_INC)

$(ODIR)/%.o: %.c $(DEPS) $(EXE_DEPS)
	mkdir -p $(ODIR)
	$(CC) -c -o $@ $< $(CFLAGS) $(LIBS_INC)

.PHONY: clean uninstall

clean:
	rm -f $(ODIR)/*.o *~ core vgcore.* $(IDIR)/*~ $(BIN_DIR)/*


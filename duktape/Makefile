#
# Copyright (C) Dog Hunter AG 2014
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=duktape
PKG_VERSION:=1.2.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://duktape.org/
PKG_HASH:=a52ff1af9750361041f51fb42cc74d3a6a2cb1be839034437f177625903811ca

include $(INCLUDE_DIR)/package.mk

define Package/duktape
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=Duktape embeddable JavaScript interpreter
	URL:=https://www.duktape.org
endef

define Package/duktape/description
	Duktape is a light-weight embeddable JavaScript interpreter.  It is provided as
	a C source file that is to be compiled into other projects.  Because it is
	highly configurable at compile-time, it is not suitable for being built as a
	library.
endef

define Build/Prepare
	$(Build/Prepare/Default)
	$(CP) $(PKG_BUILD_DIR)/Makefile.cmdline $(PKG_BUILD_DIR)/Makefile
endef

define Build/Configure
endef

define Build/Compile
	CC=$(TARGET_CC) \
	CCOPTS="$(TARGET_CFLAGS)" \
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR="$(PKG_INSTALL_DIR)"
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr
	$(INSTALL_DIR) $(1)/usr/src
	$(INSTALL_DIR) $(1)/usr/src/duktape
	$(INSTALL_DIR) $(1)/usr/src/duktape/src
	$(CP) $(PKG_BUILD_DIR)/src/*.c $(1)/usr/src/duktape/src
	$(CP) $(PKG_BUILD_DIR)/src/*.h $(1)/usr/src/duktape/src
endef

define Package/duktape/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/duk $(1)/usr/bin/
endef

$(eval $(call BuildPackage,duktape))

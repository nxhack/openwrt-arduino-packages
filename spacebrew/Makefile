#
# Copyright (c) 2013 Arduino LLC. All right reserved.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=spacebrew
PKG_VERSION:=2.3.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/julioterra/yunSpacebrew.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=0299adaca762f75afd4551aa63ff54c703cbd7e6
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=de0f598dc7f18fe3b7cc744e17c66912fa1649f5d089163bc7f343d8427e16a7

PKG_MAINTAINER:=
PKG_LICENSE:=
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/package.mk

define Package/spacebrew
  SECTION:=net
  CATEGORY:=Network
  TITLE:= Easily connect the Arduino Yun to Spacebrew
  DEPENDS:=+python-light
  URL:=https://github.com/julioterra/yunSpacebrew
endef

define Package/spacebrew/description
	This library was developed to enable you to easily connect the Arduino Yun to Spacebrew. To learn more about Spacebrew visit Spacebrew.cc
endef

define Build/Compile
	# NOOP
endef

define Package/spacebrew/install
	$(INSTALL_DIR) $(1)/usr/lib/python2.7/websocket
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/linino/websocket/* $(1)/usr/lib/python2.7/websocket/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/linino/websocket/websocket.py $(1)/usr/lib/python2.7/websocket/

	$(INSTALL_DIR) $(1)/usr/lib/python2.7/spacebrew
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/linino/spacebrew/* $(1)/usr/lib/python2.7/spacebrew/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/linino/spacebrew/spacebrew.py $(1)/usr/lib/python2.7/spacebrew/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/linino/spacebrew/getprocpid.py $(1)/usr/lib/python2.7/spacebrew/

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/linino/shell/run-spacebrew $(1)/usr/bin
endef

$(eval $(call BuildPackage,spacebrew))


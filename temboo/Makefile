#
# Copyright (c) 2013 Arduino LLC. All right reserved.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=temboo
PKG_VERSION:=1.4.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://downloads.arduino.cc/
PKG_HASH:=3ff22dcfbe1260b4c308a56b8dcfd67c19c19304e4b7298a562d87c6a5cc9424

PKG_MAINTAINER:=
PKG_LICENSE:=
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/package.mk

define Package/temboo
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Build Inventive Apps With Ease
  URL:=https://temboo.com/arduino
  DEPENDS:=+python-light
endef

define Package/temboo/description
	1500+ cloud-based code components that let you create without limits.
endef

define Build/Compile
	# NOOP
endef

define Package/temboo/install
	$(INSTALL_DIR) $(1)/usr/bin

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/temboo $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tembooMessaging $(1)/usr/bin
	cp -r $(PKG_BUILD_DIR)/temboo-m2m $(1)/usr/bin
	cp -r $(PKG_BUILD_DIR)/ws4py $(1)/usr/bin
endef

$(eval $(call BuildPackage,temboo))


#
# Copyright (c) 2014-2016 Arduino LLC. All right reserved.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-arduino-webpanel
PKG_VERSION:=1.7.2
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://downloads.arduino.cc/
PKG_HASH:=5d4aebbbcb044c23a7a820fbf6c355cede80615b614a3e04a843ad6cf63d0adf

PKG_BUILD_DEPENDS:=lua/host

PKG_MAINTAINER:=
PKG_LICENSE:=GPL+
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/package.mk

define Package/luci-app-arduino-webpanel
  SECTION:=luci
  CATEGORY:=LuCI
  SUBMENU:=3. Applications
  TITLE:= simplified wifi configuration panel
  URL:=https://github.com/arduino/YunWebUI
  DEPENDS:=+luci +luci-lib-json +uhttpd-mod-lua +yun-scripts
endef

define Package/luci-app-arduino-webpanel/description
	simplified wifi configuration panel
endef

#define Package/luci-app-arduino-webpanel/conffiles
#/www/index.html
#endef

define Build/Compile
	# NOOP
endef

define Package/luci-app-arduino-webpanel/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller/arduino
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view/arduino
	$(INSTALL_DIR) $(1)/www/luci-static/resources/arduino

	find $(PKG_BUILD_DIR)/usr/ -name '*.lua' -exec luac -s -o {} {} \;
	$(CP) $(PKG_BUILD_DIR)/usr/* $(1)/usr/
	$(CP) $(PKG_BUILD_DIR)/www/* $(1)/www/
	$(CP) ./files/* $(1)/
endef

$(eval $(call BuildPackage,luci-app-arduino-webpanel))

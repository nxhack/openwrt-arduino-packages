# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=lua-base64
PKG_VERSION:=1.0.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/toastdriven/lua-base64.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=1254abbcb979865677695313b11fb3e56925e987
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=b16f18f6b5496ca3aa948d4e088a41125f3b3ec8074cfb2d226b884fecf73ac3

PKG_MAINTAINER:=Dirk Chang <dirk@kooiot.com>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/lua-base64
  SUBMENU:=Lua
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Lua Base64
  URL:=https://github.com/toastdriven/lua-base64
  DEPENDS:=+lua
endef

define Package/lua-base64/description
	Another base64 implementation for LUA
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/lua-base64/install
	$(INSTALL_DIR) $(1)/usr/lib/lua
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/base64.lua $(1)/usr/lib/lua
endef

$(eval $(call BuildPackage,lua-base64))

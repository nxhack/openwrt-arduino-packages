# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# Initial port of shellinabox to OpenWrt - Jan Jaeger 25-MAR-2010
#

include $(TOPDIR)/rules.mk

PKG_NAME:=shellinabox
PKG_VERSION:=2.20
PKG_RELEASE:=5

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/shellinabox/shellinabox.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=05b2d3630c48dc9bca678d8158fce486c583c26b
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=f0a0f7be823216a6df49a8830d1ca8329854d88195a9d666f76b09c0d07eda52

PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>
PKG_LICENSE:=GPL-2.0+
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/shellinabox
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Shell In A Box
  URL:=https://github.com/shellinabox/shellinabox
  DEPENDS:=+libopenssl +libpam +openssl-util +zlib
endef

define Package/shellinabox/description
 Web based AJAX terminal emulator
endef

TARGET_CFLAGS+= $(FPIC) \
	-DHAVE_PTSNAME_R=1

CONFIGURE_ARGS:= \
	--host=$(GNU_TARGET_NAME) \
	--disable-runtime-loading \
	--disable-utmp

CONFIGURE_VARS+= \
	config_BUILD_CC="$(HOSTCC)" \
	config_BUILD_CFLAGS="-O2" \
	config_TARGET_CC="$(TARGET_CC)" \
	config_TARGET_CFLAGS="$(TARGET_CFLAGS)"

define Package/shellinabox/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/shellinaboxd $(1)/usr/sbin/
	$(INSTALL_BIN) files/gen-cert.sh $(1)/usr/sbin/gen-cert

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) files/shellinabox-init.sh $(1)/etc/init.d/shellinabox-init
endef

define Package/shellinabox/prerm
#!/bin/sh
# check if we are on real system
if [ -z "$${IPKG_INSTROOT}" ]; then
        /etc/init.d/shellinabox-init stop 2> /dev/null
fi
exit 0
endef

$(eval $(call BuildPackage,shellinabox))

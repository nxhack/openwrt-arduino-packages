#
# Copyright (c) 2015 Arduino Srl. All right reserved.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ciao
PKG_VERSION:=0.1.0
PKG_RELEASE:=5

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/arduino-org/Ciao/tar.gz/$(PKG_VERSION)?
PKG_HASH:=33a02b2817203064f7250cc0c60ad7ccae101496136c5107fb94323627479765
PKG_BUILD_DIR:=$(BUILD_DIR)/Ciao-$(PKG_VERSION)

PKG_MAINTAINER:=
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/ciao
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Ciao
  TITLE:=The new Ciao CPU-MCU Bridge
  DEPENDS:=+python-light +python-paho-mqtt +python-openssl +python-pyserial +python-logging
endef

define Package/ciao/description
 The new Ciao Bridge lets linux and arduino communicate
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	find $(PKG_BUILD_DIR)/ciao/connectors -name "*.py" -exec chmod +x {} \;
endef

define Build/Compile
	# NOOP
endef

define Package/ciao/install
	$(INSTALL_DIR) $(1)/usr/lib/python2.7/ciao
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ciao/*.py $(1)/usr/lib/python2.7/ciao

	$(INSTALL_DIR) $(1)/usr/lib/python2.7/ciao/conf
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/ciao/conf/*.conf $(1)/usr/lib/python2.7/ciao/conf

	$(INSTALL_DIR) $(1)/usr/lib/python2.7/ciao/connectors
	$(CP) -r $(PKG_BUILD_DIR)/ciao/connectors/* $(1)/usr/lib/python2.7/ciao/connectors

	# Installing run-ciao script in /usr/bin
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) ./files/run-ciao $(1)/usr/bin

	# Installing ciaotools.py
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ciaotools.py $(1)/usr/lib/python2.7
endef

define Package/ciao/postinst
#!/bin/sh
# check if we are on a real system
if [ -z "$${IPKG_INSTROOT}" ]; then
  if [ -e /etc/sysctl.conf ]; then
          sed -i '/kernel.printk/d' /etc/sysctl.conf
          echo "kernel.printk = 3 4 1 3" >> /etc/sysctl.conf
          /sbin/sysctl -p 2>&1 /dev/null
  fi

  if [ -e /usr/lib/python2.7/asyncore.py ]; then
          sed -i -e s/512/8192/g /usr/lib/python2.7/asyncore.py
  fi
fi
exit 0
endef

define Package/ciao/postrm
#!/bin/sh
# check if we are on a real system
if [ -z "$${IPKG_INSTROOT}" ]; then
  if [ -e /etc/sysctl.conf ]; then
          sed -i '/kernel.printk/d' /etc/sysctl.conf
          /sbin/sysctl -p 2>&1 /dev/null
  fi
fi
exit 0
endef

$(eval $(call BuildPackage,ciao))

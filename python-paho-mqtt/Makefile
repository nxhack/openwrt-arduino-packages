# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=paho-mqtt
PKG_VERSION:=1.3.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://pypi.python.org/packages/2a/5f/cf14b8f9f8ed1891cda893a2a7d1d6fa23de2a9fb4832f05cef02b79d01f
PKG_HASH:=31911f6031de306c27ed79dc77b690d7c55b0dcb0f0434ca34ec6361d0371122

PKG_LICENSE:=EPL-1.0
PKG_LICENSE_FILES:=LICENSE.txt
PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(BUILD_VARIANT)-$(PKG_NAME)-$(PKG_VERSION)

PYTHON_MK:=$(TOPDIR)/feeds/packages/lang/python/python-package.mk
ifneq ("$(wildcard $(PYTHON_MK))","")
  IS_HEAD:=y
endif

ifneq ($(IS_HEAD),y)
PKG_BUILD_DEPENDS:=PACKAGE_python-paho-mqtt:python \
                   PACKAGE_python-paho-mqtt:python-setuptools \
                   PACKAGE_python3-paho-mqtt:python \
                   PACKAGE_python3-paho-mqtt:python3-setuptools
endif

include $(INCLUDE_DIR)/package.mk
ifeq ($(IS_HEAD),y)
include $(TOPDIR)/feeds/packages/lang/python/python-package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-package.mk
else
$(call include_mk, python-package.mk)
$(call include_mk, python3-package.mk)
endif

PKG_UNPACK:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components=1 -xzf $(DL_DIR)/$(PKG_SOURCE)

define Package/python-paho-mqtt/Default
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Python
  URL:=http://www.eclipse.org/paho/
  DEPENDS:=+libopenssl
endef

define Package/python-paho-mqtt
  $(call Package/python-paho-mqtt/Default)
  TITLE:=python-paho-mqtt
  DEPENDS+=+PACKAGE_python-paho-mqtt:python-light \
           +PACKAGE_python-paho-mqtt:python-xml \
           +PACKAGE_python-paho-mqtt:python-openssl
  VARIANT:=python
endef

define Package/python3-paho-mqtt
  $(call Package/python-paho-mqtt/Default)
  TITLE:=python3-paho-mqtt
  DEPENDS+=+PACKAGE_python3-paho-mqtt:python3-light \
           +PACKAGE_python3-paho-mqtt:python3-xml \
           +PACKAGE_python3-paho-mqtt:python3-openssl
  VARIANT:=python3
endef

define Package/python-paho-mqtt/description
MQTT version 3.1.1 client class
endef

define Package/python3-paho-mqtt/description
$(call Package/python-paho-mqtt/description)
.
(Variant for Python3)
endef

$(eval $(call PyPackage,python-paho-mqtt))
$(eval $(call BuildPackage,python-paho-mqtt))
#$(eval $(call BuildPackage,python-paho-mqtt-src))
$(eval $(call Py3Package,python3-paho-mqtt))
$(eval $(call BuildPackage,python3-paho-mqtt))
#$(eval $(call BuildPackage,python3-paho-mqtt-src))

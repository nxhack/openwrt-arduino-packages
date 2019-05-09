# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=distribute
PKG_VERSION:=0.7.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).zip
PKG_SOURCE_URL:=https://files.pythonhosted.org/packages/5f/ad/1fde06877a8d7d5c9b60eff7de2d452f639916ae1d48f0b8f97bf97e570a
PKG_HASH:=3dc7a8d059dcf72f0ead2fa2144a24ee0ef07dce816e8c3545d7345767138c5e

PKG_LICENSE:=PSF or ZPL
PKG_LICENSE_FILES:=
PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(BUILD_VARIANT)-$(PKG_NAME)-$(PKG_VERSION)

OW_VER:=$(shell git --git-dir=$(TOPDIR)/.git describe --abbrev=0 --always --tag)
ifneq ($(filter reboot v18.06%, $(OW_VER)),)
  IS_HEAD:=y
endif

ifneq ($(IS_HEAD),y)
PKG_BUILD_DEPENDS:=PACKAGE_python-distribute:python \
                   PACKAGE_python-distribute:python-setuptools \
                   PACKAGE_python3-distribute:python3 \
                   PACKAGE_python3-distribute:python3-setuptools
endif

include $(INCLUDE_DIR)/package.mk
ifeq ($(IS_HEAD),y)
include $(TOPDIR)/feeds/packages/lang/python/python-package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-package.mk
else
$(call include_mk, python-package.mk)
$(call include_mk, python3-package.mk)
endif

define Package/python-distribute/Default
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Python
  URL:=https://pypi.org/project/distribute/
endef

define Package/python-distribute
  $(call Package/python-distribute/Default)
  TITLE:=python-distribute
  DEPENDS:=PACKAGE_python-distribute:python-light
  VARIANT:=python
endef

define Package/python3-distribute
  $(call Package/python-distribute/Default)
  TITLE:=python3-distribute
  DEPENDS:=+PACKAGE_python3-distribute:python3-light
  VARIANT:=python3
endef

define Package/python-distribute/description
distribute legacy wrapper
endef

define Package/python3-distribute/description
$(call Package/python-distribute/description)
.
(Variant for Python3)
endef

$(eval $(call PyPackage,python-distribute))
$(eval $(call BuildPackage,python-distribute))
#$(eval $(call BuildPackage,python-distribute-src))
$(eval $(call Py3Package,python3-distribute))
$(eval $(call BuildPackage,python3-distribute))
#$(eval $(call BuildPackage,python3-distribute-src))

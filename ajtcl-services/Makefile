include $(TOPDIR)/rules.mk

PKG_NAME:=ajtcl-services
PKG_VERSION:=15.04
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/alljoyn/services-base_tcl.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=1cee32aa4d545eec8b80a6438b271f873d354d05
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=83ad0a9002c052974b1acc559c318284e6a926a80790ba0abd8a8809e4acfabd

PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_DEBUG

include $(INCLUDE_DIR)/package.mk

define Package/ajtcl-services/Default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=ajtcl
  TITLE:=AllJoyn Thin Client Base Services library
  URL:=https://github.com/alljoyn/
endef

define Package/ajtcl-services
$(call Package/ajtcl-services/Default)
  TITLE:=AllJoyn Thin Client - Base Services
  MENU:=0
endef

define Package/ajtcl-services/description
	AllJoyn Thin Client Base Services is the standard AllJoyn services libraries for
	the AllJoyn Thin Client C library for developing ad hoc peer-to-peer
	applications that use the AllJoyn protocol. Revision : 7b875c76cb29304f955c5b5a4bafb2194c6f779e 
endef

#define Package/ajtcl-services-samples
#$(call Package/ajtcl-services/Default)
#  TITLE:=AllJoyn Thin Client - Base Services - testing samples
#  DEPENDS:=ajtcl
#endef

#define Package/ajtcl-services-samples/description
#Alljoyn Thin Client Base Services samples.  (This is primarily for testing.)
#endef

ifeq ($(CONFIG_DEBUG),y)
  ALLJOYN_BUILD_VARIANT:=debug
else
  ALLJOYN_BUILD_VARIANT:=release
endif

define Build/Configure/Default
# Override default to do nothing
endef

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include/ajtcl

define Build/Compile
	TARGET_CC_NOCACHE="$(TARGET_CC_NOCACHE)" \
	TARGET_CXX_NOCACHE="$(TARGET_CXX_NOCACHE)" \
	TARGET_LINK="$(TARGET_CC_NOCACHE)" \
	STAGING_DIR="$(STAGING_DIR)" \
	TARGET_PATH="$(TARGET_PATH)" \
	TARGET_CFLAGS="$(TARGET_CFLAGS)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	TARGET_CPPFLAGS="$(TARGET_CPPFLAGS)" \
	TARGET_LINKFLAGS="$(TARGET_LINKFLAGS)" \
	TARGET_LDFLAGS="$(TARGET_LDFLAGS)" \
	TARGET_PATH="$(PATH)" \
	scons -C $(PKG_BUILD_DIR) \
		-f SConstruct.openwrt \
		$(PKG_JOBS) \
		"VARIANT=$(ALLJOYN_BUILD_VARIANT)"
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/include/ajtcl
	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn

	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn/config
	$(CP) $(PKG_BUILD_DIR)/config/inc/alljoyn/config/*.h $(1)/usr/include/ajtcl/alljoyn/config

	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn/controlpanel
	$(CP) $(PKG_BUILD_DIR)/controlpanel/inc/alljoyn/controlpanel/*.h $(1)/usr/include/ajtcl/alljoyn/controlpanel
	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn/controlpanel/Common
	$(CP) $(PKG_BUILD_DIR)/controlpanel/inc/alljoyn/controlpanel/Common/*.h $(1)/usr/include/ajtcl/alljoyn/controlpanel/Common
	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn/controlpanel/Widgets
	$(CP) $(PKG_BUILD_DIR)/controlpanel/inc/alljoyn/controlpanel/Widgets/*.h $(1)/usr/include/ajtcl/alljoyn/controlpanel/Widgets

	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn/notification
	$(CP) $(PKG_BUILD_DIR)/notification/inc/alljoyn/notification/*.h $(1)/usr/include/ajtcl/alljoyn/notification

	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn/onboarding
	$(CP) $(PKG_BUILD_DIR)/onboarding/inc/alljoyn/onboarding/*.h $(1)/usr/include/ajtcl/alljoyn/onboarding

	$(INSTALL_DIR) $(1)/usr/include/ajtcl/alljoyn/services_common
	$(CP) $(PKG_BUILD_DIR)/services_common/inc/alljoyn/services_common/*.h $(1)/usr/include/ajtcl/alljoyn/services_common

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libajtcl_services.a $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libajtcl_services.so $(1)/usr/lib/
endef

define Package/ajtcl-services/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libajtcl_services.so $(1)/usr/lib/
endef

#define Package/ajtcl-services-samples/install
#	$(INSTALL_DIR) $(1)/usr/bin
#endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,ajtcl-services))
#$(eval $(call BuildPackage,ajtcl-services-samples))

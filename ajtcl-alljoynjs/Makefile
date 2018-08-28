include $(TOPDIR)/rules.mk

PKG_NAME:=ajtcl-alljoynjs
PKG_VERSION:=15.04
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/alljoyn/core-alljoyn-js.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=57e7f26fad727bb41015fff46f7d96fe261dc38c
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=eac178b794cc75d0b8243884d09dbc9c5c8e89934f2d1d8c1ee1fbcff1cd7252

PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_DEBUG

include $(INCLUDE_DIR)/package.mk

define Package/ajtcl-alljoynjs
  TITLE:=AllJoyn Thin Client - AllJoynJS
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=ajtcl +ajtcl-services +duktape
  URL:=https://github.com/alljoyn/
  MENU:=1
endef

define Package/ajtcl-alljoynjs/description
	AllJoyn Thin Client AllJoynJS binding is a basic, low-level JavaScript
	interpreter that uses the Duktape embeddable JavaScript engine for developing
	ad hoc peer-to-peer applications that use the AllJoyn protocol. MASTER Revision
endef

define Package/ajtcl-alljoynjs-samples
$(call Package/ajtcl-alljoynjs)
  TITLE+=- testing samples
  DEPENDS:=ajtcl-alljoynjs
  MENU:=0
endef

define Package/ajtcl-alljoynjs-samples/description
	Sample JavaScript programs that use AllJoyn via alljoyn.js.
endef

ifeq ($(CONFIG_DEBUG),y)
  ALLJOYN_BUILD_VARIANT:=debug
else
  ALLJOYN_BUILD_VARIANT:=release
endif

define Build/Configure/Default
	# Override default to do nothing
endef

define Build/Compile
	YUN_BUILD=0 \
	CROSS_CFLAGS="-I$(STAGING_DIR)/usr/src/duktape/src -I$(STAGING_DIR)/usr/include/ajtcl $(TARGET_CFLAGS) $(EXTRA_CFLAGS) $(TARGET_CPPFLAGS)" \
	CROSS_LINKFLAGS="$(TARGET_LINKFLAGS) $(TARGET_LDFLAGS) -lajtcl_services" \
	CROSS_PATH=$(TARGET_PATH) \
	CROSS_PREFIX=$(REAL_GNU_TARGET_NAME)- \
	DUKTAPE_DIST="$(STAGING_DIR)/usr/src/duktape" \
	scons -C $(PKG_BUILD_DIR) \
		$(PKG_JOBS) \
		WS=off \
		TARG=linux \
		"VARIANT=$(ALLJOYN_BUILD_VARIANT)"
endef

define Package/ajtcl-alljoynjs/install
	$(INSTALL_DIR) $(1)/usr
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/alljoynjs $(1)/usr/bin/
endef

define Package/ajtcl-alljoynjs-samples/install
	$(INSTALL_DIR) $(1)/usr
	$(INSTALL_DIR) $(1)/usr/share
	$(INSTALL_DIR) $(1)/usr/share/alljoyn.js
	$(CP) $(PKG_BUILD_DIR)/js/*.js $(1)/usr/share/alljoyn.js/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,ajtcl-alljoynjs))
$(eval $(call BuildPackage,ajtcl-alljoynjs-samples))

include $(TOPDIR)/rules.mk

PKG_NAME:=ajtcl
PKG_VERSION:=15.04b
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/alljoyn/core-ajtcl.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=7d7871a950985a03979c8e22df258c049ae12b20
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=11ce6931e239ad36f54664279e1b2a91c15208527179483f1136177337b035ee

PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_DEBUG

include $(INCLUDE_DIR)/package.mk

define Package/ajtcl/Default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libopenssl +libpthread +librt
  URL:=https://github.com/alljoyn/
  TITLE:=The AllJoyn Thin Client Library
endef

define Package/ajtcl
$(call Package/ajtcl/Default)
  TITLE:=AllJoyn Thin Client Library
  MENU:=1
endef

define Package/ajtcl/description
	AllJoyn Thin Client Library is a basic, low-level C library for developing
	ad hoc peer-to-peer applications that use the AllJoyn protocol. Revision : c7246570b10c70913c2b983b5f9353672908f73d 
endef

define Package/ajtcl-samples
$(call Package/ajtcl/Default)
  TITLE:=AllJoyn Thin Client Library - testing samples
  DEPENDS:=ajtcl
endef

define Package/ajtcl-samples/description
	Alljoyn Thin Client Library samples.  (This is primarily for testing.)
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
	CROSS_CFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS) $(FPIC) $(TARGET_CPPFLAGS) -DOPENWRT_MIPS" \
	CROSS_LINKFLAGS="$(TARGET_LINKFLAGS) $(TARGET_LDFLAGS)" \
	CROSS_PATH=$(TARGET_PATH) \
	CROSS_PREFIX=$(TARGET_CC_NOCACHE:gcc=) \
	scons -C $(PKG_BUILD_DIR) \
		$(PKG_JOBS) \
		WS=off \
		TARG=linux \
		"VARIANT=$(ALLJOYN_BUILD_VARIANT)"
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/include/ajtcl
	$(CP) $(PKG_BUILD_DIR)/inc/*.h $(1)/usr/include/ajtcl/
	$(CP) $(PKG_BUILD_DIR)/target/linux/*.h $(1)/usr/include/ajtcl/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libajtcl_st.a $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libajtcl.so $(1)/usr/lib/
endef

define Package/ajtcl/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libajtcl.so $(1)/usr/lib/
endef

define Package/ajtcl-samples/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/samples/secure/SecureClient $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/samples/secure/SecureService $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/samples/basic/basic_service $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/samples/basic/basic_client $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/samples/basic/signal_service $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/samples/basic/nameChange_client $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/samples/basic/signalConsumer_client $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/mutter $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/ajlite $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/aestest $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/aesbench $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/svclite $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/clientlite $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/siglite $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/sessions $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/nvramtest $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/bastress2 $(1)/usr/bin/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,ajtcl))
$(eval $(call BuildPackage,ajtcl-samples))

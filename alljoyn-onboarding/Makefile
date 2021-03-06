AJ_SERVICE:=onboarding
AJ_SERVICE_NAME:=onboarding

include $(TOPDIR)/rules.mk

PKG_NAME:=alljoyn-$(AJ_SERVICE)
PKG_BASE_VERSION:=16.10
PKG_PATCHLEVEL:=.02
PKG_VERSION:=$(PKG_BASE_VERSION)$(PKG_PATCHLEVEL)
PKG_TARBALL_VERSION:=$(PKG_VERSION)
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/alljoyn/services-base.git
PKG_SOURCE_SUBDIR:=alljoyn-services-$(PKG_TARBALL_VERSION)-src
PKG_SOURCE_VERSION:=28b5c7e9b731d4e397d187cdb0ea957170b92a27
PKG_SOURCE:=alljoyn-base-$(PKG_TARBALL_VERSION).tar.xz
PKG_MIRROR_HASH:=75a0195c509d4cfd00d62e31aa483bbc7bc2eace8c54d5b09ca1d69beb74e3d3

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

PKG_IREMAP:=0
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

ifneq ($(CONFIG_CCACHE),)
  TARGET_CC=$(TARGET_CC_NOCACHE)
  TARGET_CXX=$(TARGET_CXX_NOCACHE)
endif

ALLJOYN_BINDINGS:=cpp,

# Due to inherited dependencies (onboarding package depends on wpa-cli package and therefore inherits its dependencies)
# the onboarding package will not be visible for selection unless the wpa-supplicant package is first selected
define Package/$(PKG_NAME)
  TITLE:=AllJoyn $(AJ_SERVICE_NAME) service library
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=alljoyn \
		wpa-supplicant \
		+alljoyn-about \
		+alljoyn-config \
		+alljoyn-services_common \
		+hostapd \
		+wpa-cli
  URL:=https://github.com/alljoyn/alljoyn.github.com/wiki
endef

define Package/$(PKG_NAME)/description
AllJoyn $(AJ_SERVICE_NAME) service.
endef

ifeq ($(CONFIG_DEBUG),y)
  ALLJOYN_BUILD_VARIANT:=debug
else
  ALLJOYN_BUILD_VARIANT:=release
endif

ALLJOYN_DISTDIR:=$(BUILD_DIR)/alljoyn-$(PKG_VERSION)-src/build/openwrt/openwrt/$(ALLJOYN_BUILD_VARIANT)/dist
PKG_INSTALL_DIR:=$(PKG_BUILD_DIR)/$(AJ_SERVICE)/build/openwrt/openwrt/$(ALLJOYN_BUILD_VARIANT)/dist/$(AJ_SERVICE)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)
TARGET_LDFLAGS+=-lpthread -lrt

define Build/Configure/Default
# Override default to do nothing
endef

ALLJOYN_BUILD_SERVICES_SAMPLES:=off

define Build/Compile
	scons -C $(PKG_BUILD_DIR)/$(AJ_SERVICE) \
		$(PKG_JOBS) \
		WS=off \
		CPU=openwrt \
		OS=openwrt \
		"BINDINGS=$(ALLJOYN_BINDINGS)" \
		"BUILD_SERVICES_SAMPLES=$(ALLJOYN_BUILD_SERVICES_SAMPLES)" \
		"VARIANT=$(ALLJOYN_BUILD_VARIANT)" \
		BR=off \
		BT=off \
		ICE=off \
		"TARGET_CC=$$(TARGET_CC)" \
		"TARGET_CXX=$$(TARGET_CXX)" \
		"TARGET_CFLAGS=$$(TARGET_CFLAGS)" \
		"TARGET_CPPFLAGS=$$(TARGET_CPPFLAGS)" \
		"TARGET_PATH=$$(TARGET_PATH)" \
		"TARGET_LINKFLAGS=$$(TARGET_LDFLAGS)" \
		"TARGET_LINK=$$(TARGET_CC)" \
		"TARGET_AR=$$(TARGET_AR)" \
		"TARGET_RANLIB=$$(TARGET_RANLIB)" \
		"STAGING_DIR=$$(STAGING_DIR)" \
		"ALLJOYN_DISTDIR=$$(ALLJOYN_DISTDIR)"
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/alljoyn/$(AJ_SERVICE)
	$(CP) $(PKG_INSTALL_DIR)/inc/alljoyn/$(AJ_SERVICE)/*.h $(1)/usr/include/alljoyn/$(AJ_SERVICE)
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/lib/liballjoyn_$(AJ_SERVICE).a $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/lib/liballjoyn_$(AJ_SERVICE).so $(1)/usr/lib/
endef


define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/lib/liballjoyn_$(AJ_SERVICE).so $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/onboarding-daemon $(1)/usr/bin/
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/OnboardingClient $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/alljoyn-onboarding.init $(1)/etc/init.d/alljoyn-onboarding
	$(INSTALL_BIN) ./files/alljoyn-onboarding-reload.init $(1)/etc/init.d/alljoyn-onboarding-reload
	$(INSTALL_BIN) ./files/wifi_scan.init $(1)/etc/init.d/wifi_scan
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/alljoyn-onboarding.defaults $(1)/etc/uci-defaults/alljoyn-onboarding
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/alljoyn-onboarding.config $(1)/etc/config/alljoyn-onboarding
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) ./files/wifi_scan $(1)/usr/sbin/wifi_scan
	$(INSTALL_BIN) ./files/parse_iw_wifi_scan $(1)/usr/sbin/parse_iw_wifi_scan
	$(INSTALL_BIN) ./files/parse_iwlist_wifi_scan $(1)/usr/sbin/parse_iwlist_wifi_scan
	$(INSTALL_BIN) ./files/wifi_mgmt $(1)/usr/sbin/wifi_mgmt
	$(INSTALL_DIR) $(1)/etc/alljoyn-onboarding
	$(INSTALL_CONF) ./files/alljoyn-onboarding.state $(1)/etc/alljoyn-onboarding/alljoyn-onboarding-state
	$(CP) $(PKG_BUILD_DIR)/onboarding/cpp/src/OnboardingDaemon/OnboardingService.conf $(1)/etc/alljoyn-onboarding/
	$(CP) $(PKG_BUILD_DIR)/onboarding/cpp/src/OnboardingDaemon/FactoryOnboardingService.conf $(1)/etc/alljoyn-onboarding/
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
    . /etc/uci-defaults/alljoyn-onboarding
    rm -f /etc/uci-defaults/alljoyn-onboarding
fi
exit 0
endef

define Package/$(PKG_NAME)/prerm
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
    /etc/init.d/alljoyn-onboarding stop
    /etc/init.d/alljoyn-onboarding disable
    /etc/init.d/wifi_scan disable
    /etc/init.d/alljoyn-onboarding-reload disable
    crontab -l 2>/dev/null | sed '/\/etc\/init.d\/alljoyn-onboarding/d' | crontab -
    crontab -l 2>/dev/null | sed '/\/usr\/sbin\/wifi_scan/d' | crontab -
    local obswifiIndex=$(uci show /etc/config/firewall | grep obswifi | grep name | cut -d"[" -f2 | cut -c1)
    uci delete firewall.@zone[$(obswifiIndex)]
    uci commit firewall
    uci batch <<EOF
del_list ucitrack.@alljoyn[-1].affects=alljoyn-onboarding
delete ucitrack.@alljoyn-onboarding[-1]
commit ucitrack
EOF
fi
exit 0
endef

$(eval $(call BuildPackage,$(PKG_NAME)))

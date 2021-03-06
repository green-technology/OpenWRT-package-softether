# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=softethervpn
PKG_VERSION:=4.19
PKG_RELEASE:=9605
#PKG_VERSION2:=rtm
PKG_VERSION2:=beta
PKG_DATE:=2016.3.15

PKG_BUILD_DIR:=$(BUILD_DIR)/v$(PKG_VERSION)-$(PKG_RELEASE)
#PKG_SOURCE:=softether-src-v$(PKG_VERSION)-$(PKG_RELEASE)-$(PKG_VERSION2).tar.gz
#PKG_SOURCE_URL:=http://jp.softether-download.com/files/softether/v$(PKG_VERSION)-$(PKG_RELEASE)-$(PKG_VERSION2)-$(PKG_DATE)-tree/Source_Code/
PKG_SOURCE:=softether-src-v4.19-9605-beta.zip
PKG_SOURCE_URL:=https://chuangdian230/svn/browser_platform_code/vpn/code/branches/openwrt/tag/
PKG_MD5SUM:=5d4a8401c37a99cb20cb2ec4e54fa93f
PKG_CAT:=unzip

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/softethervpn
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	TITLE:=Open-Source Free Cross-platform Multi-protocol VPN
	URL:=http://www.softether.org/
	DEPENDS:=+libpthread +librt +libreadline +libopenssl +libncurses +kmod-tun +glib2
	
endef

#define Package/softethervpnclient
#	$(call Package/softethervpnserver)
#endef
#
#define Package/softethervpnbridge
#	$(call Package/softethervpnserver)
#endef
#
#define Package/softethervpncmd
#	$(call Package/softethervpnserver)
#endef

ifeq ($(ARCH),mips)
	SE4WRT_OPTIONS := -minterlink-mips16
endif
ifeq ($(ARCH),mipsel)
	SE4WRT_OPTIONS := -minterlink-mips16
endif

define Build/Configure
endef

define Build/Compile
	make \
		-C $(PKG_BUILD_DIR) \
		-f src/makefiles/linux_32bit.mak \
		clean

	make \
		-C $(PKG_BUILD_DIR) \
		-f src/makefiles/linux_32bit.mak \
		src/bin/BuiltHamcoreFiles/unix/hamcore.se2
	mv $(PKG_BUILD_DIR)/src/bin/BuiltHamcoreFiles/unix/hamcore.se2 $(PKG_BUILD_DIR)/src/bin/BuiltHamcoreFiles/unix/hamcore.se2.1

	make \
		-C $(PKG_BUILD_DIR) \
		-f src/makefiles/linux_32bit.mak \
		clean
	mv $(PKG_BUILD_DIR)/src/bin/BuiltHamcoreFiles/unix/hamcore.se2.1 $(PKG_BUILD_DIR)/src/bin/BuiltHamcoreFiles/unix/hamcore.se2
	#touch -d "`date -d 1day`" $(PKG_BUILD_DIR)/tmp/hamcorebuilder
	touch -d "`date -d 1day`" $(PKG_BUILD_DIR)/src/bin/BuiltHamcoreFiles/unix/hamcore.se2

	$(MAKE) \
		$(TARGET_CONFIGURE_OPTS) \
		CCFLAGS="-I$(STAGING_DIR)/usr/include $(ICONV_CFLAGS) $(SE4WRT_OPTIONS)" \
		LDFLAGS="-L$(STAGING_DIR)/usr/lib $(ICONV_LDFLAGS) -liconv" \
		-C $(PKG_BUILD_DIR) \
		-f src/makefiles/linux_32bit.mak
endef

#define Package/softethervpnserver/install
define Package/softethervpn/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnserver/* $(1)/usr/bin
	ln -s vpnserver $(1)/usr/bin/vpnclient
	ln -s vpnserver $(1)/usr/bin/vpnbridge
	ln -s vpnserver $(1)/usr/bin/vpncmd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/rc/vpnserver $(1)/etc/init.d/softethervpnserver
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/rc/vpnclient $(1)/etc/init.d/softethervpnclient
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/rc/vpnbridge $(1)/etc/init.d/softethervpnbridge
endef

#define Package/softethervpnclient/install
#	$(INSTALL_DIR) $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnclient/* $(1)/usr/bin
#endef
#
#define Package/softethervpnbridge/install
#	$(INSTALL_DIR) $(1)/usr/bin
#	$(INSTALL_DIR) $(1)/etc/init.d
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnbridge/* $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/rc/vpnbridge $(1)/etc/init.d/softethervpnbridge
#endef
#
#define Package/softethervpncmd/install
#	$(INSTALL_DIR) $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpncmd/* $(1)/usr/bin
#endef

$(eval $(call BuildPackage,softethervpn))
#$(eval $(call BuildPackage,softethervpnserver))
#$(eval $(call BuildPackage,softethervpnclient))
#$(eval $(call BuildPackage,softethervpnbridge))
#$(eval $(call BuildPackage,softethervpncmd))

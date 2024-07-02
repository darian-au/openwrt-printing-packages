#
# Copyright (C) 2008-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=fontconfig
PKG_VERSION:=2.15.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://www.freedesktop.org/software/fontconfig/release/
PKG_MD5SUM:=5bb3a2829aecb22ae553c39099bd0d6a

PKG_BUILD_DEPENDS:=gperf/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/meson.mk

define Package/fontconfig
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Fontconfig, configure and customize font access
  DEPENDS:=+libfreetype +libexpat +libpthread
  URL:=https://www.freedesktop.org/wiki/Software/fontconfig/
  SUBMENU:=Printing
endef

define Package/fontconfig/description
	Fontconfig is a library for configuring and customizing font access.
endef

MESON_ARGS += \
	-Ddoc=disabled \
	-Ddoc-txt=disabled \
	-Dnls=disabled \
	-Dtests=disabled \
	-Dtools=enabled

define Package/fontconfig/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/usr/lib $(1)/usr/share/fontconfig/conf.avail
	$(INSTALL_DIR) $(1)/etc/fonts/conf.d
	$(CP) $(PKG_INSTALL_DIR)/etc/fonts/fonts.conf $(1)/etc/fonts
	$(CP) $(PKG_INSTALL_DIR)/etc/fonts/conf.d/* $(1)/etc/fonts/conf.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/fontconfig/conf.avail/* $(1)/usr/share/fontconfig/conf.avail
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/fontconfig.init $(1)/etc/init.d/fontconfig
endef

define Package/fontconfig/postinst
#!/bin/sh
# check if we are on a real system
if [ -z "$${IPKG_INSTROOT}" ]; then
   echo "Enabling rc.d symlink for fontconfig cache rebuild"
   /etc/init.d/fontconfig enable
   /usr/bin/fc-cache --force --verbose
fi
exit 0
endef

define Package/fontconfig/prerm
#!/bin/sh
# check if we are on a real system
if [ -z "$${IPKG_INSTROOT}" ]; then
   echo "Removing rc.d symlink for fontconfig"
   /etc/init.d/fontconfig disable
fi
exit 0
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/fontconfig $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libfontconfig.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/fontconfig.pc $(1)/usr/lib/pkgconfig/
endef

$(eval $(call BuildPackage,fontconfig))

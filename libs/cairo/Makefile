#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cairo
PKG_VERSION:=1.18.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://www.cairographics.org/releases
PKG_MD5SUM:=3f0685fbadc530606f965b9645bb51d9

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/meson.mk
include $(INCLUDE_DIR)/nls.mk

define Package/libcairo
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Cairo 2D graphics library
  DEPENDS:=+pixman +fontconfig +libfreetype +libpng +glib2
  URL:=https://gitlab.freedesktop.org/cairo/cairo/
  SUBMENU:=Printing
endef

define Package/libcairo/description
	Cairo is a 2D graphics library with support for multiple output devices. 
	Currently supported output targets include the X Window System (via 
	both Xlib and XCB), Quartz, Win32, image buffers, PostScript, PDF, and 
	SVG file output.
endef

define Package/libcairo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib
endef

$(eval $(call BuildPackage,libcairo))

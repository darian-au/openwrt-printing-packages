#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=openprinting-cups-filters
PKG_VERSION:=1.28.17
PKG_RELEASE:=1

PKG_SOURCE:=cups-filters-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://github.com/OpenPrinting/cups-filters/releases/download/$(PKG_VERSION)
PKG_MD5SUM:=74741eb5ba32331676f88be468259d1f
PKG_BUILD_DIR:=$(BUILD_DIR)/cups-filters-$(PKG_VERSION)

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

EXTRA_CFLAGS+= \
	-DHAVE_CPP_POPPLER_VERSION_H

CONFIGURE_VARS+= \
	LIBQPDF_CFLAGS='-I$(PKG_INSTALL_DIR)/usr/include' \
	LIBQPDF_LIBS='-L$(PKG_INSTALL_DIR)/usr/lib -lqpdf' \
	POPPLER_CFLAGS='-I$(PKG_INSTALL_DIR)/usr/include/poppler/cpp -I$(PKG_INSTALL_DIR)/usr/include/poppler' \
	POPPLER_LIBS='-L$(PKG_INSTALL_DIR)/usr/lib -lpoppler-cpp'

define Package/openprinting-cups-filters
  SECTION:=Net
  CATEGORY:=Network
  TITLE:=OpenPrinting CUPS filters
  DEPENDS:=+cups +libcupsimage +fontconfig +libfreetype +poppler +qpdf +lcms2 +libijs +libtiff +libjpeg +libpng +glib2 +zlib +libexif
  URL:=http://www.openprinting.org
  SUBMENU:=Printing
endef

define Package/openprinting-cups-filters/description
	CUPS filters maintained by OpenPrinting.
	The CUPS Filters package contains backends, filters and other software that was once part of the core CUPS distribution but is no longer maintained by Apple Inc.
endef

define Build/Configure
	$(SED) 's,pdftopdf_CXXFLAGS = -std=c++0x,pdftopdf_CXXFLAGS = -std=c++17,g' $(PKG_BUILD_DIR)/Makefile.am
	$(call Build/Configure/Default, \
	--enable-imagefilters \
	--with-pdftops=gs \
	--with-gs-path=/usr/bin/gs \
	--with-pdftops-path=/usr/bin/gs \
	--disable-gnutls \
	--disable-openssl \
	--disable-cdsassl \
	--disable-ssl \
	--disable-gssapi \
	--disable-avahi \
	)
endef

define Package/openprinting-cups-filters/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/usr/lib/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
	$(CP) -r $(PKG_INSTALL_DIR)/usr/lib/cups $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/share/cups $(1)/usr/share/ppd/cupsfilters
	$(CP) -r $(PKG_INSTALL_DIR)/usr/share/cups/* $(1)/usr/share/cups
	$(CP) -r $(PKG_INSTALL_DIR)/usr/share/ppd/cupsfilters/* $(1)/usr/share/ppd/cupsfilters
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib
endef

define Package/preinst
#!/bin/sh
# Be on the safe side: get rid of any files from CUPS 1.5.X filters.
# See OpenPrinting's cups-filters-1.0.X/INSTALL
rm /usr/lib/cups/backend/serial
rm /usr/lib/cups/backend/parallel
rm /usr/lib/cups/filter/bannertops
rm /usr/lib/cups/filter/commandtoescpx
rm /usr/lib/cups/filter/commandtopclx
rm /usr/lib/cups/filter/imagetops
rm /usr/lib/cups/filter/imagetoraster
rm /usr/lib/cups/filter/pdftops
rm /usr/lib/cups/filter/rastertoescpx
rm /usr/lib/cups/filter/rastertopclx
rm /usr/lib/cups/filter/texttops
rm /usr/share/cups/banners/*
rm /usr/share/cups/data/testprint
rm /usr/share/cups/data/psglyphs
rm /usr/share/cups/fonts/*
endef

define Package/postinst
#!/bin/sh
/etc/init.d/cupsd stop
/etc/init.d/cupsd start
endef

$(eval $(call BuildPackage,openprinting-cups-filters))

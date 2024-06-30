include $(TOPDIR)/rules.mk

PKG_NAME:=splix
PKG_VERSION:=2.0.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/OpenPrinting/splix/archive/refs/tags
PKG_MD5SUM:=acfee35b2bd183411eff30e9305fe6ac

include $(INCLUDE_DIR)/package.mk

define Package/splix
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Printing
  TITLE:=SPL printer driver for Unix
  DEPENDS:=+cups +libcupsimage +libtiff +libjbigkit
  URL:=https://github.com/OpenPrinting/splix
endef

define Package/splix/description
	Splix is a driver for printers that speak SPL (Samsung Printer Language). This includes printers made by Samsung, Dell, and Xerox
endef

define Build/Configure
	$(SED) 's/(const PPDFile::Value::Value/(const PPDFile::Value/' $(PKG_BUILD_DIR)/src/ppdfile.cpp
	$(SED) "s/^CC\t\t:= gcc/CC\t\t:= $(TARGET_CC)/" -e "s/^CXX\t\t:= g++/CXX\t\t:= $(TARGET_CXX)/" -e "s/^AR\t\t:= ar/AR\t\t:= $(TARGET_AR)/" $(PKG_BUILD_DIR)/Makefile
	$(SED) 's|\(.*\)toqpdl_LDFLAGS\t*:=.*|\1toqpdl_LDFLAGS\t:=$(TARGET_LDFLAGS) -lpthread|' -e 's|\(.*\)toqpdl_LIBS\t*:=.*|\1toqpdl_LIBS\t:=-lpng -ltiff -lcups -lcupsimage|' -e 's|^CXXFLAGS\t\t+= `cups.*|CXXFLAGS\t\t+= $(TARGET_CFLAGS) -Iinclude -Wall $(TARGET_CPPFLAGS)|' -e 's|CUPSFILTER\t*:=.*|CUPSFILTER\t:= /usr/lib/cups/filter|' -e 's|CUPSPPD\t*?=.*|CUPSPPD\t?= /usr/share/cups/model|' -e 's|CUPSDRV\t*?=.*|CUPSDRV\t?= /usr/share/cups/driver|' -e 's|CUPSPROFILE\t*:=.*|CUPSPROFILE\t:= /usr/share/cups/profiles|' $(PKG_BUILD_DIR)/module.mk
	$(SED) 's|g++|$(TARGET_CXX)|' $(PKG_BUILD_DIR)/rules.mk
	mv $(PKG_BUILD_DIR)/*.ppd $(PKG_BUILD_DIR)/ppd/
endef

define Package/splix/install
	$(INSTALL_DIR) $(1)/usr/lib/cups/driver
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/optimized/{pstoqpdl,rastertoqpdl} $(1)/usr/lib/cups/driver
endef

$(eval $(call BuildPackage,splix))

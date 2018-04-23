# $Id: Makefile 973 2005-05-19 21:22:09Z nico $

include $(TOPDIR)/rules.mk

PKG_NAME:=tclthread
PKG_VERSION:=2.8.2
PKG_RELEASE:=2
PKG_MD5SUM:=38d301865285738d3d7f43fff0403805
# 2.7.2
#PKG_MD5SUM:=e025dbfa2dcd13e5feb4d06f6a00b2b7

PKG_SOURCE_URL:=@SF/tcl
PKG_SOURCE:=thread$(PKG_VERSION).tar.gz

PKG_BUILD_DIR:=$(BUILD_DIR)/thread$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/tclthread
  SUBMENU:=TCL
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=TclThreads
  DEPENDS:=tcl +libpthread
endef

define Package/tclthread/description
  Wsparcie wielowatkowosci dla tcl (?).
endef

CONFIGURE_ARGS += \
	--with-tcl=$(STAGING_DIR)/usr/lib/ \
	--with-tclinclude=$(STAGING_DIR)/usr/include/ \
	--disable-symbols \
	--enable-threads \
	--enable-shared \
	--enable-load


define Build/Compile
    $(call Build/Compile/Default, DESTDIR="$(PKG_INSTALL_DIR)"  all install )
endef


define Package/tclthread/install
	$(INSTALL_DIR)  $(1)/usr/lib/thread$(PKG_VERSION)
	$(INSTALL_BIN)  $(PKG_INSTALL_DIR)/usr/lib/thread$(PKG_VERSION)/libthread$(PKG_VERSION).so $(1)/usr/lib/thread$(PKG_VERSION)/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/thread$(PKG_VERSION)/pkgIndex.tcl $(1)/usr/lib/thread$(PKG_VERSION)/
endef

$(eval $(call BuildPackage,tclthread))

# $Id: Makefile 973 2005-05-19 21:22:09Z nico $

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=tclsignal
PKG_VERSION:=1.4
PKG_RELEASE:=22199
PKG_MD5SUM:=

PKG_SOURCE_URL:=http://www.nyx.net/~mschwart/

PKG_SOURCE:=signal_ext$(PKG_VERSION).tar.gz

PKG_BUILD_DIR:=$(BUILD_DIR)/signal_ext$(PKG_VERSION)
#PKG_INSTALL_DIR:=$(PKG_BUILD_DIR)/ipkg-install


include $(INCLUDE_DIR)/package.mk

define Package/tclsignal
  SUBMENU:=TCL
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=TclSignalExt
  DEPENDS:=+tcl 
endef

define Package/tclsignal/description
  Bliblioteka signal.so dla TCL 
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR); \
		$(TARGET_CONFIGURE_OPTS) \
		CFLAGS="$(TARGET_CFLAGS)" \
		./configure \
		--target=$(GNU_TARGET_NAME) \
		--host=$(GNU_TARGET_NAME) \
		--build=$(GNU_HOST_NAME) \
		--with-tcl=$(BUILD_DIR)/tcl8.6b1/unix/ \
		--prefix=$(PKG_INSTALL_DIR)/usr/lib/ \
	);
endef

define Build/Compile
	rm -rf $(PKG_INSTALL_DIR)
	mkdir -p $(PKG_INSTALL_DIR)/usr/lib
	$(MAKE) -C "$(PKG_BUILD_DIR)" CC="$(TARGET_CC)" \
		DESTDIR="$(PKG_INSTALL_DIR)" all install
endef

define Package/tclsignal/install
	install -d -m0755 $(1)/usr/lib/signal
	install -m0755 $(PKG_INSTALL_DIR)/usr/lib/signal/signal.so $(1)/usr/lib/signal/
	install -m0644 $(PKG_INSTALL_DIR)/usr/lib/signal/pkgIndex.tcl $(1)/usr/lib/signal/
endef

$(eval $(call BuildPackage,tclsignal))

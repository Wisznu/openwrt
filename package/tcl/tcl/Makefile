#
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=tcl
PKG_RELEASE:=2

ifdef      CONFIG_TCL_GENERATION_6
	#PKG_VERSION:=8.6.4
	#PKG_MD5SUM:=8b8c9d85469d8dbe32e51117b8ef11e3
        PKG_VERSION:=8.6.8
        PKG_MD5SUM:=9804911b9ac76f76d698f28894af6c55
	TCL_GENERATION=6
else ifdef CONFIG_TCL_GENERATION_4
	PKG_VERSION:=8.4.20
	PKG_MD5SUM:=b57ecc6540026fd4a83288ac6bf2b02b
	TCL_GENERATION=4
else 
	PKG_VERSION:=8.5.18
	PKG_MD5SUM:=9b80e9dde418ec92359ecc5739c6a9a8
	TCL_GENERATION=5
endif

PATCH_DIR:=patches/8.$(TCL_GENERATION)

PKG_SOURCE:=$(PKG_NAME)-core$(PKG_VERSION)-src.tar.gz
PKG_SOURCE_URL:=@SF/tcl

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)$(PKG_VERSION)


include $(INCLUDE_DIR)/package.mk

define Package/tcl
  SUBMENU:=TCL
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=TCL Lang
  URL:=http://www.tcl.tk
  DEPENDS:=+zlib +libpthread 
endef

define Package/tcl/description
	Tcl (Tool Command Language) is a very powerful but easy to learn dynamic
	programming language, suitable for a very wide range of uses, including web and
	desktop applications, networking, administration, testing and many more. Open
	source and business-friendly, Tcl is a mature yet evolving language that is truly
	cross platform, easily deployed and highly extensible.
endef

define Package/tcl/config
	select PACKAGE_libpthread if PACKAGE_tclthread

	choice
    	prompt "TCL Generation"
	    default TCL_GENERATION_6
		depends on PACKAGE_tcl
    	help
			Select generation of TCL.
	
	config TCL_GENERATION_4
	    bool "8.4"
	
	config TCL_GENERATION_5
	    bool "8.5"
	
	config TCL_GENERATION_6
	    bool "8.6"
	
	endchoice

	config TCL_CLOCK
		bool "Include clock.tcl"
		depends on PACKAGE_tcl
		default n

	config TCL_HISTORY
		bool "Include history.tcl"
		depends on PACKAGE_tcl
		default n

endef

CONFIGURE_PATH := unix

CONFIGURE_VARS += \
	tcl_cv_strstr_unbroken=ok \
	tcl_cv_strtoul_unbroken=ok\
	tcl_cv_strtod_unbroken=ok \
	tcl_cv_strtod_buggy=ok \
	tcl_cv_api_serial=termios

ifeq ($(CONFIG_PACKAGE_tclthread),y)
	THREAD_SW:=enable
else
	THREAD_SW:=disable
endif

CONFIGURE_ARGS += \
	--disable-shared \
	--disable-symbols \
	--disable-langinfo \
	--disable-framework \
	--disable-disableinfo \
	--disable-dll-unloading  \
	--$(THREAD_SW)-threads \
	--$(THREAD_SW)-load


MAKE_PATH := unix


define Build/Compile
	$(call Build/Compile/Default, DESTDIR="$(PKG_INSTALL_DIR)"  all install )
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/include/*.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/tclConfig.sh $(1)/usr/lib/
	$(SED) '/TCL_STUB_LIB_SPEC/s|-L|-L$(STAGING_DIR)|' -e '/TCL_INCLUDE_SPEC/s|-I"|-I"$(STAGING_DIR)|' $(1)/usr/lib/tclConfig.sh
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libtcl*.a $(1)/usr/lib/
endef

ifdef  CONFIG_TCL_HISTORY
	tcl_pkg += history.tcl
endif

ifdef  CONFIG_TCL_CLOCK
	tcl_pkg += clock.tcl
endif


define Package/tcl/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/tclsh8.$(TCL_GENERATION) $(1)/usr/bin/tclsh
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/tcl8/8.{4,5,6}
	$(INSTALL_DIR) $(1)/usr/lib/tcl8.$(TCL_GENERATION)

	$(foreach tcl, \
		tclIndex init.tcl package.tcl tm.tcl \
		parray.tcl \
		$(tcl_pkg) \
	, \
		if [ -e $(PKG_INSTALL_DIR)/usr/lib/tcl8.$(TCL_GENERATION)/$(tcl) ] ; then \
			$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/tcl8.$(TCL_GENERATION)/$(tcl) $(1)/usr/lib/tcl8.$(TCL_GENERATION)/; \
		fi; \
	)

# Jakiś problem w TCLu po wywaleniu komentarzy i pustych lini z ktoregoś z tych (^) plików nie potrafi załadować pakietów
#			$(SED) '/^\s*#/d; /^\s*$$$$/d' $(1)/usr/lib/tcl8.$(TCL_GENERATION)/$(tcl); \

	$(if $(CONFIG_TCL_CLOCK), \
		$(if $(CONFIG_TCL_GENERATION_4), \
			$(INSTALL_DIR) $(1)/usr/lib/tcl8.$(TCL_GENERATION)/msgcat1.3/; \
			$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/tcl8.$(TCL_GENERATION)/msgcat1.3/* $(1)/usr/lib/tcl8.$(TCL_GENERATION)/msgcat1.3/; \
			$(SED) '/^\s*#/d; /^\s*$$$$/d' $(1)/usr/lib/tcl8.$(TCL_GENERATION)/msgcat1.3/* \
		, \
			$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/tcl8/8.5/msgcat-*.tm $(1)/usr/lib/tcl8/8.5/; \
			$(SED) '/^\s*#/d; /^\s*$$$$/d' $(1)/usr/lib/tcl8/8.5/msgcat-*.tm \
	    ) \
	)

endef


$(eval $(call BuildPackage,tcl))

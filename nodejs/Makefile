#
# Copyright (C) 2008-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nodejs
PKG_VERSION:=HEAD

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/joyent/node.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_BUILD_PARALLEL:=1
HOST_BUILD_PARALLEL:=1
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/nodejs
  SECTION:=base
  CATEGORY:=Base system
  TITLE:=nodejs
endef

define Build/Configure
		$(PKG_BUILD_DIR)/configure
endef

define Build/Compile
		+$(MAKE) -j $(HOST_JOBS) -C $(PKG_BUILD_DIR) CC="$(HOSTCC)" all
endef

define Build/InstallDev
		$(INSTALL_BIN) $(PKG_BUILD_DIR)/out/Release/node $(STAGING_DIR_HOST)/bin/
endef

$(eval $(call BuildPackage,nodejs))


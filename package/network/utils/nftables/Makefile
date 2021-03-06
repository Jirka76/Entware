# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nftables
PKG_VERSION:=0.9.6
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://netfilter.org/projects/$(PKG_NAME)/files
PKG_HASH:=68d6fdfe8ab02303e6b1f13968a4022da5b0120110eaee3233d806857937b66e
PKG_MAINTAINER:=
PKG_LICENSE:=GPL-2.0

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

DISABLE_NLS:=

CONFIGURE_ARGS += \
        --disable-debug \
        --disable-man-doc \
        --with-mini-gmp \
        --without-cli \
        --disable-python

define Package/nftables/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Firewall
  TITLE:=nftables userspace utility
  DEPENDS:=+libnftnl
  URL:=http://netfilter.org/projects/nftables/
  PROVIDES:=nftables
endef

define Package/nftables-nojson
  $(Package/nftables/Default)
  TITLE+= no JSON support
  VARIANT:=nojson
  DEFAULT_VARIANT:=1
endef

define Package/nftables-json
  $(Package/nftables/Default)
  TITLE+= with JSON support
  VARIANT:=json
  DEPENDS+=+jansson
endef

ifeq ($(BUILD_VARIANT),json)
  CONFIGURE_ARGS += --with-json
endif

TARGET_CFLAGS += -flto
TARGET_LDFLAGS += -flto

define Package/nftables/install/Default
	$(INSTALL_DIR) $(1)/opt/sbin
	$(CP) $(PKG_INSTALL_DIR)/opt/sbin/nft $(1)/opt/sbin/
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/*.so* $(1)/opt/lib/
endef

Package/nftables-nojson/install = $(Package/nftables/install/Default)
Package/nftables-json/install = $(Package/nftables/install/Default)

$(eval $(call BuildPackage,nftables-nojson))
$(eval $(call BuildPackage,nftables-json))

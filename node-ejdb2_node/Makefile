# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=ejdb2_node
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=1.0.28
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=https://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_HASH:=af588f4f460848e779d45f47cf0b0984f972553ec81f3810941855ae9732b26d

PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=

PKG_BUILD_DEPENDS:=node/host node-yarn/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/node-ejdb2_node
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=EJDB2 Node.js native binding
  URL:=https://www.npmjs.com/package/ejdb2_node
  DEPENDS:=+node +libatomic
endef

define Package/node-ejdb2_node/description
 Embeddable JSON Database engine http://ejdb.org Node.js binding.
endef

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)
TARGET_LDFLAGS+=-latomic

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_target_arch=$(NODEJS_CPU) \
	npm_config_build_from_source=true \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_prefix=$(PKG_INSTALL_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	CMAKE_INCLUDE_PATH="$(STAGING_DIR)/usr/include:$(TOOLCHAIN_DIR)/include" \
	CMAKE_LIBRARY_PATH="$(STAGING_DIR)/usr/lib:$(TOOLCHAIN_DIR)/lib" \
	npm install -g $(PKG_BUILD_DIR)
	rm -rf $(TMP_DIR)/npm-tmp-$(TMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(TMPNPM)
endef

define Package/node-ejdb2_node/install
	$(INSTALL_DIR) $(1)/usr/lib/node/$(PKG_NPM_NAME)
	$(INSTALL_DIR) $(1)/usr/lib/node/$(PKG_NPM_NAME)/build/Release
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/{package.json,README.md} \
		$(1)/usr/lib/node/$(PKG_NPM_NAME)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/{Changelog,*.ts,*.js} \
		$(1)/usr/lib/node/$(PKG_NPM_NAME)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/node_modules \
		$(1)/usr/lib/node/$(PKG_NPM_NAME)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/linux-x64/ejdb2_node.node \
		$(1)/usr/lib/node/$(PKG_NPM_NAME)/build/Release
endef

$(eval $(call BuildPackage,node-ejdb2_node))

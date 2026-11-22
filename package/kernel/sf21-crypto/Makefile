#
# Siflower SF21 crypto engine driver (from the Siflower BSP)
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=sf21-crypto
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0-only

include $(INCLUDE_DIR)/package.mk

define KernelPackage/crypto-hw-sf21
  SUBMENU:=Cryptographic API modules
  TITLE:=Siflower SF21 crypto engine
  DEPENDS:=@TARGET_siflower_sf21
  FILES:=$(PKG_BUILD_DIR)/sf_crypto.ko
  AUTOLOAD:=$(call AutoProbe,sf_crypto)
endef

define KernelPackage/crypto-hw-sf21/description
 Hardware offload of AES (CTR, GCM, CCM) and MD5, SHA-1, SHA-224/256/384/512
 on the crypto engine of the Siflower SF21A6826/SF21H8898.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
endef

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="-DCONFIG_CRYPTO_SIFLOWER_HASH" \
		EXTRA_CFLAGS="-Wno-error -Wno-missing-prototypes -Wno-missing-declarations -Wno-unused-variable -Wno-unused-label" \
		CONFIG_CRYPTO_SIFLOWER_HASH=y modules
endef

$(eval $(call KernelPackage,crypto-hw-sf21))

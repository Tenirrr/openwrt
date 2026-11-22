# SPDX-License-Identifier: GPL-2.0-only
#
# Siflower DPNS userspace control tool (generic netlink <-> ubus), from the
# Siflower OpenWrt 22.03 BSP. Exposes ACL, TMU (QoS), VLAN, NAT, router and
# multicast configuration of the NPU as ubus objects.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sf21-dpns-genl
PKG_RELEASE:=2
PKG_LICENSE:=GPL-2.0-only

include $(INCLUDE_DIR)/package.mk

DPNS_SRC:=$(TOPDIR)/package/kernel/sf21-dpns/src

define Package/sf21-dpns-genl
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Siflower SF21 DPNS (NPU) control tool (ubus)
  DEPENDS:=@TARGET_siflower_sf21 +kmod-sf21-dpns +libnl-genl +libubus +libubox
endef

define Package/sf21-dpns-genl/description
 sf_genl_user daemon: configures the DPNS ACL, TMU (QoS queues, schedulers,
 shapers), VLAN, NAT, router and multicast tables through ubus.
 dpns-qos: hardware QoS (shaping, queue scheduling, classification) from UCI.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
	$(CP) $(DPNS_SRC)/dpns_genl/sf_genl.h \
		$(DPNS_SRC)/dpns_l2/se_common.h \
		$(DPNS_SRC)/dpns_tmu/sf_tmu_regs.h \
		$(DPNS_SRC)/dpns_tmu/sf_tmu_genl.h \
		$(DPNS_SRC)/dpns_mcast/se_mcast_cfg.h \
		$(DPNS_SRC)/dpns_mcast/sf_genl_mcast.h \
		$(DPNS_SRC)/dpns_acl/dpns_acl_msg.h \
		$(DPNS_SRC)/include/sf_genl_msg.h \
		$(DPNS_SRC)/dpns_nat/dpns_nat_genl.h \
		$(DPNS_SRC)/dpns_vlan/dpns_vlan_genl.h \
		$(DPNS_SRC)/dpns_router/dpns_router_genl.h \
		$(DPNS_SRC)/dpns_common/dpns_common_genl.h \
		$(PKG_BUILD_DIR)/
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CC="$(TARGET_CC)" \
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -I$(STAGING_DIR)/usr/include/libnl3 -Wall -Wno-unused-but-set-variable -Wno-unused-function" \
		LDFLAGS="$(TARGET_LDFLAGS) -lubus -lubox -lnl-genl-3 -lnl-3"
endef

define Package/sf21-dpns-genl/conffiles
/etc/config/dpns-qos
endef

define Package/sf21-dpns-genl/install
	$(INSTALL_DIR) $(1)/usr/sbin $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/sf_genl_user $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/sf-genl.init $(1)/etc/init.d/sf-genl
	$(INSTALL_BIN) ./files/dpns-qos.init $(1)/etc/init.d/dpns-qos
	$(INSTALL_CONF) ./files/dpns-qos.config $(1)/etc/config/dpns-qos
endef

$(eval $(call BuildPackage,sf21-dpns-genl))

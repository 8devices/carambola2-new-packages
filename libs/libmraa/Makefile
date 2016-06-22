#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk


PKG_NAME:=libmraa
PKG_VERSION:=1.0.0

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/8devices/mraa.git
PKG_SOURCE_VERSION:=9116655da79cc7d6432b25927a495e22221f86fd
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_BUILD_DEPENDS:=node python/host swig/host node/host
CMAKE_INSTALL:=1

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=LGPL-2.1
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_OPTIONS=-DBUILDARCH=$(CONFIG_ARCH) \
	-DENABLEEXAMPLES=1 \
	-DNODEJS_EXECUTABLE=$(STAGING_DIR)/host/bin/node \
	-DNODEJS_INCLUDE_DIRS=$(STAGING_DIR)/usr/include/node/ \
	-DSWIG_EXECUTABLE=$(STAGING_DIR)/host/bin/swig \

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include/node

define Package/libmraa
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libstdcpp
  TITLE:=Intel IoT lowlevel IO library
endef

define Package/libmraa-nodejs
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libmraa +node
  TITLE:=Intel IoT lowlevel IO library nodejs bindings
endef

define Package/libmraa-python
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libmraa +python
  TITLE:=Intel IoT lowlevel IO library python bindings
endef

define Package/libmraa-examples
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libmraa +libmraa-nodejs +libmraa-python
  TITLE:=Intel IoT lowlevel IO library examples
endef

define Package/libmraa/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/libmraa.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/mraa/examples/mraa-gpio $(1)/usr/bin/
endef

MRAA_EXAMPLES_C := \
	analogin_a0 blink-io blink_onboard cycle-pwm3 gpio_read6 hellomraa i2c_HMC5883L \
	isr_pin6 mmap-io2 mraa-i2c spi_max7219 spi_mcp4261 uart uart_ow

MRAA_EXAMPLES_CPP := AioA0 I2c-compass Iio-dummy Isr-pin6 Pwm3-cycle Spi-pot Uart UartOW

define Package/libmraa-examples/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(foreach example, $(MRAA_EXAMPLES_C),	\
		$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/mraa/examples/$(example) $(1)/usr/bin/;)
	$(foreach example, $(MRAA_EXAMPLES_CPP),	\
		$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/mraa/examples/c++/$(example) $(1)/usr/bin/;)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/mraa/examples/python/*.py $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/mraa/examples/javascript/*.js $(1)/usr/bin/
endef

define Package/libmraa-nodejs/install
	$(INSTALL_DIR) $(1)/usr/lib/node/mraa
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/node_modules/mraa/* $(1)/usr/lib/node/mraa/
endef

define Package/libmraa-python/install
	$(INSTALL_DIR) $(1)/usr/lib/python2.7/site-packages
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/python2.7/site-packages/* $(1)/usr/lib/python2.7/site-packages/
endef

$(eval $(call BuildPackage,libmraa))
$(eval $(call BuildPackage,libmraa-nodejs))
$(eval $(call BuildPackage,libmraa-python))
$(eval $(call BuildPackage,libmraa-examples))


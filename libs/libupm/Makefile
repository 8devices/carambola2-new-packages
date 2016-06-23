#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libupm
PKG_VERSION:=0.7.0

PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/intel-iot-devkit/upm.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=29bfa7ef79ec7e944e8803b39fe1fb71ab7c0825
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
CMAKE_INSTALL:=1

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=LGPL-2.1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

UPM_MODULES:= \
	a110x ad8232 adafruitms1438 adafruitss adc121c021 adis16448 ads1x15 adxl335 adxl345 adxrs610 am2315 \
	apa102 apds9002 apds9930 at42qt1070 biss0001 bma220 bmi160 bmp280 bmpx8x bno055 buzzer cjq4435 cwlsxxa \
	dfrph ds1307 ds1808lc ds18b20 ds2413 ecs1030 enc03r flex \
	gas gp2y0a grove grovecollision groveehr groveeldriver groveelectromagnet groveemg grovegprs grovegsr grovelinefinder \
	grovemd grovemoisture groveo2 grovescam grovespeaker groveultrasonic grovevdiv grovewater grovewfs \
	guvas12d h3lis331dl h803x hcsr04 hdxxvxta hlg150h hm11 hmc5883l hmtrp hp20x ht9170 htu21d hwxpxx hx711 \
	ili9341 ina132 isd1820 itg3200 joystick12 kxcjk1013 \
	l298 l3gd20 ldt0028 lm35 lol loudness lp8860 lpd8806 lsm303 lsm9ds0 \
	m24lr64e max31723 max31855 max44000 max44009 max5487 maxds3231m maxsonarez mcp9808 mg811 mhz16 mic micsv89 \
	mlx90614 mma7455 mma7660 mpl3115a2 mpr121 mpu9150 mq303a my9221 nlgpio16 nrf24l01 nrf8001 nunchuck otp538u \
	pca9685 pn532 ppd42ns pulsensor rfr359f rgbringcoder rhusb rotaryencoder rpr220 \
	servo si1132 si114x si7005 sm130 smartdrive ssd1351 st7735 stepmotor sx1276 sx6119 \
	t3311 t6713 ta12200 tcs3414cs teams tex00 th02 tm1637 tsl2561 ttp223 \
	ublox6 uln200xa urm37 vcap waterlevel wheelencoder wt5001 xbee yg1006 zfm20

CMAKE_OPTIONS=-DBUILDARCH=$(CONFIG_ARCH) \
	-DSWIG_DIR=$(STAGING_DIR)/host/bin \
	-DSWIG_EXECUTABLE=$(STAGING_DIR)/host/bin/swig \
	-DNODE_ROOT_DIR=$(STAGING_DIR)/usr/ \
	-DNODEJS_EXECUTABLE=$(STAGING_DIR)/host/bin/node \
	-DPYTHON_LIBRARY=$(STAGING_DIR)/usr/lib/libpython2.7.so \
	-DPYTHON_INCLUDE_DIR=$(STAGING_DIR)/usr/include/python2.7/

define Package/libupm/Default
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libmraa +librt +python +libmodbus +libjpeg
  SUBMENU:=IoT
endef

define Package/libupm
  $(call Package/libupm/Default)
  TITLE:=Intel IoT sensor library - Full
endef

define Package/libupm/install/Default
	$(INSTALL_DIR) $(1)/usr/lib/{node/,python2.7/site-packages}; \
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libupm-$(2).so* $(1)/usr/lib/; \
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/jsupm_$(2) $(1)/usr/lib/node/; \
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/python2.7/site-packages/{pyupm_$(2).py,_pyupm_$(2).so} \
		$(1)/usr/lib/python2.7/site-packages/ ;
endef

define Package/libupm/install
	$(foreach module, $(UPM_MODULES),	\
  		$(call Package/libupm/install/Default,$(1),$(module)))
endef

define UpmPackage
define Package/libupm-$(1)
  $(call Package/libupm/Default)
  TITLE:=Intel IoT sensor library - $(1)
endef

define Package/libupm-$(1)/install
  	$(call Package/libupm/install/Default,$$(1),$(1))
endef
endef

$(eval $(call BuildPackage,libupm))
$(foreach package, $(UPM_MODULES),			\
	$(eval $(call UpmPackage,$(package)))		\
	$(eval $(call BuildPackage,libupm-$(package)))	\
)

# DeviceHive BLE Guide
##Intro

DeviceHive makes any connected device part of the Internet of Things. It provides the communication layer, control software and multi-platform libraries to bootstrap development of smart energy, home automation, remote sensing, telemetry, remote control and monitoring software, and [much more](http://devicehive.com). 

This tutorial will guide you through the following steps to automate use of BLE devices using DeviceHive:

1. [Flashing openwrt firmware with DH BLE](#firmware)
2. [Creating DeviceHive playground account](#playground)
3. [Configuring gateway to connect to DH server](#configure)
4. [Discovering BLE devices](#discovery)
5. [BLE Automation Examples](#examples)
  * [Automating Honeywell Air Purifier](#purifier)
  * [Automating SATECHI LED Bluetooth lamp](#bulb)

##  <a name="firmware"></a>1. Flashing openwrt firmware with DH BLE
DeviceHive is available on many [platforms and languages](http://devicehive.com/documentation) but for this tutorial we will be using a precompiled firmware for openwrt enabled router [DIR-505L](http://goo.gl/v0tJG9) that includes DeviceHive BLE router.

Download [firmware file](https://drive.google.com/file/d/0B9Db_guVlBYjSlpybW1uRUszeE0). It has 2 files and a sources folder included.

**openwrt-ar71xx-generic-dir-505-a1-squashfs-factory.bin** - should be used to flash firmware over the factory d-liunk image. See [openwrt page](http://wiki.openwrt.org/toh/d-link/dir-505#debricking) for instructions.

**openwrt-ar71xx-generic-dir-505-a1-squashfs-sysupgrade.bin ** - should be used to flash firmware over the existing openwrt image. See [openwrt page](http://wiki.openwrt.org/doc/howto/generic.sysupgrade) for instructions.

If you have other openwrt image that you want to include DeviceHive gateway, you can try to build your own image using **/src** folder. This is however beyond current tutorial. 

Once you've flashed firmware, switch router toggle to `Router` position. You should be able to connect to router using Ethernet cable as it acts as a router and has build it DHCP server. 

##  <a name="playground"></a>2. Creating DeviceHive playground account
##  <a name="configure"></a>3. Configuring gateway to connect to DH server
##  <a name="discovery"></a>4. Discovering BLE devices
##  <a name="examples"></a>5. BLE Automation Examples
### <a name="purifier"></a>5.1 Automating Honeywell Air Purifier
### <a name="bulb"></a>5.2 Automating SATECHI LED Bluetooth lamp

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

**openwrt-ar71xx-generic-dir-505-a1-squashfs-sysupgrade.bin** - should be used to flash firmware over the existing openwrt image. See [openwrt page](http://wiki.openwrt.org/doc/howto/generic.sysupgrade) for instructions.

*If you have other openwrt image that you want to include DeviceHive gateway, you can try to build your own image using **/src** folder. This is however beyond current tutorial.*

Once you've flashed firmware, switch router toggle to `Router` position. You should be able to connect to router using Ethernet cable as it acts as a router and has build it DHCP server. 

##  <a name="playground"></a>2. Creating DeviceHive playground account

To control your gateway and devices device over the internet you will need DeviceHive server instance to be hosted on some server on in the cloud. DeviceHive.com conveniently provides development playgrounds for free so you can start developing in minutes. Go to  [Playground](http://devicehive.com/user/register) page and sign up for a new account. You can use GitHub, Facebook or Google sign in options for faster process. 

Once you follow signup process you will be presented with pair of urls:
```
Admin Url: 
http://xyz.pg.devicehive.com/admin
API Url: 
http://xyz.pg.devicehive.com/api
```

Use those for server api url later and to see/manage your devices online using **admin link**. You need that one to **issue access token** for api access. To generate one please follow admin link and sign in using your credentials or github/facebook/google account. 
On the DeviceHive Admin interface click `Access Keys` tab and then `create new access key` button. 
Enter any value into `label` field, for example *restkey*, leave Type=Default  and click `Save`.
You will see a new key appear in the list. Use the Key value for Authorization header for the REST calls, example:
```
Authorization: Bearer 7Dy4Bcqz+eRjax29lFxFyz8GxvGQxA3N4xfzon4/97o=
``` 

##  <a name="configure"></a>3. Configuring gateway to connect to DH server

This section covers two topics: making sure your router can connect to internet and it can discover and connect to your DeviceHive playground.

### Configure router for internet access

By default provided firmware has WiFi interface disabled and Ethernet acts as DHCP server, available on `192.168.1.1` address. This is useful to connect your computer as a client and manage settings on the router. But for convenience you want to configure wlan interface to connect to the local wifi network.
To do that, first connect  to router via telnet as root user:
```
telnet 192.168.1.1
``` 
You should see OpenWrt invitation. If not, please check if your Ethernet connection is up and you have local address in `192.168.1.x` network with `192.168.1.1` gateway.
At this point you can continue using `telnet` or assign password to root user to be able to use `ssh` connection. To do that type `passw` in telnet terminal and type a new password as prompted.  Now you can connect to router using ssh as well as use scp as needed.
```
ssh root@192.168.1.1
```

Now as we gained access to router terminal, follow instructions on openwrt website to configure [`/etc/config/network`](http://wiki.openwrt.org/doc/uci/network) and [`/etc/config/wireless`](wiki.openwrt.org/doc/uci/wireless) files.
For simple wifi client mode you can use the following configuration replacing `YOUR_WIFI_SSID` and `YOUR_WIFI_PASSWORD`:

`> vi /etc/config/wireless`
```
config wifi-device  radio0
        option type     mac80211
        option channel  auto
        option hwmode   11g
        option path     'platform/ar933x_wmac'
        option htmode   HT20
        option disabled 0

config wifi-iface
        option device   radio0
        option network  wlan
        option mode     sta
        option ssid     YOUR_WIFI_SSID
        option encryption psk2
        option key YOUR_WIFI_PASSWORD
```

`> vi /etc/config/network`

```
config interface 'loopback'
        option ifname 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config globals 'globals'
        option ula_prefix 'fddd:4f5b:f024::/48'

config interface 'lan'
        option ifname 'eth1'
        option force_link '1'
        # option type 'bridge'
        option proto 'static'
        option ipaddr '192.168.1.1'
        option netmask '255.255.255.0'
        option ip6assign '60'

config interface wlan
        option proto 'dhcp'
        option ifname 'wlan0'
```

`> reboot`


##  <a name="discovery"></a>4. Discovering BLE devices
##  <a name="examples"></a>5. BLE Automation Examples
### <a name="purifier"></a>5.1 Automating Honeywell Air Purifier
### <a name="bulb"></a>5.2 Automating SATECHI LED Bluetooth lamp

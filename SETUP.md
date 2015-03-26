# DeviceHive openwrt Setup Guide
##Intro

This tutorial will guide you through the following steps to automate use of BLE devices using DeviceHive:

1. [Flashing openwrt firmware with DH BLE](#firmware)
2. [Creating DeviceHive playground account](#playground)
3. [Configuring gateway to connect to DH server](#configure)
4. [Discovering BLE devices](#discovery)

##  <a name="firmware"></a>1. Flashing openwrt firmware with DH BLE
DeviceHive is available on many [platforms and languages](http://devicehive.com/documentation) but for this tutorial we will be using a precompiled firmware for openwrt enabled router [DIR-505L](http://goo.gl/v0tJG9) that includes DeviceHive BLE router. You will also need a Bluetooth Low Energy USB Dongle to use with this router, like [this one](http://goo.gl/CtnIyd) for example. 

Download [firmware file](https://drive.google.com/file/d/0B9Db_guVlBYjSlpybW1uRUszeE0). It has 2 files and a sources folder included.

**openwrt-ar71xx-generic-dir-505-a1-squashfs-factory.bin** - should be used to flash firmware over the factory d-link image. See [openwrt page](http://wiki.openwrt.org/toh/d-link/dir-505#debricking) for instructions.

**openwrt-ar71xx-generic-dir-505-a1-squashfs-sysupgrade.bin** - should be used to flash firmware over the existing openwrt image. See [openwrt page](http://wiki.openwrt.org/doc/howto/generic.sysupgrade) for instructions.

*If you have other openwrt image that you want to include DeviceHive gateway, you can try to build your own image using **/src** folder. This is however beyond current tutorial.*

Once you've flashed firmware, switch router toggle to `Router` position. You should be able to connect to router using Ethernet cable as it acts as a router and has built-in DHCP server. 

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

### Configuring router for internet access

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

Confirm that router connected to wifi network in ssh terminal:
```
> ifconfig
...
> ping devicehive.com
...
```

### Connecting to your DeviceHive playground

Now is the time to configure provided DeviceHive BLE Gateway to connect to your specific playground api. 
Corresponding settings are located in `/etc/init.d/devicehive-blegw` file.
Open file in [vim](http://www.radford.edu/~mhtay/CPSC120/VIM_Editor_Commands.htm) editor `vi /etc/init.d/devicehive-blegw` and change **DH_SERVER** property to your api url, ex:
```
DH_SERVER=http://xyz.pg.devicehive.com/api
```
Make sure, your BLE USB dongle is connected to router and reboot router: 
```
> reboot
```
After reboot you should see your router in the `Devices` section on the DeviceHive Playground Admin page. It will appear as **btle_gw**.

##  <a name="discovery"></a>4. Discovering BLE devices

In order to connect to BLE devices you'll need to know their MAC addresses. There are multiple ways to know those addresses. Either looking on the device label if available or using some mobile apps. The easiest way however is to use `hcitool` command on gateway router: 
```
> hcitool blescan
LE Scan ...
```

You will see lots of device MAC and Name pairs. Most devices will provide meaningful name but some won't so you'll have to guess.


Alternatively you can use DeviceHive REST api or Admin interface to send `scan` command and gateway will reply with the list of found ble devices.
To use Admin interface, login and go to `Devices` tab, then click `detail` button on btle_gw device, switch to `commands` tab and click `enter new command`. Enter `scan` as command name, leave parameters empty and click `push`. In about 20 seconds gateway should reply with the list of ble devices and their MAC addresses (You may need to click `refresh` to see result):
```javascript
{
	"20:C3:8F:F5:49:B4":"SATECHILED-0",
	"30:8C:FB:5A:7A:F7":"(unknown)",
	"F4:F9:51:D2:3F:71":"(unknown)",
	"F9:8A:AD:01:CA:71":"W Activite 7D"
}
```
In this example `20:C3:8F:F5:49:B4` is a MAC address of Bluetooth led bulb from SATECHI and `F9:8A:AD:01:CA:71` is my Withings Activite watch.

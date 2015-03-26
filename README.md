# DeviceHive BLE Guide
##Intro

DeviceHive makes any connected device part of the Internet of Things. It provides the communication layer, control software and multi-platform libraries to bootstrap development of smart energy, home automation, remote sensing, telemetry, remote control and monitoring software, and [much more](http://devicehive.com). 

## Setup

These are short instructions to start (*for more details look [here](SETUP.md).*):

1. Create DeviceHive playground account at [`http://devicehive.com`](http://devicehive.com/user)

2. Install [openwrt image](https://drive.google.com/file/d/0B9Db_guVlBYjSlpybW1uRUszeE0) with preinstalled DeviceHive BLE Gateway [(help me)](SETUP.md#firmware)

3. Configure gateway to connect to your DeviceHive playground api in `/etc/init.d/devicehive-blegw` [(help me)](SETUP.md#configure)


## <a name="purifier"></a>Automating Honeywell Air Purifier

This example demonstrates how to control BLE enabled air purifier [Honeywell HPA250B](http://www.honeywellcleanair.com/air-purifiers/bluetooth/) from the web page via DeviceHive cloud.

Simply open provieded example `examples/purifier.html` or [jsFiddle](http://jsfiddle.net/demon_xxi/6w50v3dd/2/) and change configurartion parameters with your values:

```javascript
    // DeviceHive server api url, ex: http://xyz.pg.devicehive.com/api
    var DEVICE_HIVE_API = 'http://nn7975.pg.devicehive.com/api';

    // Authentication token. May be Bearer or Basic scheme
    var AUTH_TOKEN = 'Bearer DgG0lZXdEoei7YWB/O3bf57aDVZCsP2Bw8Hyb3KShwk=';

    // MAC address of BLE device
    var DEVICE_MAC = '1C:BA:8C:23:19:98';
```

You should be able to control your purifier now given everything works as expected :)
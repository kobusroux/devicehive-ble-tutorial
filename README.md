# DeviceHive BLE Guide
##Intro

DeviceHive makes any connected device part of the Internet of Things. It provides the communication layer, control software and multi-platform libraries to bootstrap development of smart energy, home automation, remote sensing, telemetry, remote control and monitoring software, and [much more](http://devicehive.com). 

## Setup

These are prerequirements to start:

1. Create DeviceHive playground account at [`http://devicehive.com`](http://devicehive.com/user)


2. Install [openwrt image](https://drive.google.com/file/d/0B9Db_guVlBYjSlpybW1uRUszeE0) with preinstalled DeviceHive BLE Gateway [(help me)](SETUP.md#firmware)


3. Configure gateway to connect to your DeviceHive playground api in `/etc/init.d/devicehive-blegw` [(help me)](SETUP.md#configure)

## <a name="purifier"></a>Automating Honeywell Air Purifier

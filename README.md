# homebridge-mi-flora-filtered


[![NPM version](https://badge.fury.io/js/homebridge-mi-flora-filtered.svg)](https://npmjs.org/package/homebridge-mi-flora-filtered)
![License](https://img.shields.io/badge/license-MIT-lightgrey.svg)
[![Downloads](https://img.shields.io/npm/dm/homebridge-mi-flora-filtered.svg)](https://npmjs.org/package/homebridge-mi-flora-filtered)

This is a [Homebridge](https://github.com/nfarina/homebridge) plugin for exposing the Xiaomi Flower Care / Flower Mate / Flower Monitor / Mi Flora devices to HomeKit. Historical display of temperature / moisture data is available via HomeKit apps that support graphing (e.g. Elgato Eve).

### Additional functionality
In this fork of homebridge-mi-flora, the data is first filtered with a moving-median filter to remove read errors. This is fixed with in this fork. Due to the median filter, the moving-median package is necessary. It should be installed automatically as dependency, but can be manually installed with:

```
(sudo) npm install -g moving-median
```

<img src=https://raw.githubusercontent.com/honkmaster/homebridge-mi-flower-care/master/images/flower_care.jpg />


## Installation

### Prerequisites

#### System dependencies

This plugin is using [node-mi-flora](https://github.com/demirhanaydin/node-mi-flora) / [Noble](https://github.com/noble/noble) in the background with the same package dependencies. You can install these dependencies using `apt-get`, if not already done.

```
(sudo) apt-get install bluetooth bluez libbluetooth-dev libudev-dev
```

It is recommended to install [noble](https://github.com/noble/noble), [node-gyp](https://github.com/nodejs/node-gyp) and [bluetooth-hci-socket](https://github.com/noble/node-bluetooth-hci-socket) manually with the following commands:

```
sudo npm install --unsafe-perm -g bluetooth-hci-socket
sudo npm install -g --unsafe-perm noble
sudo npm install -g --unsafe-perm node-gyp
```

Due to a bug in the [Noble](https://github.com/noble/noble) package, the maximum working version of Nodejs is 9.11.2.

For more details and descriptions for other platforms see the [Noble documentation](https://github.com/noble/noble#readme).

#### MAC address

Ensure you know the MAC address of your Xiaomi Flower Care. You can use `hcitool lescan` to scan for devices. The device will appear as `AA:BB:CC:DD:EE:FF Flower care` in the list.

### npm

```
(sudo) npm install -g --unsafe-perm homebridge-mi-flora-filtered
```

## Example Configuration

```
{
  "accessory": "mi-flower-care",
  "name": "Golden cane palm",
  "deviceId": "AA:BB:CC:DD:EE:FF",
  "interval": 300
}
``` 

| Key           | Description | Optional / Required |
|---------------|-------------|---------------------|
| accessory     | Has to be `mi-flower-care`. | Required |
| name          | The name of this accessory. This will appear in your HomeKit app. | Required |
| deviceId      | The MAC address of your Xiaomi Flower Care device. | Required |
| interval      | Frequency of data refresh in seconds. Minimum: 1 (not recommended); Maximum: 600 (due to FakeGato). | Required |
| humidityAlertLevel | Humidity level in percent used to trigger the humidity alert contact sensor. | Optional |
| lowLightAlertLevel |  Low light level in Lux used to trigger a low light alert contact sensor. | Optional |

Typical values for `humidityAlertLevel`are 30 (%) and 2000 (Lux) for `lowLightAlertLevel`. 

## Running

Due to Bluetooth access, Homebridge **must** run with elevated privileges to work correctly i.e. sudo or root.

## Note

The plugins is using Bluetooth LE (Low Energy) to connect to the Xiaomi Flower Care devices. Therefore, the first measured values are only visible after the first broadcast of the sensor. Up to this point the plugin is marked as inactive in HomeKit. In the worst case, the waiting time can last up to several minutes. Just have a little patience.

## Credits

* lucavb - homebridge-mi-flora
* demirhanaydin - node-mi-flora
* simont77 - fakegato-history

## Legal

Xiaomi and Mi are registered trademarks of BEIJING XIAOMI TECHNOLOGY CO., LTD.

This project is in no way affiliated with, authorized, maintained, sponsored or endorsed by BEIJING XIAOMI TECHNOLOGY CO., LTD or any of its affiliates or subsidiaries.

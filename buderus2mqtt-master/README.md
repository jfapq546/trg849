# mqtt-km200

[![mqtt-smarthome](https://img.shields.io/badge/mqtt-smarthome-blue.svg)](https://github.com/mqtt-smarthome/mqtt-smarthome)
[![NPM version](https://badge.fury.io/js/buderus2mqtt.svg)](http://badge.fury.io/js/buderus2mqtt)
[![Dependency Status](https://img.shields.io/gemnasium/krambox/buderus2mqtt.svg?maxAge=2592000)](https://gemnasium.com/github.com/krambox/buderus2mqtt)
[![Build Status](https://travis-ci.org/krambox/buderus2mqtt.svg?branch=master)](https://travis-ci.org/krambox/buderus2mqtt)
[![Maintainability](https://api.codeclimate.com/v1/badges/323bbf948a25557a2406/maintainability)](https://codeclimate.com/github/krambox/buderus2mqtt/maintainability)
[![js-semistandard-style](https://img.shields.io/badge/code%20style-semistandard-brightgreen.svg?style=flat-square)](https://github.com/Flet/semistandard)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Written and (C) 2015-18 Kai Kramer, based on an idea and code from Andreas Hahn.
Build with https://github.com/hobbyquaker/xyz2mqtt-skeleton from Sebastian Raff.

Provided under the terms of the MIT license.

## Overview

buderus2mqtt is a gateway between a KM200 Buderus internet gateway and MQTT
with the  https://github.com/mqtt-smarthome topic and payload format.

## Installation

The recommended way is via docker hub.

The bridge requires a configuration file with the measuring points. This file should be stored in a shared host directory.

In addition, environment variables with the addresses and the key are required.  

    docker run --env-file ./km200.env -v <local data directory with config.yml>:/data  --name buderus2mqtt -d krambox/buderus2mqtt

Environment variables in file km200.env

```
KM200_url=mqtt://<MQTT Host IP>
KM200_config=/data/config.yml
KM200_passcode=<AES k generated with https://ssl-account.com/km200.andreashahn.info/ >
KM200_km200=<KM200 IP>
```

### Key

AES key generator for the KM200 web gateway:  https://ssl-account.com/km200.andreashahn.info/

### Configuration with measuring points

config.yml example

```
measurements:
  - url: '/system/sensors/temperatures/chimney'
  - url: '/system/sensors/temperatures/outdoor_t1'
  - url: '/system/sensors/temperatures/supply_t1_setpoint'
  - url: '/heatSources/actualSupplyTemperature'
  - url: '/heatingCircuits/hc1/temperatureLevels/day'
  - url: '/heatingCircuits/hc1/temperatureLevels/night'
  - url: '/heatingCircuits/hc1/roomtemperature'
  - url: '/heatingCircuits/hc1/temperatureRoomSetpoint'
  - url: '/dhwCircuits/dhw1/actualTemp'
  - url: '/dhwCircuits/dhw1/setTemperature'
  - url: '/system/appliance/workingTime/centralHeating'
  - url: '/system/appliance/workingTime/secondBurner'
  - url: '/system/appliance/workingTime/totalSystem'
  - url: '/dhwCircuits/dhw1/workingTime'
  - url: '/system/appliance/numberOfStarts'
  - url: '/system/appliance/fanSpeed'
  - url: '/system/appliance/flameCurrent'
  - url: '/system/appliance/actualPower'
  - url: '/system/appliance/powerSetpoint'
  - url: '/dhwCircuits/dhw1/waterFlow'
```

The  tool scan.js is used to determine the possible and measurings. This
allows you to retrieve the measuring points of the installed KM200 and display
it in a table on the console. 

```
./scan.js -p <KEY generated with https://ssl-account.com/km200.andreashahn.info/ > -k <km200 ip> gb135.txt
```


Or via direct call

    ./km200mqtt.js -u mqtt://192.168.1.13 -k 192.168.1.162 -p <AES key>


## Build and run local Docker container

    docker build -t km200 .

    docker run --env-file ./km200.env -v <local data directory with config.yml>:/data -it km200

## Local build and development

Call ./km200mqtt.js with -h option and set properties with command line. 

Example

    ./km200mqtt.js  -url mqtt://<MQTT Host IP> \
        -config ./config.yml \
        -passcode <AES k generated with https://ssl-account.com/km200.andreashahn.info/ > \
        -km200 <KM200 IP>

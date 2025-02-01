# Purpose
To provide a means for communicating with one or more Sol-Ark/Deye/Sunsynk inverters using a single RS485 port and sending it to Home Assistant

# Hardware
1. In the interest of saving money, an ESP32 based device was choosen with a suitably protected RS485 port built in.  The [LilyGo T-CAN485](https://github.com/Xinyuan-LilyGO/T-CAN485) can be purchased on [ebay](https://www.ebay.com/itm/385405630728), [AliExpress](https://www.aliexpress.us/item/3256803437719340.html), or directly from [LilyGo](https://lilygo.cc/products/t-can485).

    - [Schematics](/T-CAN485.pdf)
    - [Pinmap](T-CAN-PINMAP.jpg)
    - [DXF](/T-CAN485.DXF)

2. If multiple inverters or a closed-loop battery are used, an RJ45 splitter/combiner is required to connect everything together.  Something similar to [this from Amazon](https://www.amazon.com/dp/B0069LVUPS) will do.  Depending upon the number of required connections, multiple combiners may be needed.

    NOTE - I am in no way affiliated with LilyGo, Amazon, AliExpress, or ebay.

3. The LilyGo T-CAN485 board needs a power supply.  A standard 5V DC supply can be connected to the USB-C port. Alternatively, a 5-12V DC supply can be connected to the DC power header.

4. (Optional) There are a few printed enclosure designs available for this PCB.
    - [Printables 1](https://www.printables.com/model/949875-lilygo-t-can485-case) - NouveauDommag_200197
    - [Thingiverse 1](https://www.thingiverse.com/thing:6196129) - janneman001
    - [Thingiverse 2](https://www.thingiverse.com/thing:6788996) - CMDRTheoJones

# Firmware
ESPHome can be installed in a number of ways. I prefer to use docker-compose, so it would look something like this:

```
services:
  esphome:
    container_name: esphome
    image: esphome/esphome
    environment:
      - ESPHOME_DASHBOARD_USE_PING=true
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /your/esphome/config:/config
    restart: unless-stopped
    privileged: true
    ports:
      - 6052:6052
```
Make sure to substitute the appropriate folder in place of "/your/esphome/config".

Alternatively, ESPHome can be installed as an add-on within Home Assistant by following the instructions [here](https://esphome.io/guides/getting_started_hassio).

Once ESPHome is running, create a new device using the ESPHome Device Builder interface and choose the appropriate YAML code below depending upon your inverter setup.

- [Single 1 Phase Inverter](/ESPHome-1P-Sunsynk-Deye.yaml)
- [Primary/Secondary 1 Phase Inverters](/ESPHome-1P-Sunsynk-Deye-Master-Slave.yaml)
- [Single 1 Phase 12k Inverter](/DEY12k-modbus.yml) - ! Not Tested !
- [Single 3 Phase Inverter](/ESPHome-3P-Sunsynk-Deye.yaml) - ! Not Tested !

Read through the configuration file and disable any metrics which aren't required as this will minimize Home Assistant database growth.
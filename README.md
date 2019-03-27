# Incredibly Cheap Wireless Window/Door Sensors

## Overview

This project is intended to provide tools/code/documentation for creating and setting up very cheap window/door sensors. The project will not describe every detail in the process but it will point you to the needed links. The sensors are designed to work within the following parameters.

- Cost under $5 each
- Work seemlessly with home automation software
- Be easy to create and maintain
- Be wireless (Wifi & Battery powered)

![](images/profile_view.png)
![](images/open_view.png)

## Supplies/Requirements

### Sensor Components

- x1 [ESP8266 12E](https://www.aliexpress.com/item/new-ESP8266-Remote-Serial-Port-WIFI-Transceiver-Wireless-Module-Esp-12F-AP-STA/32633529267.html)
- x1 [Normally closed Reed Switch](https://www.aliexpress.com/item/5pcs-Reed-Switch-3-pin-Magnetic-Switch-2-5-14mm-Normally-Open-Normally-Closed-Conversion-2/32947287626.html)
- x1 [3.7v YX-W9B Rechargable Battery]()
- x1 2" insulated small guage copper wire (The twisted pairs of a ethernet cable work perfectly)

### Development Components

- x1 [USB to TTL adapter](http://a.co/d/hhTu3ec) (Drivers can be found [here]( http://www.prolific.com.tw/US/ShowProduct.aspx?pcid=41&showlevel=0041-0041))
- (Optional, but Highly Recommended) ESP to Breadboard Adapter ([Pin Headers](https://www.aliexpress.com/item/86056-Free-Shipping-20pcs-2mm-40-Pin-Male-Single-Row-Pin-Header-Strip/32784910355.html), [Adapter](https://www.aliexpress.com/item/IDE-2-5-to-3-5-inch-Laptop-Hard-Drive-Converter-Adapter/32335319922.html))
    - The Pin Headers fit into the holes on the ESP12E chip
    - The Adapter connects the ESP to the breadboard without needing to solder anything!
- (Optional, but Highly Recommended) [Breadboard & jumper wires](https://www.aliexpress.com/item/3-3V-5V-MB102-Breadboard-power-module-MB-102-830-points-Solderless-Prototype-Bread-board-kit/32697390495.html)


### Software Components

- [ESPHomeFlasher](https://github.com/esphome/esphome-flasher/releases)
- [ESPHome](https://esphome.io)
- [Home Automation Software](https://www.home-assistant.io)

## Building & Testing
The general build process goes like:

1. Flash your ESP8266 with the ESPHome firmware
2. Print the enclosure ([Bottom](enclosure/Bottom.stl),[Top](enclosure/Top.stl))
3. Wirelessly flash firmware to sensor
4. Install sensor

## Setup ESPHome Config
ESPHome makes programming the esp microcontroller very easy - no Arduino, or sketch programming knowledge is needed. All the configuration is done via simple yaml configurations. We just need to simply follow the ESPHome documentation to assemble a configuration file that will send an MQTT (or other) alert to our home automation software whenever the microcontroller powers on. Don't worry, [Setting up ESPHome in HomeAssistant](https://esphome.io/guides/getting_started_hassio.html) is super easy.

### WiFi
The sensor will need to connect to a wifi access point. ESPHome makes this very easy with their [Wifi component](https://esphome.io/components/wifi.html). (See the sample [esphome_config.yaml](esphome_config.yaml) for a sample wifi configuration.)

### MQTT
The sensor will send an MQTT signal to a specific MQTT topic that is specified in the ESPHome configuration (see the [esphome_config.yaml](esphome_config.yaml) file for a sample MQTT configuration). In order for the sensor to be useful, you will need to configure an MQTT server (broker) to receive the message that will be sent by the sensor. HomeAssistant has a built-in MQTT server that is easy to configure and is the preferred MQTT broker that this project will use. Setup in HomeAssistant is simple:
1. [Setup Homeassistant](https://www.home-assistant.io/getting-started/) & enable the MQTT Add-On
2. [Create an MQTT sensor in Homeassistant](https://www.home-assistant.io/components/sensor.mqtt/). Be sure to use the same topic that you will use in the sketch.ino file.
3. Test your MQTT sensor using [an MQTT client](https://www.hivemq.com/blog/seven-best-mqtt-client-tools/). From your client MQTT tool, you will just need to make sure you are publishing to the same topic you've specified in your homeassistant setup.

## Flash your ESP8266 & Test the MQTT signal
Flashing an ESP8266 can be tricky to get right at first, but once you get it to work, you should be able to use the same process on each ESP8266 you'd like to flash. Here's my general process:
1. [Wire the ESP12E for flashing mode](http://cdn.srccodes.com/c/2017/02/57/6.png). (It is very helpful to use a breadboard along with the adapter mentioned above.)
2. Download [ESPHomeFlasher](https://github.com/esphome/esphome-flasher/releases)
3. Run through the ESPHome [initial setup wizard](https://esphome.io/guides/getting_started_hassio.html) and download the initial .bin firmware from the ESPHome initial setup wizard.
4. Flash the inital .bing firmeare to the esp12e chip using ESPHomeFlasher.
5. Use ESPHome to configure and re-flash the esp12e chip wirelessly. (See [esphome_config.yaml](esphome_config.yaml) for a sample ESPHome configuration file.)
6. Power on the sensor to test the MQTT signal.

## Credits & Inspiration

### ESPHome
ESPHome made the whole project incredibly easier and more enjoyable. I'm glad I found ESPHome while working on this project! Please check them out and support their work if you can.

### TheHookup
Original inspiration for this project came from [thehookup](https://github.com/thehookup/MQTT_Window_Sensors). His youtube videos below were helpful in visualizing how the sensor could work.
Wireless MQTT Window Sensors with the ESP-01:
Part 1: https://www.youtube.com/watch?v=BoYVr2UwWWg
Part 2: https://www.youtube.com/watch?v=2kLZ7DlP9KU

## Contributions
Any pull requests and/or improvements would be greatly appreciated.

## License
[MIT Licnese](license.txt)
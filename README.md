# Home Automation

There is a decent amount of home automation running at the Vondruska house. I've gotten quite a few inquries on everything that is running. I try to keep this document updated as much as possible.

I use [GitHub Actions](https://github.com/actions) to build and deploy as needed.


## Goals
No 100% reliance on cloud services unless absolutely necessary. Operations should function should function when an external connection is unavailable (internet down, cloud provider is having a bad day, etc.). That being said the cloud can provide convenience so during an external outage, it could be slightly more challenging to complete a task (i.e. Google Home would have problems when the internet is down but we can execute the task by using the Home Assistant web interface)

## Devices used

##### Note: this is likely not an exhaustive list

1. [Philips Hue](https://www.meethue.com)
1. [Ecobee 3](https://www.ecobee.com) with multiple [room sensors](https://www.ecobee.com/room-sensors/)
1. [Google Home and Google Home Mini](https://www.google.com/home)
1. [Aeotec Z-Stick Gen5](https://aeotec.com/z-wave-usb-stick)
1. Aeotec Z-Wave dry contact sensor
1. Z-Wave door sensor
1. [2GIG door/window/motion sensors](https://www.2gig.com/)
1. [Raspberry Pi 3 B+](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/)
1. [RTL-SDR Blog V3 R820T2 RTL2832U](https://www.amazon.com/gp/product/B011HVUEME)
1. [Tempy](#tempy)
1. Ecovacs Deebot N79S
1. [OpenGarage](https://opengarage.io/)

Obtained but not integrated:
1. GE Z-Wave Dimmer Switch
1. GE Z-Wave Switch
1. [Sonoff S20](https://www.itead.cc/smart-socket.html) flashed with [Sonoff-Tasmota](https://github.com/arendst/Sonoff-Tasmota)

## Services used

Here are all the services (and some modules) actively running to make it all works:

### Docker

Everything in this setup runs in a container. Running on a Raspberry Pi 3B+.

### [Home Assistant](https://github.com/home-assistant/home-assistant)

Home Assistant is the central point where all IoT endpoints integrate with. Essentially the IoT hub.

Configuration is source controlled and uses GitHub Actions to test the configuration before it gets pulled into instance. I'll open source the configuration soon.

### [Node-Red](https://nodered.org/)

All the Home Assistant events are piped into Node-Red allowing better visualization and more complex flows of events.

### [Mostquito](https://mosquitto.org/)

[MQTT](http://mqtt.org) broker. This allows many devices like the 2GIG sensors and freezer temperature to push messages to Home Assistant in a decoupled way.

### [345securitymqtt](https://github.com/vondruska/345securitymqtt)

Uses the [RTL-SDR USB dongle](https://www.amazon.com/gp/product/B011HVUEME) to detect and push 2GIG sensor events (door open/close, motion detector, etc) into MQTT.

More information at https://github.com/vondruska/345securitymqtt

### [Tempy](https://github.com/vondruska/tempy)

Tempy is a temperature monitor that I built using an ESP8266 and a DS18B20 temperature gauge. home built temperature monitor using an ESP8266 reporting the temperature via MQTT.

Read about that project at https://github.com/vondruska/tempy.

### [home-assistant-config-puller](https://github.com/vondruska/home-assistant-config-puller)

This is a service that will listen to webhooks allowing for continuious delivery of the Home Assistant configuration from GitLab.

Current work is based upon https://github.com/vondruska/docker-webhook-script-runner and https://hub.docker.com/r/vondruska/webhook-script-runner.

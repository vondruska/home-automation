# Home Automation

There is a decent amount of home automation running at the Vondruska house. This repo has an overview of all of it and (eventually) includes the `docker-compose.yaml` which brings it up.

I use GitLab CI to build and deploy as needed.


## Goals
No 100% reliance on cloud services unless absolutely necessary. Operations should function should function when an external connection is unavailable (internet down, cloud provider is having a bad day, etc.). That being said the cloud can provide convenience so during an external outage, it could be slightly more challenging to complete a task (i.e. Google Home would have problems when the internet is down but we can execute the task by using the Home Assistant web interface)

## Devices used

This may not be an exhaustive list

1. [Sonoff S20](https://www.itead.cc/smart-socket.html) flashed with [Sonoff-Tasmota](https://github.com/arendst/Sonoff-Tasmota)
1. [Philips Hue](https://www.meethue.com)
1. [Ecobee 3](https://www.ecobee.com) with multiple [room sensors](https://www.ecobee.com/room-sensors/)
1. [Google Home and Google Home Mini](https://www.google.com/home)
1. [Aeotec Z-Stick Gen5](https://aeotec.com/z-wave-usb-stick)
1. Aeotec Z-Wave dry contact sensor
1. Z-Wave door sensor
1. [Hard wired DSC PC1550 security system](https://images.google.com/?q=dsc+pc1550&gws_rd=ssl)
1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) w/ [Xbee shield](https://www.sparkfun.com/products/12847) + Xbee USB Explorer

Obtained but not integrated:
1. GE Z-Wave Dimmer Switch
1. GE Z-Wave Switch

## Services used

Here are all the services actively running to make it all works:

### Docker

Probably pretty obvious. Everything in this setup runs in a container.

### [Home Assistant](https://github.com/home-assistant/home-assistant)

Home Assistant is the central point where all endpoints integrate with and where all "triggers" are stored.

Configuration is source controlled and uses GitLab CI to test the configuration before it gets pulled into instance.

### [Mostquito](https://mosquitto.org/)

[MQTT](http://mqtt.org) broker. This allows the Sonoff outlets and the hard wired security system to push messages to Home Assistant in a decoupled way.

### [pc1550-xbee-bridge](../pc1550-xbee-bridge)

Node app listening for events from the hard wired home security system and publishes this via MQTTT for Home Assistant to act upon.

Changes in security system state are transmitted via Xbee by a Arduino connected directly to the security system logic board. [Picture of the wasps nest of wires with Arduino in the middle](https://i.imgur.com/SA4ujbsh.jpg).

This was all made possible by the fine work at https://github.com/dougkpowers/pc1550-interface.

The Arduino sketch source repository: https://github.com/vondruska/pc1550-arduino

### [home-assistant-config-puller](../home-assistant-config-puller)

This is a service that will listen to webhooks allowing for continuious delivery of the Home Assistant configuration from GitLab.

Current work is based upon https://github.com/vondruska/docker-webhook-script-runner and https://hub.docker.com/r/vondruska/webhook-script-runner.
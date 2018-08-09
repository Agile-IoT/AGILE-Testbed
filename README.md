# Using the AGILE Gateway on the IoT-Lab Testbed

### Available hardware

- Two AGILE Gateways (makers version, RaspberryPi) are deployed and available in the
[Saclay site testbed](https://www.iot-lab.info/deployment/saclay/)

- Each AGILE Gateway is connected via serial USB to an
[Atmel SAMR21](http://www.atmel.com/tools/atsamr21-xpro.aspx)
board which offers IEEE 802.15.4 wireless connection. The attached SAMR21 board
can either be used from the Pi as client node, or as border router (see below).

- Each AGILE Gateway is observable via a webcam streamed online at [agile-pi-2](http://demo-fit.saclay.inria.fr/agile-pi-2?action=stream)
and [agile-pi-3](http://demo-fit.saclay.inria.fr/agile-pi-3?action=stream)

- Also available on the same IoT-Lab site: hundreds of [IoT-Lab M3 nodes](https://www.iot-lab.info/hardware/m3/), with bare-metal access. 
IoT-Lab M3 nodes communicate via IEEE 802.15.4 wireless transcievers.

## Connecting to the AGILE Gateways

Each AGILE Gateway deployed on IoT-Lab is accessible online, as described below.

### Setup

Follow these [steps](https://github.com/Agile-IoT/AGILE-Testbed/wiki/Connecting-To-the-Agile-Gateways) to set up access to the AGILE Gateways on the IoT-Lab testbed.

### Accessing AGILE OS on the Gateways

AGILE software is installed and running on `agile-pi-2`, `agile-pi-3` and `agile-pi-5` gateways but cannot be
accessed directly from the web, for security reasons. 
For examples, to access AGILE OS running on agile-pi-2, you need to start an SSH tunnel
between your local computer and the AGILE gateway.

You can set this up from your command line on your local computer with the command:
```
    ssh -L8080:localhost:8080 -L8000:localhost:8000 \
        -L1880:localhost:1880 -L8083:localhost:8083 -L8086:localhost:8086 \
        -L3000:localhost:3000 -L2000:localhost:2000 agile-pi-2
```

At this point, you can access AGILE OS from your local computer, in a browser, at http://localhost:8000

## Connecting with RIOT nodes using IEEE 802.15.4

AGILE gateways `agile-pi-2` and `agile-pi-3` can use their attached Atmel SARM21 xpro board as a
IPv6 border router between the IEEE 802.15.4 wireless network used between IoT-Lab nodes and the
AGILE gateway, as described below.

### Setup

If you do not have one already, create your account on the
[IoT-Lab testbed](https://www.iot-lab.info/) and learn how to use it by
following [the online tutorials](https://www.iot-lab.info/tutorials/).

We assume basic [RIOT](http://riot-os.org) git and toolchain setup on your local computer.
For newbies, we recommend following the beginning of this [tutorial](https://github.com/RIOT-OS/Tutorials/blob/master/README.md) (you can stop after completing Task 1).

### Using the SAMR21 as a 6LoWPAN border router

Follow these [steps](https://github.com/Agile-IoT/AGILE-Testbed/wiki/6LoWPAN-Border-Router) to setup and start using an 6LoWPAN border router on a SAMR21 board
attached to the AGILE Gateway in IoT-Lab.

## Connecting with TI SensorTag node using BLE

This can only be performed on `agile-pi-5`.

### Connecting to Agile OS

1. Open an SSH tunnel to the gateways to map Agile OS local ports on your local host:
<pre>
    ssh -L8080:localhost:8080 -L8000:localhost:8000 \
            -L1880:localhost:1880 -L8083:localhost:8083 -L8086:localhost:8086 \
            -L3000:localhost:3000 -L2000:localhost:2000 agile-pi-5
</pre>

### Connecting the SensorTag in AgileOS

1. Ensure the SensorTag is up and listening to BLE incoming connections
2. From you host, login AgileOS at http://localhost:8000
3. Open the device manager, register and connect to the SensorTag device

### Connect the gateway to Eclipse MQTT broker

1. Open NodeRed and import the preconfigured `Library/test_sensortag_mqtt` flow
2. Verify everything is connected correctly (websockets for sensortag and mqtt clients to iot.eclipse.org)
3. Use Mosquitto MQTT client to subscribe to available topics:
<pre>
    mosquitto_sub -h iot.eclipse.org -t saclay/agile/sensors/temperature
    mosquitto_sub -h iot.eclipse.org -t saclay/agile/sensors/humidity
</pre>

## Upcoming support

In the near-future, we plan to :
* instead of SAMR21, use the Libelium Hat + [802.15.4 Shield](https://www.cooking-hacks.com/xbee-pro-802-15-4-sma-module), or an [OpenLabs](http://openlabs.co/OSHW/Raspberry-Pi-802.15.4-radio) dongle
on the AGILE gateways following these [steps](https://github.com/RIOT-Makers/wpan-raspbian/wiki/Create-a-generic-Raspbian-image-with-6LoWPAN-support)
* Switch from Raspbian to ResinOS

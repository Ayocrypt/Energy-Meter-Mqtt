# ADW300 Energy Meter MQTT Setup

This guide provides detailed instructions on how to set up and use MQTT with the ADW300 Energy Meter.

## Table of Contents
1. [Overview](#overview)
2. [Components Needed](#components-needed)
3. [Setting up the WiFi](#setting-up-the-wifi)
4. [Configuring MQTT](#configuring-mqtt)
5. [Python Examples](#python-examples)
6. [Additional Information](#additional-information)

## Overview
The ADW300 Energy Meter uses MQTT to report data. This guide covers the necessary steps to configure and use MQTT for data communication.

## Components Needed
- ADW300 Energy Meter
- WiFi Network
- MQTT Broker (e.g., Mosquitto)
- Computer with Python installed
- MQTT client library for Python (e.g., Paho MQTT)

## Setting up the WiFi
1. Power on the ADW300 Energy Meter.
2. Connect to the device's WiFi access point (AP) using a computer or smartphone.
3. Open a web browser and navigate to the default IP address of the device (e.g., `192.168.4.1`).
4. Enter the WiFi settings and connect the device to your WiFi network.

## Configuring MQTT
### MQTT Topic Specification
MQTT topics follow the format:

```python
<target object>/<vendor identification>/<device type>/<protocol type>/<data type>/<group>
```


### Attribute Specifications
- **Target Object**: Manufacturer's identification code (hexadecimal numbers)
- **Device Type**: Type of device (e.g., meter)
- **Protocol Type**: Type of protocol (e.g., json-v2)
- **Data Type**: Type of data (e.g., analog)
- **Group**: Group number for load balancing (0 for <2000 devices)

### Connection Management
- **Server**: IP or URL of the MQTT broker
- **User Name**: Your MQTT username
- **Password**: Your MQTT password
- **KeepAlive**: Keep alive interval (1min ~ 5min)
- **CleanSession**: Set to `1`

### Data Reporting
- **Topic Format**: `platform/<vendor_id>/<device_type>/json-v2/analog/<group>`
- **QoS**: `1`

### Example Topic

```bash
platform/acrel/meter/json-v2/analog/0000
```


### Configurable Data Items
| Data Item Name          | Type       | Unit | Serial Number | Description                       |
|-------------------------|------------|------|---------------|-----------------------------------|
| device ID               | String     |      | 0             | Device unique serial number       |
| A phase voltage         | Voltage    | V    | 1             | A-phase voltage                   |
| B phase voltage         | Voltage    | V    | 2             | B-phase voltage                   |
| C phase voltage         | Voltage    | V    | 3             | C-phase voltage                   |
| AB line voltage         | Voltage    | V    | 4             | AB voltage                        |
| BC line voltage         | Voltage    | V    | 5             | BC voltage                        |
| CA line voltage         | Voltage    | V    | 6             | CA voltage                        |
| A phase current         | Current    | A    | 7             | A phase current                   |
| B phase current         | Current    | A    | 8             | B phase current                   |
| C phase current         | Current    | A    | 9             | C phase current                   |
| Phase A active power    | Active Power | kW  | 10            | A-phase active power              |
| B phase active power    | Active Power | kW  | 11            | B-phase active power              |
| C phase active power    | Active Power | kW  | 12            | C-phase active power              |
| Total active power      | Active Power | kW  | 13            | Total active power                |
| Phase A reactive power  | Reactive Power | kvar | 14         | A-phase reactive power            |
| B phase reactive power  | Reactive Power | kvar | 15         | B-phase reactive power            |
| C phase reactive power  | Reactive Power | kvar | 16         | C-phase reactive power            |
| Total reactive power    | Reactive Power | kvar | 17         | Total reactive power              |
| Phase A apparent power  | Apparent Power | kVA | 18         | A-phase apparent power            |
| B phase apparent power  | Apparent Power | kVA | 19         | B-phase apparent power            |
| C phase apparent power  | Apparent Power | kVA | 20         | C-phase apparent power            |
| Total apparent power    | Apparent Power | kVA | 21         | Total apparent power              |
| A phase power factor    | Power Factor |   | 22             | A-phase power factor              |
| B phase power factor    | Power Factor |   | 23             | B-phase power factor              |
| C phase power factor    | Power Factor |   | 24             | C-phase power factor              |
| Total power factor      | Power Factor |   | 25             | Total power factor                |
| Frequency               | Frequency  | Hz   | 26            | Frequency                         |
| Signal strength         | Working Conditions | dBm | 27     | Signal strength                   |
| Forward active total electric energy indication value | Electric Energy Indication | kWh | 28 | Indication value of total positive active energy |
| Reverse active total electric energy indication value | Electric Energy Indication | kWh | 29 | Indication value of total reverse active energy |
| Total forward reactive energy indication value | Electric Energy Indication | kvarh | 30 | Indication value of total positive reactive energy |
| Reverse reactive power total energy indication value | Electric Energy Indication | kvarh | 31 | Indication value of total reverse reactive energy |
| Current active power demand | Demand | kW | 32            | Current active demand             |
| ICCID                   | Working Conditions |   | 33          | Force startup to upload and only upload once |
| PT                      | Voltage Transformation Ratio |   | 34 | Voltage transformation ratio      |
| CT                      | Current Transformation Ratio |   | 35 | Current transformation ratio      |
| A phase temperature     | Temperature | ℃   | 36            | A phase temperature               |
| B phase temperature     | Temperature | ℃   | 37            | B phase temperature               |
| C phase temperature     | Temperature | ℃   | 38            | C phase temperature               |
| N phase temperature     | Temperature | ℃   | 39            | N phase temperature               |
| Residual current        | Leakage Current | A | 40            | Leakage current                   |
| 4-way DI status         | DI Status | bit 0: DI1, bit1: DI2, bit2: DI3, bit3: DI4 | 41 | 4DI status                          |

## Python Examples
### Installing the Paho MQTT Client
Install the Paho MQTT client library using pip:
```bash
pip install paho-mqtt
```


# Python examples
## Publishing Data
```python
import paho.mqtt.client as mqtt
import json

# MQTT Broker settings
broker = "broker.hivemq.com"
port = 1883
topic = "platform/acrel/meter/json-v2/analog/0000"

# Data payload
payload = {
    "tp": 1622467890123,
    "ID": 1,
    "val": 230.0
}

# Create MQTT client
client = mqtt.Client()
client.username_pw_set("username", "password")

# Connect to MQTT broker
client.connect(broker, port)

# Publish data
client.publish(topic, json.dumps(payload))

# Disconnect
client.disconnect()
```
# Subscribing to Data

```python
import paho.mqtt.client as mqtt

# MQTT Broker settings
broker = "broker.hivemq.com"
port = 1883
topic = "platform/acrel/meter/json-v2/analog/0000"

# Define on_message callback
def on_message(client, userdata, message):
    print(f"Received message: {message.payload.decode()} on topic {message.topic}")

# Create MQTT client
client = mqtt.Client()
client.username_pw_set("username", "password")

# Connect to MQTT broker
client.connect(broker, port)

# Subscribe to topic
client.subscribe(topic)

# Assign on_message callback
client.on_message = on_message

# Start MQTT client
client.loop_forever()
```


# ADW300-WIFI Energy Meter Setup Guide

This guide provides step-by-step instructions to connect and configure your ADW300-WIFI energy meter.

## 1. List of Components Needed

### Hardware Parts
- A computer (Windows)
- The ADW300-WIFI meter
- A USB to RS485 converter module
- Screwdriver and some wires

### Software Parts
- ADW300-WIFI debugging software
- USB to RS485 driver (if it's your first time using it)

## 2. Setting Up the WiFi

### Connect USB to RS485 and the Meter
1. Connect the + sign on the module to port 21 on the meter.
2. Connect the - sign on the module to port 22 on the meter.
3. Power up the meter.

### Open the Debugging Software
1. Run the executable file in the debugging software folder.
2. Select the appropriate language.

### Select the Correct Port
1. Choose the correct COM port that your USB to RS485 converter is connected to.

### Broadcast Read Address
1. Click “Broadcast Read Address”. If the device communicates normally, you will see the setting interface.

### Set WiFi Parameters
1. Click the WiFi set button on the right.
2. Enter your WiFi username and password, then click the “Set” button.

### Set the Connection Method
1. For DHCP connection, check the DHCP box.
2. For a static IP connection, enter the parameters manually and ensure to click “Set”.

## 3. Additional Information to Ensure Proper Functioning

### Debugging Method (Modbus-TCP)
1. **Select Modbus-TCP**:
   - Click “Modbus-Tcp”.
2. **Choose Correct Mode**:
   - Read the current mode and choose the required mode, then click “Set”.
3. **Set Connection Parameters**:
   - For server mode, no need to set gateway IP and port.
   - For client mode, set the IP or domain and the port, then click “Set”.

### Debugging Method (MQTT)
1. **Select MQTT**:
   - Click “MQTT (basic)”.
2. **Set MQTT Parameters**:
   - Enter IP or domain name of your server, port number, upload interval, MQTT username, and password.
   - Set the UP topic and MQTT QOS to 1, then click “Set”.

### Debugging Method (HWIOT)
1. **Select HWIOT (Oversea)**:
   - Click “EIOT (Oversea)”.
2. **Set Parameters**:
   - Enter IP or domain name, port number, upload interval, country, UTC, serial number (14-bit code), QOS (set to 1), and datanum (set to 43).


# Sound Level Monitor with ESP8266 and ThingSpeak

This project is designed to monitor sound levels using an ESP8266 microcontroller and a sound sensor. The data is read from the sensor, converted to decibels (dB), and sent to ThingSpeak for visualization and analysis.

## Table of Contents

- [Introduction](#introduction)
- [Components](#components)
- [Setup](#setup)
- [Usage](#usage)

## Introduction

This project uses an ESP8266 microcontroller to measure sound levels with a sound sensor. The measured data is sent to ThingSpeak, an IoT analytics platform, where it can be visualized in real-time. The data includes the current sound level and the average sound level over the last minute.

## Components

- ESP8266 Microcontroller
- Sound Sensor (e.g., Analog Sound Sensor)
- Breadboard and Jumper Wires
- Power Supply (e.g., USB cable)

## Setup

1. **Hardware Setup**:
   - Connect the sound sensor to the ESP8266:
     - VCC to 3.3V
     - GND to GND
     - Analog Output to A0

2. **Software Setup**:
   - Install the Arduino IDE.
   - Add the ESP8266 Board to the Arduino IDE:
     - Open the Arduino IDE.
     - Go to `File` > `Preferences`.
     - In the "Additional Board Manager URLs" field, add: `http://arduino.esp8266.com/stable/package_esp8266com_index.json`
     - Go to `Tools` > `Board` > `Boards Manager`.
     - Search for "ESP8266" and install the latest version.
   - Install the following libraries from the Library Manager (`Sketch` > `Include Library` > `Manage Libraries`):
     - `ESP8266WiFi`
     - `ThingSpeak`

3. **ThingSpeak Setup**:
   - Create a ThingSpeak account and create a new channel.
   - Note the Channel ID and Write API Key.

4. **Secrets File**:
   - Create a `secrets.h` file in your project directory.
   - Add your WiFi credentials and ThingSpeak details to the `secrets.h` file:
     ```cpp
     #define SECRET_SSID "your_SSID_here"
     #define SECRET_PASS "your_password_here"
     #define SECRET_CH_ID your_channel_id_here  // Numeric ID
     #define SECRET_WRITE_APIKEY "your_write_API_key_here"
     ```

## Usage

1. **Upload the Code**:
   - Open the Arduino IDE.
   - Load the provided code into the Arduino IDE.
   - Ensure the correct board and port are selected (`Tools` > `Board` > `ESP8266`).
   - Upload the code to the ESP8266.

2. **Monitor the Serial Output**:
   - Open the Serial Monitor (`Tools` > `Serial Monitor`).
   - Set the baud rate to 115200.
   - Observe the connection status and sound level readings.

3. **View Data on ThingSpeak**:
   - Go to your ThingSpeak channel.
   - Observe the real-time data updates and visualizations.

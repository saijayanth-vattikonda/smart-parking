# Smart Parking System using ESP8266

## Overview

The Smart Parking System is designed to provide an optimal solution for parking problems in crowded cities. Utilizing ESP8266, RFID technology, and IR sensors, this project aims to reduce the time spent searching for parking slots, thus decreasing air and noise pollution and traffic congestion.

## Features

- Real-time monitoring of parking slots.
- Automated gate control using IR sensors and servo motors.
- Integration with Adafruit IO for data visualization.

## Components

### Hardware
- NodeMCU ESP8266
- IR Sensor (4 units)
- Servo Motor (2 units)

### Online Services
- Adafruit IO

## Getting Started

### Prerequisites
- Arduino IDE
- NodeMCU ESP8266 board installed in Arduino IDE
- Adafruit IO account

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/smart-parking-system.git
   cd smart-parking-system
   ```

2. Open the Arduino IDE and install the required libraries:
   - ESP8266WiFi
   - Servo
   - NTPClient
   - Adafruit MQTT Library

3. Update the WiFi and Adafruit IO credentials in the code:
   ```cpp
   const char *ssid = "Your_SSID";
   const char *pass = "Your_PASSWORD";
   #define MQTT_NAME "Your_Adafruit_IO_Username"
   #define MQTT_PASS "Your_Adafruit_IO_Key"
   ```

4. Connect the hardware components as per the circuit diagram provided in the documentation.

5. Upload the code to the NodeMCU ESP8266 board.

## Usage

Once the system is set up, it will automatically detect available parking slots and update the status on the Adafruit IO dashboard. Users can view the parking status in real-time on their mobile devices or computers.

## Circuit Diagram

Refer to the detailed circuit diagram in the `Report` file.

## Code

Here's a snippet of the main code:
```cpp
#include <ESP8266WiFi.h>
#include <Servo.h>
#include <NTPClient.h>
#include <WiFiUdp.h>
#include "Adafruit_MQTT.h"
#include "Adafruit_MQTT_Client.h"

const char *ssid = "Your_SSID";
const char *pass = "Your_PASSWORD";

#define MQTT_SERV "io.adafruit.com"
#define MQTT_PORT 1883
#define MQTT_NAME "Your_Adafruit_IO_Username"
#define MQTT_PASS "Your_Adafruit_IO_Key"

WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, "pool.ntp.org", 19800, 60000);

Servo myservo;
Servo myservos;

int carEnter = D1;
int carExited = D6;
int slot2 = D3;
int slot1 = D2;
int count = 0;
int CLOSE_ANGLE = 80;
int OPEN_ANGLE = 0;
int hh, mm, ss;
int pos;
int pos1;

String h, m, EntryTimeSlot1, ExitTimeSlot1, EntryTimeSlot2, ExitTimeSlot2;
boolean entrysensor, exitsensor, s1, s2;

boolean s1_occupied = false;
boolean s2_occupied = false;

WiFiClient client;
Adafruit_MQTT_Client mqtt(&client, MQTT_SERV, MQTT_PORT, MQTT_NAME, MQTT_PASS);

Adafruit_MQTT_Subscribe EntryGate = Adafruit_MQTT_Subscribe(&mqtt, MQTT_NAME "/f/EntryGate");
Adafruit_MQTT_Subscribe ExitGate = Adafruit_MQTT_Subscribe(&mqtt, MQTT_NAME "/f/ExitGate");

Adafruit_MQTT_Publish CarsParked = Adafruit_MQTT_Publish(&mqtt, MQTT_NAME "/f/CarsParked");
Adafruit_MQTT_Publish EntrySlot1 = Adafruit_MQTT_Publish(&mqtt, MQTT_NAME "/f/EntrySlot1");
Adafruit_MQTT_Publish ExitSlot1 = Adafruit_MQTT_Publish(&mqtt, MQTT_NAME "/f/ExitSlot1");
```

## Contributing

Contributions are welcome! Please fork the repository and create a pull request with your changes.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contact

For any queries or support, please contact [Vattikonda Sai Jayanth](mailto:vattikondasaijayanth@gmail.com).
---

Feel free to modify this README file according to your needs.

<div align="center">

# 🚗 Intelligent Vehicle Monitoring & Alert System

### **Distributed CAN-Based Vehicle Health Monitoring with ESP8266 Email Alerts**

[![Platform](https://img.shields.io/badge/Platform-ESP8266%20%7C%20Arduino-blue?style=for-the-badge)](https://www.espressif.com/)

[![Protocol](https://img.shields.io/badge/Protocol-CAN%202.0-orange?style=for-the-badge)]()

[![Controller](https://img.shields.io/badge/CAN%20Controller-MCP2515-red?style=for-the-badge)]()

[![Language](https://img.shields.io/badge/Language-Embedded%20C%2FC%2B%2B-brightgreen?style=for-the-badge)](https://www.arduino.cc/)

[![Connectivity](https://img.shields.io/badge/Connectivity-Wi--Fi-blueviolet?style=for-the-badge)]()

[![Alerts](https://img.shields.io/badge/Alerts-Email-critical?style=for-the-badge)]()

</div>

---

# 🚘 Project Overview

Modern vehicles contain multiple sensors distributed across different subsystems. A practical monitoring system therefore needs a reliable communication mechanism to collect sensor information, process it centrally and notify the user when abnormal conditions occur.

The **Intelligent Vehicle Monitoring & Alert System** is a distributed embedded system designed around this concept.

Multiple sensor nodes collect vehicle-related parameters and communicate with a central controller through the **Controller Area Network (CAN)** protocol.

The central **ESP8266 gateway** receives the CAN frames, decodes the sensor information, displays the latest values locally on an LCD and uses Wi-Fi to send monitoring information through email.

### The complete concept:

```text
Sensors
   ↓
Distributed CAN Nodes
   ↓
MCP2515 CAN Controllers
   ↓
CAN Bus
   ↓
ESP8266 Gateway
   ↓
┌───────────────┬────────────────┐
│               │                │
▼               ▼                ▼
LCD          Wi-Fi          Email Alert
Display      Network        Notification
```

The project demonstrates how **automotive-style distributed communication** can be combined with **IoT connectivity** for remote monitoring.

---

# 🎯 Problem Statement

A vehicle contains several parameters that need to be monitored continuously.

Examples include:

* 🌡️ Temperature
* ⛽ Fuel level
* 💡 Ambient/light condition
* 🚨 Obstacle or presence status

Monitoring all these parameters using a single controller can make wiring complex as the number of sensors increases.

A distributed architecture provides a better approach:

```text
Sensor Group 1 ──┐
                  │
Sensor Group 2 ──┼──→ CAN Bus ──→ Central Gateway
                  │
Sensor Group 3 ──┘
```

The CAN protocol allows multiple nodes to communicate over a shared bus while the ESP8266 acts as the gateway between the embedded vehicle network and the Internet.

---

# 💡 What Makes This Project Different?

This project is not simply a sensor-reading application.

It demonstrates a **three-layer embedded architecture**:

```text
┌─────────────────────────────────────┐
│          VEHICLE SENSOR LAYER       │
│                                     │
│ Temperature | Fuel | IR | Light     │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│       VEHICLE COMMUNICATION LAYER   │
│                                     │
│          CAN + MCP2515              │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│        CONNECTIVITY LAYER            │
│                                     │
│        ESP8266 + Wi-Fi              │
│             ↓                       │
│        Email Notification           │
└─────────────────────────────────────┘
```

This makes the project a useful example of integrating:

**Embedded Systems + CAN + Sensors + IoT + Remote Alerts**

---

# 🏗️ Complete System Architecture

```text
                    ┌───────────────────────┐
                    │    VEHICLE SENSORS    │
                    └───────────┬───────────┘
                                │
              ┌─────────────────┴─────────────────┐
              │                                   │
              ▼                                   ▼
       ┌───────────────┐                  ┌───────────────┐
       │   SENSOR      │                  │   SENSOR      │
       │   NODE 1      │                  │   NODE 2      │
       │               │                  │               │
       │ Temperature   │                  │ IR Sensor     │
       │ Fuel Level    │                  │ LDR           │
       └───────┬───────┘                  └───────┬───────┘
               │                                  │
               ▼                                  ▼
          ┌─────────┐                        ┌─────────┐
          │ MCP2515 │                        │ MCP2515 │
          │   CAN   │                        │   CAN   │
          └────┬────┘                        └────┬────┘
               │                                  │
               └──────────────┬───────────────────┘
                              │
                         CAN BUS
                       250 kbps
                              │
                              ▼
                    ┌──────────────────┐
                    │     ESP8266      │
                    │  Central Gateway │
                    └────────┬─────────┘
                             │
                ┌────────────┼────────────┐
                │            │            │
                ▼            ▼            ▼
              I²C           Wi-Fi       Processing
              LCD             │
                              ▼
                        Email Alert
```

---

# 🔌 Sensor Node Architecture

The project uses distributed sensor nodes rather than connecting every sensor directly to the gateway.

### Node 1

```text
Temperature Sensor
       +
Fuel-Level Sensor
       │
       ▼
   Sensor Node
       │
       ▼
    MCP2515
       │
       ▼
     CAN Bus
```

### Node 2

```text
IR Sensor
    +
LDR
    │
    ▼
Sensor Node
    │
    ▼
 MCP2515
    │
    ▼
  CAN Bus
```

The current repository implements CAN communication through the MCP2515 controller and transmits the sensor information using separate CAN message identifiers.

---

# 📡 Why CAN Protocol?

**Controller Area Network (CAN)** is widely used for communication between distributed controllers and sensors.

Instead of requiring a separate communication line between every sensor and the main controller, CAN provides a shared communication bus.

### Without CAN

```text
Sensor 1 ─────────→ Controller
Sensor 2 ─────────→ Controller
Sensor 3 ─────────→ Controller
Sensor 4 ─────────→ Controller
```

This can lead to increased wiring complexity.

### With CAN

```text
Sensor 1 ──┐
Sensor 2 ──┤
Sensor 3 ──┼──→ CAN BUS ──→ Gateway
Sensor 4 ──┘
```

This is one of the main reasons CAN is useful for distributed embedded systems.

---

# 🧠 MCP2515 CAN Controller

The **MCP2515** is used as the CAN controller between the microcontroller and the CAN bus.

The communication path is:

```text
Microcontroller
      │
      │ SPI
      ▼
  MCP2515
      │
      │ CAN
      ▼
   CAN BUS
```

The project configures the MCP2515 for:

```text
CAN Speed : 250 kbps
Clock     : 8 MHz
```

The ESP8266 gateway initializes the CAN controller in normal mode.

---

# 🆔 CAN Message Architecture

The system separates sensor information using CAN message identifiers.

### CAN ID `0x01`

Carries:

```text
Temperature
     +
Fuel Level
```

The gateway receives a 6-byte payload and reconstructs the values.

```text
CAN ID 0x01

┌───────────────┬───────────────┐
│ Temperature   │  Fuel Level   │
│    4 bytes    │    2 bytes    │
└───────────────┴───────────────┘
```

### CAN ID `0x02`

Carries:

```text
IR Status
     +
Light Level
```

```text
CAN ID 0x02

┌─────────────┬─────────────────┐
│ IR Status   │   Light Level   │
│   1 byte    │     2 bytes     │
└─────────────┴─────────────────┘
```

The gateway decodes these two message types before displaying and transmitting the information.

---

# 🌡️ Temperature Monitoring

The temperature sensor provides an analog signal to the sensor node.

The node converts the analog reading into a temperature value and packs the result into the CAN payload.

```text
Temperature Sensor
        │
        ▼
 Analog Measurement
        │
        ▼
 Sensor Node
        │
        ▼
   CAN Message
     ID 0x01
        │
        ▼
    ESP8266
        │
        ├──→ LCD
        │
        └──→ Email
```

---

# ⛽ Fuel-Level Monitoring

Fuel level is monitored as part of the vehicle sensor layer.

The sensor information is converted into a usable level value and transmitted through the CAN network.

```text
Fuel-Level Sensor
       │
       ▼
Level Measurement
       │
       ▼
Sensor Node
       │
       ▼
CAN ID 0x01
       │
       ▼
ESP8266
       │
       ├──→ LCD
       │
       └──→ Email
```

The gateway represents the received fuel value as a percentage.

---

# 💡 LDR-Based Light Monitoring

The LDR is used to obtain information about ambient light conditions.

The sensor node converts the analog reading into a percentage representation.

```text
LDR
 │
 ▼
Analog Reading
 │
 ▼
Mapped Light Level
 │
 ▼
CAN ID 0x02
 │
 ▼
ESP8266
```

The gateway then displays the received light level.

The repository's second sensor node reads the LDR through an analog input and maps the result to a 0–100 representation.

---

# 🚨 IR-Based Monitoring

The IR sensor provides a digital status signal.

The sensor node reads the digital input and packs the result into the CAN message.

```text
IR Sensor
    │
    ▼
Digital Status
    │
    ▼
CAN ID 0x02
    │
    ▼
ESP8266
```

The received status can then be displayed and included in the remote monitoring information.

---

# 📺 Local LCD Monitoring

The ESP8266 gateway uses an I²C LCD to display the latest received sensor information.

The current gateway code initializes a **16×2 I²C LCD at address `0x27`**.

The display concept is:

```text
┌────────────────┐
│ T:28.5C F:75%  │
│ L:62% IR:0     │
└────────────────┘
```

This gives the user local visibility without requiring a smartphone or computer.

---

# 🌐 ESP8266 as IoT Gateway

The ESP8266 is the central communication gateway.

It has two major responsibilities:

### 1. Vehicle Network Interface

```text
CAN Bus
   ↓
MCP2515
   ↓
ESP8266
```

### 2. Internet Interface

```text
ESP8266
   ↓
Wi-Fi
   ↓
Internet
   ↓
Email Service
```

Therefore:

```text
              ESP8266
             /       \
            /         \
       Vehicle        Internet
       Network         Network
          │               │
          ▼               ▼
        CAN             Wi-Fi
```

This gateway architecture is one of the key concepts demonstrated by the project.

---

# 📧 Email Alert System

The ESP8266 uses Wi-Fi and an SMTP-based email client to transmit monitoring information.

The project uses:

```text
SMTP
   │
   ▼
Gmail SMTP Server
   │
   ▼
Email Recipient
```

The gateway prepares an email containing the current sensor information.

The implemented email content includes:

* Temperature
* Fuel level
* Light level
* IR status

The source code uses the ESP Mail Client library and Gmail SMTP configuration.

---

# 🔄 Complete Data Pipeline

```text
                    VEHICLE
                      │
          ┌───────────┴───────────┐
          │                       │
          ▼                       ▼
      Sensor Node 1          Sensor Node 2
          │                       │
     Temperature              IR Sensor
     Fuel Level                  LDR
          │                       │
          ▼                       ▼
       MCP2515                 MCP2515
          │                       │
          └──────────┬────────────┘
                     │
                     ▼
                   CAN BUS
                     │
                     ▼
               MCP2515 Gateway
                     │
                     ▼
                  ESP8266
                     │
             ┌───────┴───────┐
             │               │
             ▼               ▼
           LCD              Wi-Fi
                             │
                             ▼
                        Email Service
                             │
                             ▼
                         User Inbox
```

---

# 🧩 Software Flow

```text
                 START
                   │
                   ▼
          Initialize ESP8266
                   │
                   ▼
              Connect Wi-Fi
                   │
                   ▼
             Initialize CAN
                   │
                   ▼
            Set CAN Normal Mode
                   │
                   ▼
             Wait for CAN Data
                   │
                   ▼
          Receive CAN Message
                   │
             ┌─────┴─────┐
             │           │
          ID 0x01      ID 0x02
             │           │
             ▼           ▼
       Temperature     IR Status
       Fuel Level      Light Level
             │           │
             └─────┬─────┘
                   │
                   ▼
             Update LCD
                   │
                   ▼
          Prepare Monitoring Data
                   │
                   ▼
             Email Service
                   │
                   ▼
                Repeat
```

---

# 📨 CAN Data Decoding

The gateway reconstructs multi-byte values from the CAN payload.

For example:

```text
CAN ID 0x01
        │
        ├── Bytes 0–3 → Temperature
        │
        └── Bytes 4–5 → Fuel Level
```

and:

```text
CAN ID 0x02
        │
        ├── Byte 0    → IR Status
        │
        └── Bytes 1–2 → Light Level
```

This demonstrates an important embedded concept:

> **Sensor data must be serialized at the transmitting node and correctly reconstructed at the receiving node.**

---

# ⏱️ Periodic Monitoring

The sensor nodes transmit their data periodically rather than sending it only once.

The current node implementations use approximately one-second transmission intervals.

The gateway also uses a time-based mechanism for periodic email transmission.

Conceptually:

```text
Sensor Data
    ↓
Continuous CAN Monitoring
    ↓
Latest Values Stored
    ↓
Periodic Email
    ↓
Updated Vehicle Status
```

---

# 📁 Project Structure

The current repository is organized into three main firmware files:

```text
Automatic-Vehicle-Monitoring-system/
│
├── ESP8266.c
│
├── slave.c
│
├── slave2.c
│
└── README.md
```

### `slave.c`

Responsible for the second sensor node containing:

* IR sensor
* LDR
* MCP2515 CAN controller
* CAN message transmission

The node transmits data using CAN ID `0x02`.

### `slave2.c`

Responsible for the other sensor node containing:

* Temperature sensing
* Fuel/water-level sensing concept
* MCP2515 CAN controller
* CAN message transmission

The node uses CAN ID `0x01`.

### `ESP8266.c`

Acts as the central gateway.

It handles:

* Wi-Fi connection
* CAN reception
* CAN data decoding
* LCD display
* Sensor data storage
* Email communication

---

# 🛠️ Hardware & Software Stack

| Category                    | Technology                     |
| --------------------------- | ------------------------------ |
| Main Gateway                | ESP8266                        |
| CAN Controller              | MCP2515                        |
| CAN Protocol                | CAN                            |
| CAN Speed                   | 250 kbps                       |
| CAN Controller Clock        | 8 MHz                          |
| Local Display               | 16×2 I²C LCD                   |
| Temperature Sensing         | Analog temperature sensor      |
| Fuel-Level Sensing          | Ultrasonic / level sensing     |
| Light Sensing               | LDR                            |
| Presence / Obstacle Sensing | IR sensor                      |
| Wireless Communication      | Wi-Fi                          |
| Remote Alert                | Email / SMTP                   |
| Programming                 | Embedded C/C++                 |
| Development Platform        | Arduino-compatible environment |

---

# 🔗 Interface Architecture

### SPI

The MCP2515 communicates with the microcontroller through SPI.

```text
ESP8266 / Arduino
       │
       │ SPI
       ▼
   MCP2515
       │
       │ CAN
       ▼
    CAN BUS
```

### I²C

The ESP8266 uses I²C to communicate with the LCD.

```text
ESP8266
   │
   │ I²C
   ▼
16×2 LCD
```

### Wi-Fi

The ESP8266 provides the Internet connection.

```text
ESP8266
   │
   ▼
Wi-Fi
   │
   ▼
Internet
   │
   ▼
SMTP Server
```

This project therefore demonstrates **three different communication interfaces**:

```text
SPI  → MCP2515
I²C  → LCD
Wi-Fi → Remote Alert
```

---

# 🧠 Embedded Concepts Demonstrated

This project provides practical exposure to:

### CAN Communication

* CAN bus architecture
* CAN message identifiers
* CAN payload packing
* CAN payload decoding
* MCP2515 configuration
* Multi-node communication

### Microcontrollers

* ESP8266
* Arduino-compatible controllers
* GPIO
* Analog inputs
* Digital inputs

### Communication Interfaces

* SPI
* I²C
* CAN
* Wi-Fi
* SMTP

### Sensors

* Temperature sensor
* Fuel-level sensing
* LDR
* IR sensor

### Embedded Programming

* Sensor acquisition
* Data serialization
* Data decoding
* Periodic processing
* Peripheral interfacing
* Event/status monitoring

---

# 📚 Key Learning Outcomes

Through this project, I gained practical understanding of:

* How distributed embedded systems communicate
* How CAN nodes exchange sensor information
* How CAN identifiers separate different data sources
* How MCP2515 interfaces with a microcontroller
* How sensor values are packed into CAN frames
* How received CAN data is decoded
* How an ESP8266 can act as an IoT gateway
* How SPI and I²C are used for peripheral communication
* How Wi-Fi can extend a local embedded system to remote monitoring
* How email can be integrated for remote alerts

---

# 🎓 Why This Project Is Relevant to Embedded Systems

This project demonstrates a complete embedded communication chain:

```text
         SENSOR ACQUISITION
                 ↓
        EMBEDDED PROCESSING
                 ↓
         CAN COMMUNICATION
                 ↓
          DATA AGGREGATION
                 ↓
        ESP8266 GATEWAY
                 ↓
          Wi-Fi CONNECTIVITY
                 ↓
        REMOTE NOTIFICATION
```

Instead of treating every sensor as an independent project, the system demonstrates how multiple embedded nodes can cooperate as a **distributed monitoring network**.

---

# 🚀 Possible Future Enhancements

The architecture can be extended into a more advanced vehicle telematics platform.

### 📍 GPS Tracking

Add GPS to transmit:

```text
Vehicle Location
       ↓
ESP8266
       ↓
Cloud Dashboard
```

### 📊 Web Dashboard

Instead of email-only alerts:

```text
CAN Data
   ↓
ESP8266
   ↓
Cloud
   ↓
Web Dashboard
```

### 📱 Mobile Notifications

Generate instant notifications when abnormal conditions are detected.

### 💾 Data Logging

Store historical sensor data for:

* Vehicle maintenance
* Trend analysis
* Fault investigation

### 🔋 Battery Monitoring

Add:

* Battery voltage
* Charging status
* Low-voltage alert

### 🚨 Advanced Fault Detection

Combine multiple sensor values to detect potential vehicle faults.

### 🛰️ GPS + IoT Telematics

A future version could provide:

```text
Vehicle
  │
  ├── Sensor Data
  ├── CAN Data
  ├── GPS
  └── Alerts
        │
        ▼
     ESP8266
        │
        ▼
      Cloud
        │
   ┌────┴────┐
   ▼         ▼
Mobile      Web
```

---

# ⚠️ Security Note

The gateway source contains placeholders for:

* Wi-Fi credentials
* SMTP credentials
* Email addresses

These values should **never contain real passwords or private authentication credentials in a public repository**.

Use placeholders such as:

```cpp
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

#define AUTHOR_EMAIL "YOUR_EMAIL";
#define AUTHOR_PASSWORD "YOUR_APP_PASSWORD";
```

For production IoT systems, credentials should be stored securely rather than hard-coded into publicly accessible source code.

---

# ⚠️ Hardware Safety

Some vehicle and electrical interfaces may involve hazardous voltages or moving components.

For development and demonstration:

* Use safe low-voltage test setups.
* Verify sensor wiring before powering the system.
* Use appropriate CAN transceivers and termination.
* Avoid directly connecting microcontroller pins to automotive electrical systems without proper protection.
* Use suitable voltage regulation and transient protection for real vehicle deployment.

---

# 🧪 Testing Strategy

The system can be tested layer by layer.

### Test 1 — Individual Sensors

Verify:

```text
Temperature → Correct reading
Fuel → Correct level
LDR → Correct light value
IR → Correct digital status
```

### Test 2 — Individual CAN Nodes

Verify that each node successfully transmits its assigned CAN frame.

```text
Node 1 → CAN ID 0x01
Node 2 → CAN ID 0x02
```

### Test 3 — CAN Gateway

Verify that the ESP8266 receives and decodes both CAN message types.

### Test 4 — LCD

Verify that the decoded values are displayed correctly.

### Test 5 — Wi-Fi

Verify that the ESP8266 connects to the configured network.

### Test 6 — Email

Verify that the monitoring information is successfully transmitted through the SMTP server.

---

# 🏁 Final System Flow

```text
 ┌────────────────────────────────────────────┐
 │              VEHICLE SENSORS              │
 │                                            │
 │ Temperature │ Fuel │ IR │ Ambient Light   │
 └──────────────────────┬─────────────────────┘
                        │
                        ▼
                ┌───────────────┐
                │ CAN SENSOR    │
                │    NODES      │
                └───────┬───────┘
                        │
                        ▼
                 ╔══════════════╗
                 ║   CAN BUS    ║
                 ║   250 kbps   ║
                 ╚══════╤═══════╝
                        │
                        ▼
                ┌───────────────┐
                │    ESP8266    │
                │    GATEWAY    │
                └───────┬───────┘
                        │
              ┌─────────┼─────────┐
              │         │         │
              ▼         ▼         ▼
            LCD       Wi-Fi    Processing
                        │
                        ▼
                 ┌─────────────┐
                 │    SMTP     │
                 │    EMAIL    │
                 └──────┬──────┘
                        │
                        ▼
                    USER ALERT
```

---

# ⭐ Key Takeaway

The project demonstrates how a distributed vehicle monitoring system can be built by combining:

```text
        Sensors
           +
      CAN Protocol
           +
        MCP2515
           +
        ESP8266
           +
          Wi-Fi
           +
      Email Alerts
           =
 Intelligent Vehicle Monitoring
```

The most important concept demonstrated is the **gateway architecture**:

> **CAN handles communication inside the monitoring network, while the ESP8266 bridges the embedded system to the Internet for remote notification.**

---

# 👩‍💻 Project Focus

### Core Technologies

```text
CAN
MCP2515
ESP8266
SPI
I²C
LDR
IR Sensor
Temperature Sensor
Fuel-Level Sensing
Wi-Fi
SMTP
Embedded C/C++
```

### Core Embedded Skills

* Sensor interfacing
* CAN communication
* SPI communication
* I²C communication
* Data serialization
* Data parsing
* Embedded networking
* IoT gateway design
* Remote alerting
* Modular embedded-system development

---

<div align="center">

# 🚗 CAN → ESP8266 → IoT

### **From Vehicle Sensors to Remote Alerts**

**Embedded Systems | CAN | IoT | Vehicle Monitoring**

</div>

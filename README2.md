# Real-time Remote Toxic Gas Monitoring System

## 📖 Overview

[cite_start]This project is a robust, IoT-based system for real-time remote monitoring of toxic gases and air quality in hazardous environments like mines and chemical plants[cite: 223]. [cite_start]The system is designed to be a scalable, cost-effective, and reliable solution to enhance workforce safety and environmental oversight[cite: 232]. [cite_start]It leverages long-range wireless communication (LoRa) to transmit data from sensor nodes to a central gateway, which then relays the information to an interactive web-based dashboard for live monitoring and instant alerts[cite: 228, 230, 231].

[cite_start]The project emphasizes a professional engineering approach, moving from a basic prototype to a deployment-ready product by incorporating high-precision sensors, robust hardware design, efficient firmware, and a feature-rich user interface[cite: 4, 15].

## ✨ Key Features

- **Multi-Gas Detection**: Accurately measures concentrations of critical gases:
    - [cite_start]Carbon Dioxide (CO2) [cite: 227]
    - [cite_start]Carbon Monoxide (CO) [cite: 227]
    - [cite_start]Sulfur Dioxide (SO2) [cite: 227]
    - [cite_start]Nitrogen Dioxide (NO2) [cite: 227]
    - [cite_start]Methane (CH4) [cite: 227]
- [cite_start]**High-Precision Sensing**: Utilizes a hybrid sensor strategy for maximum reliability, featuring NDIR sensors for CO2 and industrial-grade electrochemical sensors for toxic gases (CO, SO2, NO2)[cite: 30, 34, 35].
- [cite_start]**Long-Range Communication**: Employs LoRa technology (SX1278 module) to ensure stable data transmission over long distances in challenging industrial environments[cite: 228, 237].
- **Real-time Web Dashboard**: An interactive front-end application that visualizes data with:
    - [cite_start]Live data gauges for at-a-glance readings[cite: 185].
    - [cite_start]Historical data charts for trend analysis using Chart.js[cite: 184].
    - [cite_start]A geospatial map (Leaflet.js) showing the status and location of each sensor node[cite: 188].
- [cite_start]**Instant Alerts**: The system automatically sends alerts when gas concentrations exceed predefined safety thresholds based on official Vietnamese environmental standards (QCVN 05:2013/BTNMT)[cite: 231, 141, 143].
- [cite_start]**Energy Efficient**: Designed for field deployment with smart power management, including support for solar charging and leveraging the ESP32's Deep Sleep mode to maximize battery life[cite: 83, 97].

## 🛠️ Technology Stack

### Hardware
- [cite_start]**Microcontroller**: ESP32 [cite: 228]
- [cite_start]**Communication**: LoRa Module (SX1278, 433MHz) [cite: 237, 69]
- **Sensors**:
    - [cite_start]**CO2**: MH-Z19B (NDIR) [cite: 30]
    - [cite_start]**CO, SO2, NO2**: High-precision Electrochemical Sensors [cite: 34]
    - [cite_start]**CH4**: MQ-4 Sensor [cite: 37]
- [cite_start]**Power**: Li-ion Battery with TP4056 Solar Charging Circuit and Load Sharing [cite: 85, 88]

### Firmware
- [cite_start]**Development Environment**: Visual Studio Code + PlatformIO [cite: 107]
- **Framework**: Arduino
- **Key Libraries**:
    - [cite_start]`RadioLib` for robust LoRa communication[cite: 117].
    - [cite_start]`PubSubClient` for MQTT communication at the gateway[cite: 175].
    - [cite_start]`ArduinoOTA` for over-the-air firmware updates on the gateway[cite: 155].
- [cite_start]**Version Control**: Git & GitHub [cite: 109]

### Software & Cloud
- [cite_start]**Communication Protocol**: MQTT [cite: 168]
- [cite_start]**MQTT Broker**: HiveMQ (for development) [cite: 174]
- [cite_start]**Dashboard**: Custom Web Application (HTML, CSS, JavaScript) [cite: 178]
- **Frontend Libraries**:
    - [cite_start]Chart.js [cite: 184]
    - [cite_start]Gauge.js [cite: 185]
    - [cite_start]Leaflet.js [cite: 188]
    - [cite_start]Paho.js (MQTT over WebSockets) [cite: 182]
- [cite_start]**Hosting**: GitHub Pages [cite: 179]

## 🏗️ System Architecture

The system consists of two main components:

1.  **Sensor Nodes**: Deployed at remote locations. [cite_start]Each node reads data from its sensors, packs it into an efficient binary format (C struct), and transmits it periodically via LoRa[cite: 119, 132]. [cite_start]The ESP32's deep sleep mode is used between transmissions to conserve power[cite: 97].

2.  [cite_start]**Gateway & Dashboard**: A central gateway (also an ESP32) continuously listens for LoRa packets[cite: 169]. [cite_start]Upon receiving data, it connects to a local WiFi network, publishes the sensor readings to an MQTT broker, and enables OTA updates[cite: 154, 167, 168, 171]. [cite_start]The web dashboard subscribes to the MQTT topics via WebSockets to display the data in real-time to the end-user[cite: 181].

## 🚀 Getting Started

### Prerequisites
- [VS Code](https://code.visualstudio.com/) with the [PlatformIO IDE extension](https://platformio.org/) installed.
- Git installed on your local machine.

### Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/your-repository-name.git](https://github.com/your-username/your-repository-name.git)
    cd your-repository-name
    ```

2.  **Open the project in VS Code:**
    - Open VS Code and click on `File > Open Folder...` and select the cloned repository folder.
    - PlatformIO will automatically detect the `platformio.ini` file and install the required libraries and dependencies.

3.  **Configure the Firmware:**
    - In the `src` folder, configure your WiFi credentials, MQTT broker details, and LoRa parameters.

4.  **Build and Upload:**
    - Connect your ESP32 board via USB.
    - Use the PlatformIO toolbar to **Build** and **Upload** the firmware to the Sensor Node and Gateway devices.

### Usage
- Once the Gateway is running and connected to WiFi, it will start forwarding data.
- Open the `index.html` file from the `dashboard` folder in your browser to view the live monitoring dashboard. For best results, host the dashboard folder on a web server or use GitHub Pages.

## 📈 Future Development

- [ ] [cite_start]Expand the sensor network with more nodes[cite: 241].
- [ ] [cite_start]Implement AI/ML algorithms for predictive analysis and risk forecasting[cite: 261, 268].
- [ ] [cite_start]Develop a mobile application for on-the-go monitoring and alerts[cite: 276].
- [ ] Further optimize power consumption for long-term, maintenance-free operation.
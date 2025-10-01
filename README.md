# Real-time Remote Toxic Gas Monitoring System

## 1. Introduction & Vision

### 1.1. Problem Statement

Industrial environments such as manufacturing plants, chemical facilities, and mines present significant occupational safety risks. The presence of toxic gases (e.g., CO, NO2), the potential for combustible gas leaks (CH4), and poor ambient air quality (high CO2, high PM2.5) can lead to severe health hazards for workers and catastrophic safety incidents. Existing monitoring solutions are often prohibitively expensive, difficult to deploy across large or RF-challenging areas, and lack intelligent, context-aware alerting capabilities.

### 1.2. Vision

To engineer a robust, scalable, and cost-effective IoT solution for comprehensive occupational safety and environmental monitoring in industrial settings. This project aims to design, build, and validate an end-to-end system capable of real-time data acquisition, long-range wireless transmission, and intelligent analysis of critical atmospheric hazards.

## 2. System Architecture

The system is designed with a two-tier architecture consisting of distributed Sensor Nodes and a central Gateway.

1.  **Sensor Node:**
    *   **Function:** Deployed at critical locations to acquire data from a suite of sensors. It processes this data and transmits it wirelessly via LoRa.
    *   **Characteristics:** Designed for low-power operation, enabling long-term deployment on battery power, supported by deep-sleep cycles.

2.  **Gateway:**
    *   **Function:** Acts as a bridge between the LoRa network and the internet. It receives data packets from all Sensor Nodes, decodes them, and forwards the information to a cloud-based MQTT broker via Wi-Fi.
    *   **Characteristics:** Assumed to have a stable power source and internet connectivity, typically located in a central control room or office.

### 2.1. Data Flow

Sensor Data -> Sensor Node (ESP32) -> LoRa Transmission -> Gateway (ESP32) -> Wi-Fi -> MQTT Broker -> Web Dashboard & AI Model

## 3. Key Features

- **Multi-Parameter Sensing:** Concurrent monitoring of Carbon Monoxide (CO), Nitrogen Dioxide (NO2), Methane (CH4), Carbon Dioxide (CO2), and Particulate Matter (PM2.5).
- **Long-Range Communication:** Implementation of the LoRa protocol for robust data transmission in large-scale industrial environments where Wi-Fi is unreliable or unavailable.
- **Real-time Visualization:** A web-based dashboard featuring live data gauges, historical trend charts, and a geospatial map of node locations and statuses.
- **Intelligent Alerting:** An automated, multi-level alert system based on both static thresholds and dynamic, AI-adjusted parameters.
- **Power Efficiency:** A low-power design for Sensor Nodes, utilizing deep-sleep modes to maximize operational longevity on battery power.

## 4. Hardware Selection

Hardware selection is based on a **Hybrid Sensor Strategy**, choosing the most appropriate technology for each specific measurement to balance accuracy, reliability, and cost.

### 4.1. Core Components

*   **Microcontroller**
    *   **Selection:** ESP32 DevKit V1
    *   **Rationale:** A powerful dual-core MCU with integrated Wi-Fi and Bluetooth, extensive community support, and cost-effectiveness.

*   **LoRa Module**
    *   **Selection:** SX1278 (433MHz)
    *   **Rationale:** A widely-used LoRa transceiver providing reliable long-range, low-power communication. The 433MHz frequency is suitable for IoT applications in Asia.

### 4.2. Sensor Suite & Rationale

#### Ventilation & Health - Carbon Dioxide (CO2)

*   **Selected Sensor:** MH-Z19C
*   **Technology:** NDIR (Non-Dispersive Infrared)
*   **Purpose:** Monitors ventilation quality to ensure worker health and alertness. High CO2 levels indicate stagnant air, leading to fatigue and reduced productivity.
*   **Technical Analysis:** Uses NDIR technology, which measures CO2 concentration based on the absorption of a specific infrared wavelength. This method is highly selective, accurate, stable over time, and provides a direct digital output (UART), simplifying integration.

#### Toxic Gas - Carbon Monoxide (CO)

*   **Selected Sensor:** DFRobot Fermion MEMS CO
*   **Technology:** MEMS (Micro-Electro-Mechanical Systems)
*   **Purpose:** Provides critical alerts for acute poisoning risks from incomplete combustion processes (e.g., boilers, engines). CO is a colorless, odorless, and highly toxic gas.
*   **Technical Analysis:** This pre-calibrated module with a standard I2C interface eliminates the complex hardware and software overhead associated with traditional MOS sensors like the MQ-7, which require a dual-voltage heating cycle.

#### Toxic Exhaust - Nitrogen Dioxide (NO2)

*   **Selected Sensor:** DFRobot Fermion MEMS NO2
*   **Technology:** MEMS
*   **Purpose:** Monitors toxic nitrogen dioxide emissions, primarily from diesel engines (e.g., forklifts, generators) in enclosed or poorly ventilated areas. NO2 is a severe respiratory irritant.
*   **Technical Analysis:** Similar to the CO sensor, this pre-calibrated I2C module simplifies integration and ensures reliable qualitative measurements, allowing the project to focus on system-level features.

#### Combustible Gas - Methane (CH4)

*   **Selected Sensor:** MQ-4
*   **Technology:** MOS (Metal Oxide Semiconductor)
*   **Purpose:** Detects methane/natural gas leaks to prevent fire and explosion hazards. The primary goal is leak detection rather than precise low-level concentration measurement.
*   **Technical Analysis:** Employs a MOS sensing element optimized for high sensitivity to Methane (CH4). While it exhibits cross-sensitivity to other combustible gases, this is acceptable in a safety context, as any detection of flammable gas warrants an alert. Its low cost and high availability make it a practical choice.

#### Particulate Pollution - PM2.5

*   **Selected Sensor:** Plantower PMS5003
*   **Technology:** Laser Scattering
*   **Purpose:** Monitors fine particulate matter generated from industrial processes (e.g., cutting, grinding, welding), which poses a long-term respiratory health risk to workers.
*   **Technical Analysis:** Operates on the principle of laser scattering. An internal algorithm calculates the concentration of PM1.0, PM2.5, and PM10 particles. It provides reliable, real-time data via a UART interface.

## 5. Technology Stack

*   **Sensing Layer**
    *   **Component:** Distributed Sensor Nodes
    *   **Technology:** MCU: ESP32 DevKit V1; Sensors: MH-Z19C (NDIR), DFRobot Fermion MEMS (CO, NO2), MQ-4 (MOS), Plantower PMS5003 (Laser)

*   **Communication Layer**
    *   **Component:** Node-to-Gateway
    *   **Technology:** Protocol: LoRa; Hardware: SX1278 Transceiver Module (433MHz); Library: RadioLib
    *   **Component:** Gateway-to-Cloud
    *   **Technology:** Protocols: Wi-Fi, MQTT; Hardware: ESP32 DevKit V1; Library: PubSubClient

*   **Application Layer**
    *   **Component:** Cloud & Data Brokerage
    *   **Technology:** Broker: Public MQTT Broker (e.g., HiveMQ); Hosting: GitHub Pages
    *   **Component:** Frontend / Visualization
    *   **Technology:** Framework: HTML, CSS, JavaScript; Libraries: MQTT.js, Chart.js, Gauge.js, Leaflet.js

*   **Intelligence Layer**
    *   **Component:** AI / Machine Learning
    *   **Technology:** Frameworks: Python with TensorFlow or scikit-learn; Deployment: Cloud-based inference or Edge deployment on Gateway (advanced).

## 6. Project Milestones

- **Phase 0: Foundation & Planning**
  - Finalize system requirements and architecture.
  - Set up development environments (VS Code + PlatformIO) and Git repository.
  - Complete component sourcing and inspection.

- **Phase 1: Prototyping & Firmware Development**
  - Assemble the complete system on a breadboard.
  - Develop and test firmware for the Sensor Node and Gateway.
  - Validate the end-to-end data flow from sensor to MQTT broker.

- **Phase 2: Hardware Design & Fabrication**
  - Design a modular motherboard PCB in Altium Designer.
  - Generate manufacturing files (Gerber & NC Drill).
  - Procure and assemble the custom PCB.

- **Phase 3: Dashboard Development**
  - Implement the frontend UI/UX.
  - Integrate real-time data visualization widgets.
  - Deploy the web application to GitHub Pages.

- **Phase 4: AI Model Development & Integration**
  - Collect a baseline time-series dataset.
  - Train and evaluate an initial machine learning model.
  - Integrate the model's inference logic into the system.

- **Phase 5: System Integration, Testing & Optimization**
  - Assemble the final PCB-based hardware.
  - Conduct full-system testing (LoRa range, battery life, sensor accuracy, AI model performance).
  - Optimize firmware for power consumption and reliability.

- **Phase 6: Finalization & Reporting**
  - Design and 3D print a custom enclosure for the Sensor Node.
  - Complete the final thesis report.
  - Prepare for the final project defense.

## 7. Success Criteria (KPIs)

- **Connectivity:** Achieve stable LoRa communication at a defined target distance within a mixed industrial environment.
- **Latency:** Maintain an end-to-end data latency (from sensor reading to dashboard update) below a specified threshold (e.g., <10 seconds).
- **Power Efficiency:** The Sensor Node must achieve a target operational duration on a single battery charge.
- **Data Accuracy:** Sensor readings must be validated and fall within the manufacturers' specified tolerance.
- **Intelligence:** The AI model must demonstrate the ability to adjust alert thresholds or provide predictive insights.

## 8. Risks & Mitigation

*   **Risk: Component Sourcing Delays**
    *   **Probability:** Medium
    *   **Impact:** Medium
    *   **Mitigation:** Prioritize suppliers with confirmed stock in Hanoi. Order all critical components at the beginning of Phase 1.

*   **Risk: Technical Complexity**
    *   **Probability:** High
    *   **Impact:** High
    *   **Mitigation:** Adopt a modular, iterative development process. Test each subsystem independently before integration.

*   **Risk: Insufficient Data for AI Training**
    *   **Probability:** Medium
    *   **Impact:** High
    *   **Mitigation:** Start data collection as soon as the prototype is stable. If necessary, augment the dataset with simulated data or define a simpler, rule-based initial model.

*   **Risk: RF Interference in Industrial Environment**
    *   **Probability:** Medium
    *   **Impact:** Medium
    *   **Mitigation:** Implement robust error-checking (CRC) in the LoRa communication protocol. Select optimal LoRa parameters (Spreading Factor, Bandwidth) for the target environment during testing.
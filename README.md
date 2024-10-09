# Wings of Relief

Wings of Relief is a drone-based medical delivery system designed to provide rapid and efficient distribution of essential medical supplies in remote and underserved regions. Leveraging UAV technology, the system automates the delivery of medications and healthcare products, making healthcare accessible to areas with limited infrastructure.

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Installation and Configuration](#installation-and-configuration)
- [Usage](#usage)

## Project Overview
This project aims to address the logistical challenges in delivering medical supplies to remote areas and during emergencies. By using drones integrated with the **APM 2.8** flight controller, Mission Planner software, and real-time telemetry systems via **ESP32**, we ensure quick and reliable medical deliveries, bypassing traditional transportation barriers. While this prototype uses APM for short-range operations, future iterations will utilize **Pixhawk** and **3DR telemetry** for long-range GPS and RTK capabilities.

## Features
- **Automated Medical Delivery**: Fully autonomous delivery to the user's location.
- **Emergency & Normal Modes**: Efficient routing for both emergencies and regular deliveries.
- **Telemetry Monitoring**: Real-time updates of drone status and location using ESP32 Wi-Fi telemetry.
- **Seamless Integration**: Combines drones with Mission Planner for route planning and data monitoring.
- **Environment Flexibility**: Capable of flying in various terrains and weather conditions.

## System Architecture
The system workflow is as follows:
1. **Mode Selection**: User selects either emergency or normal mode.
2. **Data Input**: User provides prescription details and destination location. In normal mode, the medical shop location is also provided.
3. **Mission Planning**: Using Mission Planner, the delivery route is planned accordingly.
4. **Pre-flight Checks**: Drone systems are checked for errors; the operator is notified if issues are found.
5. **Mission Execution**: Drone takes off, following the planned route while transmitting real-time data to the operator.
6. **Delivery**: In normal mode, the drone collects medicine from the medical shop before proceeding to the user. In emergency mode, it goes directly to the user.
7. **Completion**: After delivering the medicine, the drone returns to home base.

## Technology Stack
- **Drone Flight Controller**: APM 2.8
- **Mission Planner Software**: For mission planning and telemetry data
- **Communication**: ESP32 Wi-Fi Telemetry
- **Programming Languages**: Arduino IDE for ESP32, C++ for firmware
- **Hardware Components**: Quadcopter frame, GPS module, ESCs, Brushless DC motors, LiPo battery

## Hardware Requirements
1. **APM 2.8 Flight Controller**
2. **Quadcopter Frame**
3. **Brushless DC Motors (4x)**
4. **Electronic Speed Controllers (ESCs) (4x)**
5. **Propellers (2x CW, 2x CCW)**
6. **ESP32 Module** (for Wi-Fi telemetry)
7. **GPS Module (e.g., NEO-7M)**
8. **LiPo Battery (3S or 4S, depending on motor specifications)**
9. **Power Distribution Board or Power Module**
10. **RC Transmitter and Receiver**
11. **Battery Charger**
12. **Miscellaneous**: Cables, connectors, mounting hardware

## Software Requirements
1. **Mission Planner**: Ground Control Station software
2. **ArduPilot Firmware**: For APM 2.8
3. **Arduino IDE**: For programming the ESP32
4. **T6Config Software**: For configuring the RC transmitter (if applicable)
5. **Google Maps API**: Integrated into Mission Planner for mapping and elevation checks
6. **Drivers**: USB drivers for APM 2.8 and ESP32

## Installation and Configuration

### 1. Hardware Setup

#### Assemble the Drone
- **Frame and Motors**:
  - Attach the four **Brushless DC Motors** to the **Quadcopter Frame**.
  - Mount the **Propellers** onto the motors (ensure two are clockwise (CW) and two are counter-clockwise (CCW)).
  
- **Electronic Speed Controllers (ESCs)**:
  - Connect each **ESC** to its corresponding motor.
  - Connect the signal wires from the ESCs to the **APM 2.8** outputs (Channels 1-4).

- **Flight Controller (APM 2.8)**:
  - Mount the **APM 2.8** flight controller securely using anti-vibration mounts.
  - Ensure the orientation arrow on the APM 2.8 points forward.

- **Power Module**:
  - Connect the **Power Module** between the LiPo battery and the APM 2.8.
  - The Power Module provides regulated power and monitors voltage/current.

- **GPS Module (NEO-7M)**:
  - Mount the **GPS Module** on the frame, away from electromagnetic interference.
  - Connect it to the GPS port on the APM 2.8.

- **RC Receiver**:
  - Connect the **RC Receiver** channels to the APM 2.8 inputs (Channels 1-8 as needed).

- **ESP32 Module for Wi-Fi Telemetry**:
  - **Wiring**:
    - Connect **ESP32 RX** to **APM 2.8 TX** (Telemetry port).
    - Connect **ESP32 TX** to **APM 2.8 RX**.
    - Use a logic level converter if necessary (APM 2.8 operates at 5V, ESP32 at 3.3V).
  - **Power Supply**:
    - Provide 5V power to the ESP32 from the APM 2.8 or an external BEC.

### 2. Software Installation

#### Mission Planner
- Download and install **Mission Planner** from the [official website](https://ardupilot.org/planner/docs/mission-planner-installation.html).

#### Arduino IDE
- Download and install the **Arduino IDE** from the [official website](https://www.arduino.cc/en/software).
- Install the **ESP32 Board Manager**:
  - Go to **File** > **Preferences**.
  - Add the following URL to **Additional Boards Manager URLs**:
    ```
    https://dl.espressif.com/dl/package_esp32_index.json
    ```
  - Open **Tools** > **Board** > **Boards Manager**, search for **ESP32**, and install.

#### T6Config Software (if using FlySky/Turnigy Transmitter)
- Download and install **T6Config** to configure your RC transmitter.

### 3. Configuring the APM 2.8 Flight Controller

#### Firmware Update
- Connect the APM 2.8 to your computer via USB.
- Open **Mission Planner**.
- Go to **Initial Setup** > **Install Firmware**.
- Select the appropriate firmware (e.g., **ArduCopter V3.2.1** for APM 2.8).

#### Sensor Calibration
- Perform mandatory calibrations under **Initial Setup** > **Mandatory Hardware**:
  - **Accelerometer Calibration**
  - **Compass Calibration**
  - **Radio Calibration**
  - **ESC Calibration** (follow on-screen instructions)

### 4. Setting Up ESP32 for Wi-Fi Telemetry

#### Programming the ESP32
- Connect the ESP32 to your computer via USB.
- Open the Arduino IDE.
- Select **ESP32 Dev Module** under **Tools** > **Board**.
- Install necessary libraries (e.g., WiFi, WiFiAP).
- Upload a sketch that sets up the ESP32 as a Wi-Fi access point and forwards serial data to Mission Planner. You can use examples or write a custom script to handle MAVLink data.

#### Wiring and Connections
- Ensure the ESP32 is correctly connected to the APM 2.8 telemetry port.
- Double-check voltage levels to prevent damage.

#### Configuring Mission Planner
- Connect your PC to the ESP32's Wi-Fi network.
- In Mission Planner, select **UDP** or **TCP** connection.
- Enter the IP address and port specified in your ESP32 sketch.

### 5. Mission Planning

- Open **Mission Planner** and connect to your drone via Wi-Fi telemetry.
- Go to the **Flight Plan** tab.
- Set up waypoints:
  - **Normal Mode**: Include waypoints for the medical shop and user location.
  - **Emergency Mode**: Directly set the user location as the destination.
- Configure actions at each waypoint (e.g., **LOITER**, **LAND**).
- Save and upload the mission to the APM 2.8.

### 6. Pre-Flight Checks

- Ensure all connections are secure.
- Verify that all calibrations are complete.
- Check battery levels.
- Confirm telemetry link is active.
- Perform a test arming and motor spin-up without propellers.

### 7. Safety Measures

- Always remove propellers during configuration and testing.
- Fly in open areas away from people and obstacles.
- Follow local regulations and obtain necessary permissions.

## Usage

1. **Starting the System**
   - Power on the RC transmitter.
   - Connect the battery to the drone.
   - Wait for the APM 2.8 to initialize and acquire GPS lock.

2. **Using Mission Planner**
   - Open **Mission Planner** and connect via Wi-Fi telemetry.
   - Monitor drone status and battery levels.

3. **Mission Execution**
   - Select the desired mission (emergency or normal).
   - Click **Start Mission**.
   - Monitor the mission execution on Mission Planner.

4. **After Delivery**
   - Once delivery is complete, the drone will return to the home position.
   - Perform post-flight checks and maintenance as needed.

## Circuit Diagram
![Circuit Diagram](https://github.com/sakthivel3000/Wings-of-Relief/blob/main/apmfinal.png)

## Drone Image
![Final Drone](https://github.com/sakthivel3000/Wings-of-Relief/blob/main/drone.png?raw=true)

## Conclusion
Wings of Relief aims to revolutionize medical supply delivery through innovative drone technology. With automated mission planning and real-time monitoring, we strive to enhance healthcare accessibility in remote areas, ensuring timely deliveries where they're needed most.

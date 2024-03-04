Project Report: Water Level Monitoring and Pump Control System
1. Introduction
Water level monitoring and pump control systems are essential for various applications, including agriculture, industrial processes, and home automation. This project aims to develop a system that can accurately measure water levels and control a pump based on the detected level.
2. Objectives
The main objectives of this project are:
Design and implement a system to monitor water levels using an ultrasonic sensor.
Interface the system with a NodeMCU microcontroller for data processing and control.
Control a water pump based on the measured water level.
Provide remote monitoring and control capabilities through the Blynk mobile app.
3. Components Used
The following components are utilized in this project:
NodeMCU microcontroller (ESP8266)
Ultrasonic sensor (HC-SR04)
Relay module
Toggle button
I2C screen (LCD with I2C interface)
4. System Architecture
The system architecture consists of the following connections:
Ultrasonic sensor trig pin connected to D5 and echo pin connected to D6 of the NodeMCU.
Relay module signal pin connected to D7 of the NodeMCU.
Toggle button pin connected to D3 of the NodeMCU.
I2C screen SCL pin connected to D1 and SDA pin connected to D2 of the NodeMCU.
5V connection shared between the I2C screen and the relay module.
All ground connections are independent and connected to respective ground pins on the NodeMCU.

5. Implementation
The system operates as follows:
The ultrasonic sensor measures the water level, and the NodeMCU processes the data.
The NodeMCU controls a relay module to turn the pump on or off based on the water level.
A toggle button allows manual control of the pump.
An I2C screen displays the current water level and pump status.
The system is integrated with the Blynk mobile app for remote monitoring and control.
6. Blynk App Configuration
The Blynk app is configured with the following widgets:
Water level display (Virtual Pin V1)
Relay control button (Virtual Pin V2)
Pump status indicator (Virtual Pin V3)
7. Results and Observations
The system successfully monitors water levels and controls the pump based on the set threshold. Remote monitoring and control via the Blynk app provide convenience and flexibility. During testing, the system demonstrated reliable performance and accuracy in water level measurement.
8. Conclusion
In conclusion, the water level monitoring and pump control system developed in this project fulfill the objectives effectively. It offers a cost-effective and efficient solution for managing water resources. Future enhancements may include additional sensors for improved accuracy and predictive maintenance features.
9. References
HC-SR04 Ultrasonic Sensor Datasheet
NodeMCU ESP8266 Documentation
Blynk Mobile App Documentation

This project report provides a comprehensive overview of the water level monitoring and pump control system, including its objectives, components, implementation details, results, and conclusions. If you have any further questions or need additional information, please feel free to ask!


# Flight Control System – AADL Model (Drone Flight Controller)

## Author

**Name and surname:** Łukasz Bogacz  

**E-mail:** lukaszbogacz@student.agh.edu.pl

---

## Description of the Modeled System

### General Description

The model presents the architecture of a flight control system for a four-rotor drone (quadcopter), designed using **AADL (Architecture Analysis & Design Language)**.  
The system implements the complete flight control process, which includes:

- acquisition of sensor data (IMU, battery voltage, RC receiver),
- estimation of spatial orientation (roll, pitch, yaw),
- computation of control signals (PID),
- flight mode and safety management,
- motor control via ESCs,
- telemetry,
- data logging to memory (blackbox).

The architecture is divided into **functional subsystems**, which improve readability, modularity, and enable system analysis.

---

### User-Oriented Description

From the perspective of the user (drone pilot), the system:

- receives commands from the RC transmitter,
- automatically stabilizes the drone,
- allows switching between flight modes,
- protects against unsafe situations (e.g. low battery level),
- records flight data for later analysis,
- transmits telemetry data.

As a result, the drone is stable, responsive, and safe to operate.

---

## List of AADL Components with Commentary

### Data Types (Data)

| Name | Description |
|------|-------------|
| `imu_data` | IMU sensor data: accelerations and angular rates |
| `sbus_frame` | Data frame from the RC receiver (SBUS) |
| `attitude_data` | Drone orientation: roll, pitch, yaw |
| `motor_command` | Motor control values |
| `flight_mode_type` | Current flight mode |
| `system_status` | System status (arming state, battery voltage, mode) |
| `telemetry_data` | Data transmitted via telemetry |

---

### Buses

| Type | Usage |
|------|-------|
| `spi`  | Communication with the IMU |
| `uart` | RC receiver and telemetry |
| `qspi` | External flash memory |
| `pwm`  | ESC motor control |

---

### Devices

| Name | Function |
|------|----------|
| `IMU` | Inertial Measurement Unit |
| `Receiver` | RC receiver |
| `Battery_Sensor` | Battery voltage sensor |
| `Telemetry_Transmitter` | Telemetry module |
| `ESC` | Electronic Speed Controllers |

---

### Memory

| Name | Description |
|------|-------------|
| `External_Flash` | External QSPI flash memory for data logging |

---

### Threads

| Name | Role in the System |
|------|--------------------|
| `IMU_Driver` | Filters IMU data |
| `RC_Handler` | Processes RC input |
| `Battery_Monitor` | Monitors battery voltage |
| `Flight_Mode_Manager` | Manages flight modes |
| `Safety_Monitor` | Monitors system safety |
| `Attitude_Estimator` | Estimates orientation |
| `PID_Controller` | PID control algorithm |
| `Motor_Mixer` | Mixes control signals for motors |
| `Telemetry_Manager` | Handles telemetry |
| `Blackbox_Writer` | Writes data to flash memory |

---

### Subsystems (Processes / Systems)

| Name | Function |
|------|----------|
| `Subsys_Data_Acquisition` | Sensor data acquisition |
| `Subsys_Flight_Logic` | Flight logic and control |
| `Subsys_Output_Management` | Outputs: motors and telemetry |
| `Subsys_Blackbox` | Data logging |
| `Data_Processing_System` | Integration of main subsystems |
| `Quadcopter_FC` | Top-level system |

---

## Graphical System Model

### Simplified System Diagram

![SimpleDiagram](./img/simple_diagram.svg)

### Full System Diagram

![FullDiagram](./img/advanced_diagram.svg)

---

## System Analyses Performed Using OSATE

### ARINC429 Consistency

The analysis did not reveal any errors.  
The report is available at:  
[instances/reports/ARINC429Consistency](https://github.com/13ukasz/UAVFlightControl/tree/main/instances/reports/ARINC429Consistency)

---

### Bound Resource Budgets

The maximum CPU load was assumed to be **480 MIPS**.  
The analysis showed that, based on the adopted assumptions, the system requires **348.299 MIPS**, which is within the defined budget.

Detailed report:  
[instances/reports/BoundResourceBudgets](https://github.com/13ukasz/UAVFlightControl/tree/main/instances/reports/BoundResourceBudgets)

---

### Bus Load

The Bus Load analysis results are available at:  
[instances/reports/BusLoad](https://github.com/13ukasz/UAVFlightControl/tree/main/instances/reports/BusLoad)

The analysis did not provide new information about the actual bus utilization. It contains data related to:

- assumed bandwidth budgets,
- capacities of individual buses.

The lack of actual load results is caused by issues with assigning parameters to connections between components using the buses.

---

### Connection Consistency

Detailed report available at:  
[instances/reports/ConnectionConsistency](https://github.com/13ukasz/UAVFlightControl/tree/main/instances/reports/ConnectionConsistency)

The analysis did not reveal any errors.

---

### Not Bound Resource Budgets

The results of this analysis are very similar to those of the **Bound Resource Budgets** analysis.  
The CPU resource demand remains within the assumed limits.

Detailed report:  
[instances/reports/NotBoundResourceBudgets](https://github.com/13ukasz/UAVFlightControl/tree/main/instances/reports/NotBoundResourceBudgets)

---

### Weight Analysis

The modeled system primarily represents a software architecture.  
Nevertheless, approximate weights were assigned to hardware components and a weight analysis was performed.

As a result, some elements in the report have a value of **0**, but the overall meaning of the analysis remains valid and consistent.

Detailed report available at:  
[instances/reports/WeightAnalysis](https://github.com/13ukasz/UAVFlightControl/tree/main/instances/reports/WeightAnalysis)

---

## References

1. Betaflight – Open Source Flight Controller Firmware  
   https://github.com/betaflight/betaflight

2. INAV – Navigation-enabled Flight Control Firmware  
   https://github.com/iNavFlight/inav

3. ArduPilot – Open Source Autopilot System  
   https://ardupilot.org

4. Designing My Own Flight Controller  
   https://flying-rabbit-fpv.com/2020/10/06/designing-my-own-flight-controller/

5. How I Developed the Scout Flight Controller – Future Improvements  
   https://timhanewich.medium.com/how-i-developed-the-scout-flight-controller-part-10-future-improvements-ae1957f81f76

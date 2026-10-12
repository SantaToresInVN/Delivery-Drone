# Delivery Drone — ESP32 Quadcopter Prototype

An undergraduate embedded systems project focused on developing and testing an ESP32-based quadcopter flight controller. The prototype integrates inertial sensing, radio control, attitude estimation, PID control and electronic speed controller (ESC) outputs.

**Course:** Fundamentals of Embedded Systems  
**Course grade:** A  
**My role:** Primary contributor within a four-member research team.
**Main contributions:**
- Developed the project concept.
- Drew circuit schematics and system block diagrams.
- Purchased electronic components.
- Assembled the prototype on a breadboard and connected the components through wiring.
**Current status:** Bench-tested prototype; stable free flight has not yet been demonstrated.

## Project Overview

The project explores how a microcontroller can read pilot commands and motion data, estimate the aircraft's attitude, and adjust four motors through a feedback control loop. Development covered hardware integration, firmware, PID adjustment and testing on a fixed single-axis stand.

## Hardware

| Component | Purpose |
| --- | --- |
| ESP32 DevKit | Sensor acquisition, receiver input processing and control calculations |
| MPU6050 | Accelerometer and gyroscope measurements over I2C |
| Microzone 6C Mini and MC7RE-V2 receiver | Pilot commands through six PWM channels |
| Four A2212 2450KV brushless motors and 30A ESCs | Motor actuation |
| F250 frame | Mechanical structure |
| 3S 11.1 V 2200 mAh LiPo battery and LM2596 module | Power supply and voltage regulation |

## Firmware and Control

- **IMU processing:** Read MPU6050 measurements over I2C, apply sensor offsets and low-pass filtering, and use Madgwick 6DOF sensor fusion to estimate attitude.
- **Receiver input:** Measure six RC PWM channels using GPIO interrupts and map pilot inputs to throttle, roll, pitch and yaw commands.
- **PID control:** Use angle control for roll and pitch, and angular-rate control for yaw. Mix the controller outputs with throttle to generate commands for four motors in an X configuration.
- **Motor output:** Send PWM commands to the ESCs using ESP32Servo.
- **Web-based PID adjustment:** The supplied firmware provides an ESP32 Wi-Fi access point and browser interface for adjusting roll/pitch PID gains. The web server runs in a separate FreeRTOS task pinned to an ESP32 core.

**Control flow:** MPU6050 → filtering and attitude estimation → PID controller → motor mixer → ESCs. RC inputs provide the pilot's setpoints.

## Testing and Observations

The project report documents checks of ESP32 startup, MPU6050 communication, receiver-channel mapping, ESC calibration and motor response.

On the single-axis test stand, the motors changed speed in response to manually tilting the frame. However, strong oscillation and instability occurred during stabilization tests, particularly when throttle increased. Further checks of motor/propeller orientation and PID tuning are required before free-flight testing. Flight duration and altitude were not established.

## Current Limitations and Next Steps

- Stable attitude control remains under development.
- The MPU6050-based configuration has no magnetometer reference for correcting yaw drift.
- Automatic altitude hold, GPS positioning and delivery operation have not been demonstrated.
- Planned improvements include systematic PID tuning, checks for vibration and mechanical imbalance, and a more compact controller layout. A barometer and GPS are possible later additions described in the report.

## Technology Stack

C/C++, ESP32, Arduino framework, MPU6050, I2C, GPIO interrupts, PWM, ESP32Servo, Madgwick 6DOF, PID, Wi-Fi and FreeRTOS.

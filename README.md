
<h1 align="center">
 Solar Boat 2025-2026
</h1>
<p align="center">
 Patrick Nguyen
</p>
<p align="center">
 Cal Poly Pomona Solar Boat Club
</p>

﻿<h1 align="left">
Project Summary
</h1>
 <p>
   Over the course of the 2025-2026 school year, I designed hardware for the Cal Poly Pomona Solar Boat Club to compete in the Solar Splash Competition.

   My 2 main projects that I detail here are:
   1. The auxiliary power supply which includes: a two-phase interleaved buck converter followed by buck-LDO cascaded circuits to create 12V, 5V, and 3.3V rails 
   2. A carrier board for the STM32-G474RE Nucleo-64 development board
 </p>

<h1 align="left">
1. Auxiliary Power Supply
</h1>
<p>
 The solar boat has one main energy storage: a pack of lead acid batteries configured to provide 36 V. The purpose of the auxiliary power supply was to transform the battery voltages to provide 12V, 5V, and 3.3V for auxiliary systems such as the data acquisition system, the control system, and various sensors/other chips.
</p>
<h2 align="left"> 
 1.1 System Architecture
</h2>
<p>
 The architecture of the auxiliary power supply is shown below:
 
 <img width="2140" height="648" alt="image" src="https://github.com/user-attachments/assets/0f7ade76-eaff-4695-aabb-a7aa7dbc0d40" />
 
</p>
<h2 align="left">
 1.2 Schematic and PCB Layout
</h2>
<p>
 ...
</p>
<h2>
 1.3 Results
</h2>
<p>
The final auxiliary power supply is shown below:
 <br>
 <img width="650" height="300" alt="image" src="https://github.com/user-attachments/assets/513cd3ee-0242-4124-b034-4cad9fcdd1ad" />

The PCBs were ordered from JLC PCB and all components were soldered by hand or with a hot plate/hot air gun.
</p>

<h1 align="left">
2. Carrier Board
</h1>
<h2 align="left">
 The Why and What
</h2>
<p>
The solar boat system this year included a steer-by-wire and throttle-by-wire system. The steering was mechanically decoupled from the rudders, allowing for electronic control of the steering to occur by sensing the steering wheel angle, and sending the appropriate control signals to the stepper motors which would turn the rudders. The throttle signal was also read electronically and outputted appropriate control signals to the motor controllers to control speed. This electronic system allows for extra steering by adding differential thrust on top of the rudder angle. If the steering wheel is turned, sharper turns can be achieved by running one motor faster than the other.

To implement the above functionalities, an STM32G474RE microcontroller was used, specifically on the NUCLEO 64 development platform. The STM32 microcontroller was in charge of executing the core functionalities of the boat: driving and steering. The steering used a closed feedback control loop, meaning it had to read sensor information important for controlling the angle such as the steering wheel angle and the actual angle of the rudder to calculate error. It also had to read the throttle signal.

Another system implemented this year was the remote telemetry data acquisition system. This would be done by a Raspberry PI. Separate from the STM32, this computing platform reads non operational critical data into a buffer for the remote server to read and display. The goal was to give both the driver and ground station information about various diagnostics such as battery temperatures, speed readings, charge, and various other information. For operation critical data that gets read first by the STM32, a communication channel should be set up between the STM32 and PI for the PI to read from the STM32.

All the above dictate the need for external electronic components not included in the NUCLEO64 development platform:
-Logic Level Shifters for digital sensor and encoder information that does not match the voltage of the STM32
-Differential Receivers to read stepper motor feedback signals
-Resistor Dividers and Op-Amps for analog inputs and outputs that do not match voltages with the STM32
-CAN Transceiver to communicate sensor data to the Raspberry PI
-Various I/O ports and connectors that are more stable and immune to vibrations than the jumper pin connections of the development board

These components are implemented on a carrier PCB that interfaces the development board with the above components.

</p>


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

<h2>1.3 Results</h2>
<p>
The final auxiliary power supply is shown below:
 <br>
 <img width="650" height="300" alt="image" src="https://github.com/user-attachments/assets/513cd3ee-0242-4124-b034-4cad9fcdd1ad" />

The PCBs were ordered from JLC PCB and all components were soldered by hand or with a hot plate/hot air gun.
</p>

<h3>1.4 Mistakes, Improvements, Reflections</h3>
<p>
 One confusing thing that may pop out is the fact that the 2-phase converter and the bucking converters / LDOs are separated into two boards, and that the circuit protections are after the first converter. This was because of the way that work was originally delegated. The team had first thought that the auxiliary electronics would be powered by a secondary battery, so the board containing the power electronics to create 12V, 5V, and 3.3V lines was supposed to come from a 12V battery (full charge would be at 12.4V which is why a LDO to 12V is used). Because of this, the 2-phase inverter and the other power electronics were initially separated until the system design changed to supply the auxiliary electronics straight from the 36V batteries. Since the designs were at different phases, the 12/5/3.3 board was made first and then the 36 to 13.5V board was made after. This entire system could in the future be integrated onto one board and made more compact.

The overall PCB layout for the 12/5/3.3 board could also be improved, making it more compact and wasting less space. We could also use different chips more conducive to reducing current loops (the chip used had the SW pin right between the input and gnd pins!). Alternative options for circuit protections could be used such as ICs for protection rather than the components used. All components on this board were 1206 since I was inexperienced in soldering, although I eventually became proficient enough to solder 0402s and larger in the carrier board (and even a VQFN-40 -_- ).

One oversight was the inrush current protection that was used. Firstly, it was incorrectly sized with too high of a resistance. The resistance was high enough to cause a voltage drop when 1+ amps were running, killing the low dropout threshold required for the 12V LDO and not heating up enough to reduce the resistance. It was a poor choice that was not thoroughly checked.

For the two-phase converter, I would probably use a ferrite bead for the EMI filter next time, as ringing could be a problem and the fact that the solar splash competition requires system voltages below 50V could be problematic if the team decided to switch to a 48V battery pack configuration.
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

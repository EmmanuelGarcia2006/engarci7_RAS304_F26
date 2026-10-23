## Overview

This schematic is designed to support the **Grip Actuation Node subsystem**. 
* **Functionality:** Provides automatic bidirectional motor drive control alongside a dual-stage hardware overcurrent/overload safety monitoring loop.
* **Power:** Utilizes an onboard LM7805 linear voltage regulator to step down the incoming +12V motor supply into a stable +5V digital logic rail.
* **Controller:** Interfaces directly with the Microchip PIC18F57Q43 microcontroller via dedicated PWM, Direction, ADC, and Interrupt pins.

![Grip Actuation Node Schematic](./schematic.png)
*Figure 1: Showing the completed schematic layout for the Grip Actuation Node.*

## Resources

The schematic as a PDF download is available [here](./Subsystem_Schematic_Design_Package.pdf), and the Zip folder of the project [here](./project_folder.zip).

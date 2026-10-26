## Block Diagram & Hardware Architecture

This section details the hardware architecture, signal routing, and subsystem breakdown for **Team 104: Grip Actuation Node**.

### Purpose of the Block Diagram
The purpose of this block diagram is to map out the hardware architecture, signal paths, and power distribution for the Grip Actuation Node. It serves as a visual blueprint to ensure proper hardware integration across the following core elements:

*    **Power Source & Levels:** The node is powered by an external **9V 3A unregulated wall adapter**. This primary source is stepped down using a **5V 1.5A voltage regulator (LM7805)** to safely drive the microcontroller and logic circuitry, isolating the high-power motor lines from sensitive electronics.
*    **Sensor Integration:** For closed-loop safety monitoring, a **low-side shunt resistor** measures current draw from the motor. This analog signal is amplified through a **Texas Instruments TLV9002 Op-Amp array (Comparator)** and read by the microcontroller's ADC to trigger immediate overload protection if values spike.
*    **Actuator Control:** Physical gripping action is executed by a **Pololu 12V DC Motor & Lead-Screw assembly**. The microcontroller governs this actuator using PWM and direction signals routed through a **Cytron MD13S Bidirectional H-Bridge** motor driver.
*    **Team & Node Connections:** Inter-node communication and system-wide telemetry are facilitated through a centralized multi-pin connector. This links the Grip Actuation Node directly to the rest of the team's decentralized network (Master, Tactile, Proximity, and Slip nodes) over shared digital and analog lines.

---

### System Block Diagram

>  **Note:** Update your local repository or rename the file if it does not display properly due to the bracket spacing characters in the URL string below.

![Grip Actuation Node Block Diagram](./grip_node_block_diagram%20\(1\).png)

### Core Component Breakdown

| Subsystem | Component | Manufacturer | Part Number | Signaling / Interface |
| :--- | :--- | :--- | :--- | :--- |
| **Microcontroller** | PIC18F57Q43 Curiosity Nano | Microchip | PIC18F57Q43 | Main Logic Control |
| **Actuator Driver** | Bidirectional H-Bridge | Cytron | MD13S | PWM & Digital Direction (5V) |
| **Actuator** | DC Motor & Lead-Screw | Pololu | #4753 (12V) | Analog Motor Power (0-12VDC) |
| **Current Sensor** | Low-Side Shunt Resistor | Yageo | RL2512 | Analog (0-100mV) |
| **Signal Amplifier** | Op-Amp Array (Comparator) | Texas Instruments | TLV9002 | Digital Output to ADC1 (1 pin) |
| **Voltage Regulator** | 5V 1.5A Linear Regulator | Texas Instruments | LM7805 | Power Step-Down |

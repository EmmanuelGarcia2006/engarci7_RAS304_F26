
# Individual Component Selection Report

**Name:** Emmanuel Garcia  
**Assigned Module:** Board 2 – Grip Actuation Node  
**Course:** RAS 304 Embedded Systems Design

---

# Part 1: Major Component Selections

## Subsystem 1: Bidirectional H-Bridge Motor Driver

This subsystem converts microcontroller control signals into the electrical power needed to operate the DC motor and lead-screw assembly. The H-bridge enables forward and reverse motor operation, allowing the gripper to open and close. PWM signals provide motor speed control.

| Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: Cytron MD13S Module**<br>Estimated Cost: $15.30<br>[Product Reference](https://www.cytron.io/) | • MOSFET-based switching provides efficient motor control.<br>• Supports bidirectional DC motor operation.<br>• Integrated driver simplifies wiring and prototyping. | • Higher cost than a basic motor driver IC.<br>• Requires additional enclosure or PCB space.<br>• Motor stall current must remain within the driver's ratings. |
| **Option 2: L298N Dual H-Bridge Driver**<br>Estimated Cost: $3.15<br>[Product Reference](https://www.digikey.com/) | • Low cost and widely documented.<br>• Supports forward and reverse motor operation.<br>• Can control two DC motors independently. | • Bipolar output stages create significant voltage drops.<br>• Generates more heat than modern MOSFET drivers.<br>• Lower efficiency can reduce battery runtime. |
| **Option 3: Custom Discrete MOSFET H-Bridge**<br>Estimated Cost: $4.20<br>[Design Reference](https://www.electronics-tutorials.ws/) | • Customizable for the motor and PCB layout.<br>• Can achieve low conduction losses with suitable MOSFETs.<br>• Offers flexibility for future design changes. | • Requires additional components and gate-drive circuitry.<br>• Incorrect switching can cause destructive shoot-through.<br>• Requires more design, testing, and debugging. |

### Final Selection: Option 1 — Cytron MD13S Module

The Cytron MD13S is selected because it provides an integrated solution for bidirectional DC motor control and simplifies hardware development. Its MOSFET-based design offers efficient switching while reducing the complexity of designing and troubleshooting a discrete H-bridge.

Before finalizing the design, the selected motor's rated current, startup current, and stall current must be checked against the driver's continuous and peak current ratings. Appropriate overcurrent protection is also required to prevent damage if the gripper becomes mechanically jammed.

---

## Subsystem 2: Motor Current Sensing and Overload Protection

This subsystem measures motor current to estimate the actuator load and detect possible mechanical stalls. A low-side shunt resistor produces a small voltage proportional to motor current. An amplifier conditions the signal for the microcontroller's analog-to-digital converter (ADC), while a threshold-detection circuit can generate a digital overload alert.

| Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: Texas Instruments TLV9002**<br>Estimated Cost: $0.83<br>[Product Datasheet](https://www.ti.com/product/TLV9002) | • Dual rail-to-rail operational amplifier.<br>• Can amplify a shunt-resistor voltage and support threshold detection with external components.<br>• Low power consumption and low component cost. | • Requires external resistors and a suitable reference voltage.<br>• Is not a dedicated current-sense amplifier or comparator.<br>• Input/output limits and response time must be evaluated. |
| **Option 2: Analog Devices AD8210**<br>Estimated Cost: $7.85<br>[Product Reference](https://www.analog.com/) | • Dedicated current-sense amplifier.<br>• Designed for shunt-voltage measurement in high common-mode-voltage applications.<br>• Reduces the need to design a discrete amplifier stage. | • Higher component cost.<br>• Requires a separate threshold detector for a digital overload alert.<br>• Package and assembly requirements must be checked. |
| **Option 3: Discrete BJT Threshold Circuit**<br>Estimated Cost: $0.35 | • Low component cost.<br>• Uses basic electronic components.<br>• Can provide a simple threshold-triggered output. | • Trip point can vary with transistor characteristics and temperature.<br>• Provides limited measurement accuracy.<br>• Requires careful design to avoid unreliable triggering. |

### Final Selection: Option 1 — Texas Instruments TLV9002

The TLV9002 is selected as a cost-effective candidate because its two operational amplifier channels can support signal amplification and threshold detection when used with an appropriate external circuit.

One channel can amplify the voltage across the current-sense shunt resistor, while the second can be configured for threshold detection. The microcontroller can read the conditioned analog current signal and respond to a digital overload alert if that circuit is implemented.

The TLV9002 is an operational amplifier, not a dedicated comparator. Its input range, output behavior, response time, and recovery from saturation must be verified before using it as an overload detector. A dedicated comparator may be a better alternative if faster or more predictable fault detection is required.

The overload threshold will be established after the selected motor's current characteristics have been verified. If excessive current is detected, the controller should disable the motor driver and enter a safe state.

---

## Subsystem 3: DC Motor and Lead-Screw Assembly

This subsystem provides the mechanical torque required to actuate the gripper fingers. A 12 V DC gearmotor drives a lead screw through a compatible shaft coupler, converting rotational motion into linear movement. The motor is controlled by the Cytron MD13S H-Bridge, allowing bidirectional operation for opening and closing the gripper.

| Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: Pololu 25D Metal Gearmotor**<br>Estimated Cost: $19.95<br>[Product Link](https://www.pololu.com/category/115/25d-metal-gearmotors) | • Metal gearbox provides durability and multiple gear-ratio options.<br>• Standard 4 mm D-shaped output shaft supports compatible couplers.<br>• Multiple motor configurations allow flexibility when balancing speed and torque. | • Requires a separate shaft coupler, lead screw, and mechanical supports.<br>• Requires a suitable mounting bracket.<br>• Stall current varies by motor version and must be verified. |
| **Option 2: N20 Micro Gearmotor with Lead-Screw Mechanism**<br>Estimated Cost: $9.50<br>[Product Reference](https://www.pololu.com/category/60/micro-metal-gearmotors) | • Compact and lightweight design.<br>• Fits smaller gripper assemblies.<br>• Lower estimated cost. | • May not provide sufficient torque for higher gripping forces.<br>• Small gears and shafts may be less suitable for high mechanical loads.<br>• May require custom lead-screw mounting and alignment. |
| **Option 3: Progressive Automations Micro Linear Actuator**<br>Estimated Cost: $55.00<br>[Product Link](https://www.progressiveautomations.com/) | • Integrates a motor and linear drive mechanism.<br>• Reduces the need for a custom motor-to-screw coupling.<br>• Some models include built-in limit switches. | • Higher cost than a separate gearmotor and lead screw.<br>• Fixed dimensions may restrict the gripper layout.<br>• Speed, stroke, voltage, and load ratings depend on the selected model. |

### Final Selection: Option 1 — Pololu 25D Metal Gearmotor

The Pololu 25D metal gearmotor is selected as the preferred candidate because it offers a durable gearbox, a standard 4 mm D-shaped output shaft, and multiple gear ratios for balancing torque and speed. Its modular design allows the team to customize the lead-screw mechanism to meet the gripper's mechanical requirements.

A compatible shaft coupler will connect the motor output to the lead screw. Mechanical supports will maintain alignment and reduce unwanted loads on the gearbox shaft. The gear ratio and lead-screw pitch will be selected according to the required gripping force and opening/closing speed.

The exact 12 V motor variant must be verified before purchase. Different Pololu 25D variants have different stall-current ratings, so the selected motor must be compatible with the Cytron MD13S driver's ratings and the power supply's current capabilities.

The current-sensing subsystem will monitor motor load and help detect stalls or excessive gripping force. When a defined fault threshold is reached, the controller should stop or reverse the motor instead of allowing a prolonged stall.

**Design Considerations:**
- Verify motor voltage, gear ratio, rated current, and stall current.
- Select a compatible shaft coupler and lead screw.
- Include suitable mechanical supports and mounting hardware.
- Add limit switches or another reliable method to prevent excessive travel.
- Establish and test a safe overload threshold.
- Confirm that the motor driver, wiring, and power supply support expected operating currents.

---

# Microcontroller Pin Mapping

The following pin assignments are proposed for the grip actuation node. They must be verified against the exact microcontroller model and its datasheet before implementation.

| Microcontroller Pin | Subsystem Connection | Signal Direction | Signal Type | Functional Description |
| :--- | :--- | :--- | :--- | :--- |
| **RC0** | H-Bridge | Output | PWM | Controls motor speed through duty-cycle modulation. |
| **RC1** | H-Bridge | Output | Digital GPIO | Selects motor direction. |
| **RA1** | Current-Sense Amplifier | Input | Analog ADC | Measures the conditioned motor-current signal. |
| **RE1** | Overload Detection Circuit | Input | Digital GPIO/Interrupt | Signals an overcurrent or stall condition. |
| **RD2** | Board 1 Hub Connector | Input | UART RX, proposed | Receives operating commands from the bus master. |
| **RD3** | Board 1 Hub Connector | Output | UART TX, proposed | Sends status information to the bus master. |
| **RB0** | Board 1 Hub Connector | Input | Digital GPIO | Receives the system-wide emergency-stop signal. |

### Pin Mapping Implementation Notes

- Confirm that the selected microcontroller supports the listed functions on these pins.
- Configure the PWM peripheral rather than assuming every GPIO pin supports hardware PWM.
- Verify UART pin assignments and peripheral routing for the exact microcontroller.
- Use appropriate signal conditioning and protection for the current-sense and overload inputs.
- Design the emergency-stop function so that a fault places the motor driver in a safe state rather than relying exclusively on software.
- Verify the electrical voltage levels between the microcontroller, motor driver, and other connected boards.

---

# Part 2: Power Budget and Voltage Regulator Selection

The voltage regulator supplies a stable 5 V logic rail for the microcontroller and current-sensing circuitry. The motor should use a separate, appropriately rated power path, with a common ground where required by the selected driver and sensing design.

| Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: STMicroelectronics L7805 / LM7805**<br>Estimated Cost: $0.62<br>[Product Datasheet](https://www.st.com/en/power-management/l78.html) | • Simple three-terminal linear regulator.<br>• Provides a regulated 5 V output.<br>• Includes thermal overload and short-circuit protection. | • Converts excess input voltage into heat.<br>• Less efficient than a switching regulator.<br>• Requires appropriate input/output capacitors and thermal design. |
| **Option 2: Murata OKI-78SR-5/1.5-W36-C**<br>Estimated Cost: $4.85<br>[Product Reference](https://www.murata.com/) | • Switching step-down regulator offers higher efficiency.<br>• Reduces heat compared with a linear regulator for large voltage drops.<br>• Provides a compact power-conversion solution. | • Higher component cost.<br>• Switching ripple may affect sensitive analog measurements.<br>• Requires suitable decoupling and PCB layout practices. |

### Final Selection: Option 1 — L7805 / LM7805 Linear Regulator

The L7805 is selected as the initial logic-rail regulator because the estimated current consumption of the microcontroller and analog sensing circuitry is low. A linear regulator offers a simple design with few external components.

For a 12 V input, 5 V output, and 50 mA load, the approximate regulator power dissipation is:

\[
P_{\text{loss}}=(V_{\text{in}}-V_{\text{out}})I_{\text{out}}
\]

\[
P_{\text{loss}}=(12-5)(0.05)=0.35\text{ W}
\]

This is the estimated power converted into heat under those conditions. Whether a heatsink is required depends on the regulator package, ambient temperature, PCB layout, and thermal resistance.

The linear regulator's simplicity is useful for a low-current prototype. However, its efficiency is limited when stepping down from 12 V to 5 V. Its thermal performance must be evaluated under actual operating conditions.

If battery runtime or heat becomes a major concern, the Murata switching regulator should be reconsidered. Switching noise can be managed through appropriate decoupling, grounding, and PCB layout.

**Power Design Considerations:**
- Calculate the total logic-rail current before selecting the regulator.
- Verify the regulator's input-voltage range and thermal limits.
- Use the datasheet-recommended input and output capacitors.
- Keep motor power and logic power appropriately separated.
- Account for motor startup and stall current when sizing the battery, wiring, and driver.
- Ensure that the regulated logic supply remains stable during motor operation.

---

# Updated Component Selection Summary

| Subsystem | Selected Component | Primary Purpose |
| :--- | :--- | :--- |
| Bidirectional Motor Driver | Cytron MD13S Module | Controls motor direction and speed. |
| Current Sensing and Overload Detection | Texas Instruments TLV9002 | Supports current measurement and threshold detection with external circuitry. |
| DC Motor and Lead-Screw Assembly | Pololu 25D Metal Gearmotor | Provides mechanical torque to actuate the gripper. |
| 5 V Voltage Regulation | L7805 / LM7805 | Supplies regulated voltage to low-current logic circuitry. |

---

# Estimated Component Budget

| Component | Estimated Cost |
| :--- | ---: |
| Cytron MD13S Motor Driver | $15.30 |
| Texas Instruments TLV9002 | $0.83 |
| Pololu 25D Metal Gearmotor | $19.95 |
| L7805 / LM7805 Regulator | $0.62 |
| **Estimated Subtotal** | **$36.70** |

**Budget Note:** The estimated subtotal excludes the lead screw, shaft coupler, mounting hardware, limit switches, current-sense shunt resistor, passive components, wiring, and power supply. Actual prices and component availability must be confirmed before purchasing.

---

# Conclusion

Board 2 is responsible for controlling the robotic gripper's actuator, monitoring motor load, and communicating operating status to the system's bus master.

The selected components prioritize ease of integration, cost, and flexibility during prototyping. The Cytron MD13S provides bidirectional motor control, the TLV9002 is a candidate for the current-sensing and threshold-detection circuit, the Pololu 25D gearmotor provides mechanical drive, and the L7805 is a candidate for the low-current 5 V logic supply.

Before final implementation, the team must verify motor current requirements, driver compatibility, overload response, regulator thermal performance, and microcontroller pin assignments. The mechanical assembly must also be tested to confirm that the lead screw produces the required gripping force and travel without exceeding safe operating limits.

The final design should include a reliable method for stopping the motor during an overload, detecting excessive travel, and entering a safe state during an emergency stop.

*Note: All listed prices are estimates for comparison. Final component selection must be based on verified datasheets, exact model numbers, and the electrical and mechanical requirements of the assembled gripper.*

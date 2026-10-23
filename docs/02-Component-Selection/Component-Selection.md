
# RAS 304 — Individual Component Selection Report

**Name:** Emmanuel Garcia  
**Assigned Module:** Board 2 – Grip Actuation Node  
**Course:** RAS 304 Embedded Systems Design

---

# Part 1: Major Component Selections

## Subsystem 1: Bidirectional H-Bridge Motor Driver

The H-bridge converts microcontroller control signals into power for the DC motor. It supports forward and reverse operation and PWM speed control for opening and closing the gripper.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: Cytron MD13S**<br>![Cytron MD13S motor driver](https://www.cytron.io/image/catalog/products/MD13S/MD13S-1.jpg)<br>Estimated cost: $15.30<br>[Product page](https://www.cytron.io/) | • MOSFET-based switching.<br>• Bidirectional motor control.<br>• Simplifies wiring. | • Higher cost than a basic driver IC.<br>• Requires enclosure space.<br>• Stall-current compatibility must be verified. |
| **Option 2: L298N H-Bridge**<br>![L298N H-bridge module](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/L298N_Motor_Driver_Module.jpg/320px-L298N_Motor_Driver_Module.jpg)<br>Estimated cost: $3.15<br>[Product reference](https://www.digikey.com/) | • Low cost.<br>• Widely documented.<br>• Supports two motor channels. | • Significant voltage drop.<br>• More heat.<br>• Lower efficiency. |
| **Option 3: Discrete MOSFET H-Bridge**<br>![MOSFET transistor](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/TO-220_package.jpg/320px-TO-220_package.jpg)<br>Estimated cost: $4.20<br>[Circuit reference](https://www.electronics-tutorials.ws/) | • Customizable design.<br>• Potentially low conduction losses.<br>• Flexible PCB layout. | • More complex design.<br>• Shoot-through risk.<br>• More testing required. |

### Final Selection: Cytron MD13S

The Cytron MD13S is the preferred candidate because it integrates the power switching required for bidirectional DC motor control. The final motor variant must be checked against the driver's continuous and peak current ratings before implementation.

---

## Subsystem 2: Current Sensing and Overload Protection

This subsystem measures motor current using a shunt resistor and a signal-conditioning circuit. The microcontroller reads the current measurement and responds to an overload condition.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: Texas Instruments TLV9002**<br>![TLV9002 operational amplifier](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Integrated_Circuit.jpg/320px-Integrated_Circuit.jpg)<br>Estimated cost: $0.83<br>[Official datasheet](https://www.ti.com/product/TLV9002) | • Dual operational amplifier.<br>• Can amplify a shunt voltage.<br>• Low component cost. | • Requires external circuitry.<br>• Not a dedicated comparator.<br>• Input/output limits must be checked. |
| **Option 2: Analog Devices AD8210**<br>![Current sensing integrated circuit](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Integrated_Circuit.jpg/320px-Integrated_Circuit.jpg)<br>Estimated cost: $7.85<br>[Manufacturer reference](https://www.analog.com/) | • Dedicated current-sense amplifier.<br>• Designed for shunt measurements.<br>• Simplifies signal amplification. | • Higher cost.<br>• Requires a separate threshold detector.<br>• Package availability must be verified. |
| **Option 3: Discrete BJT Threshold Circuit**<br>![TO-220 transistor](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/TO-220_package.jpg/320px-TO-220_package.jpg)<br>Estimated cost: $0.35<br>[Circuit reference](https://www.electronics-tutorials.ws/) | • Low cost.<br>• Basic components.<br>• Simple threshold concept. | • Temperature-sensitive threshold.<br>• Lower measurement accuracy.<br>• Requires careful tuning. |

### Final Selection: Texas Instruments TLV9002

The TLV9002 is a candidate for the current-sensing circuit because its two amplifier channels can support signal amplification and threshold detection with appropriate external components. The circuit must be designed and tested for the selected motor's current range.

The TLV9002 is an operational amplifier, not a dedicated comparator. If fast fault detection is required, a dedicated comparator may be more suitable.

---

## Subsystem 3: DC Motor and Lead-Screw Assembly

This subsystem supplies the torque required to actuate the gripper fingers. The gearmotor drives a lead screw through a compatible shaft coupler, converting rotational motion into linear movement.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: Pololu 25D Metal Gearmotor**<br>![25D metal gearmotor](https://www.pololu.com/picture/view/0J4898.1200.jpg)<br>Estimated cost: $19.95<br>[Product page](https://www.pololu.com/category/115/25d-metal-gearmotors) | • Metal gearbox.<br>• Multiple gear ratios.<br>• Standard 4 mm D-shaped shaft on applicable versions. | • Requires lead screw and coupler.<br>• Needs a mounting bracket.<br>• Stall current depends on motor variant. |
| **Option 2: N20 Micro Gearmotor**<br>![N20 micro gearmotor](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/Small_DC_Motor.jpg/320px-Small_DC_Motor.jpg)<br>Estimated cost: $9.50<br>[Product reference](https://www.pololu.com/category/60/micro-metal-gearmotors) | • Compact design.<br>• Lightweight.<br>• Low estimated cost. | • May provide insufficient gripping torque.<br>• Small gears may be less suitable for heavy loads.<br>• Requires mechanical integration. |
| **Option 3: Micro Linear Actuator**<br>![Linear actuator](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Linear_actuator.jpg/320px-Linear_actuator.jpg)<br>Estimated cost: $55.00<br>[Manufacturer reference](https://www.progressiveautomations.com/) | • Integrated linear drive.<br>• Simplifies mechanical assembly.<br>• Some models include limit switches. | • Higher cost.<br>• Fixed dimensions.<br>• Specifications vary by model. |

### Final Selection: Pololu 25D Metal Gearmotor

The Pololu 25D gearmotor is the preferred candidate because it offers multiple gear ratios and a durable gearbox. A compatible shaft coupler, lead screw, and mechanical supports will be required.

The motor's voltage, gear ratio, rated current, and stall current must be verified before purchase. The overload circuit should stop the motor when excessive current is detected. Limit switches or another suitable travel-limiting mechanism should prevent excessive movement.

---

# Microcontroller Pin Mapping

These are proposed assignments and must be verified against the exact microcontroller model.

| Pin | Connection | Direction | Signal | Function |
|---|---|---|---|---|
| RC0 | H-Bridge | Output | PWM | Motor speed control. |
| RC1 | H-Bridge | Output | GPIO | Motor direction. |
| RA1 | Current-sense circuit | Input | ADC | Measures motor current. |
| RE1 | Overload detector | Input | GPIO/interrupt | Detects an overload alert. |
| RD2 | Board 1 hub | Input | UART RX | Receives commands. |
| RD3 | Board 1 hub | Output | UART TX | Sends status information. |
| RB0 | Board 1 hub | Input | GPIO | Emergency-stop signal. |

Verify the pin functions, voltage levels, and peripheral routing for the selected microcontroller before wiring.

---

# Part 2: Power Budget and Voltage Regulation

The regulator provides a stable 5 V logic supply. The motor must use a suitably rated motor power path.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: L7805 / LM7805**<br>![L7805 voltage regulator](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/TO-220_package.jpg/320px-TO-220_package.jpg)<br>Estimated cost: $0.62<br>[Manufacturer reference](https://www.st.com/en/power-management/l78.html) | • Simple three-terminal regulator.<br>• Regulated 5 V output.<br>• Thermal and short-circuit protection. | • Converts excess voltage into heat.<br>• Lower efficiency than switching regulators.<br>• Requires thermal evaluation. |
| **Option 2: Murata OKI-78SR-5/1.5-W36-C**<br>![Switching voltage regulator](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Integrated_Circuit.jpg/320px-Integrated_Circuit.jpg)<br>Estimated cost: $4.85<br>[Manufacturer reference](https://www.murata.com/) | • Switching conversion is generally more efficient.<br>• Reduces heat for large voltage drops.<br>• Compact power-conversion solution. | • Higher cost.<br>• Switching ripple may affect analog measurements.<br>• Requires suitable decoupling and layout. |

### Final Selection: L7805 / LM7805

The L7805 is a candidate for the initial low-current logic rail because it is simple to integrate. Its thermal performance must be checked using the actual input voltage and load current.

For a 12 V input, 5 V output, and 50 mA load:

\[
P_{\text{loss}}=(12-5)(0.05)=0.35\text{ W}
\]

The regulator dissipates approximately 0.35 W as heat under these assumptions. A heatsink requirement depends on the package and operating conditions.

A switching regulator should be reconsidered if battery runtime or heat becomes a major concern.

---

# Estimated Component Budget

| Component | Estimated Cost |
|---|---:|
| Cytron MD13S | $15.30 |
| TLV9002 | $0.83 |
| Pololu 25D gearmotor | $19.95 |
| L7805 / LM7805 | $0.62 |
| **Estimated subtotal** | **$36.70** |

The subtotal excludes the lead screw, shaft coupler, mounting hardware, limit switches, shunt resistor, passive components, wiring, and power supply. Prices must be confirmed before purchasing.

---

# Conclusion

Board 2 controls the gripper actuator, monitors motor load, and communicates operating status to the bus master. The proposed components prioritize cost, integration, and flexibility during prototyping.

Before final implementation, the team must verify motor current requirements, driver compatibility, overload response, regulator thermal performance, and microcontroller pin assignments. The assembled lead-screw mechanism must also be tested to ensure that it produces the required gripping force and travel safely.

*Image note: The images above are illustrative references and may not depict the exact model listed. Confirm all component specifications using the linked manufacturer documentation.*


The final design should include a reliable method for stopping the motor during an overload, detecting excessive travel, and entering a safe state during an emergency stop.

*Note: All listed prices are estimates for comparison. Final component selection must be based on verified datasheets, exact model numbers, and the electrical and mechanical requirements of the assembled gripper.*

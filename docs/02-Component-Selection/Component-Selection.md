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
| **Option 1: Cytron MD13S**<br>![Cytron MD13S motor driver](images/md13s.jpg)<br>Estimated cost: $15.30 | • MOSFET-based switching.<br>• Bidirectional motor control.<br>• Simplifies wiring. | • Higher cost than a basic driver IC.<br>• Requires enclosure space.<br>• Stall-current compatibility must be verified. |
| **Option 2: L298N H-Bridge**<br>![L298N H-bridge module](images/l298n.jpg)<br>Estimated cost: $3.15 | • Low cost.<br>• Widely documented.<br>• Supports two motor channels. | • Significant voltage drop.<br>• More heat.<br>• Lower efficiency. |
| **Option 3: Discrete MOSFET H-Bridge**<br>![MOSFET transistor](images/mosfet.jpg)<br>Estimated cost: $4.20 | • Customizable design.<br>• Potentially low conduction losses.<br>• Flexible PCB layout. | • More complex design.<br>• Shoot-through risk.<br>• More testing required. |

### Final Selection: Cytron MD13S

The Cytron MD13S is the preferred candidate because it integrates the power switching required for bidirectional DC motor control. The final motor variant must be checked against the driver's continuous and peak current ratings before implementation.

---

## Subsystem 2: Current Sensing

The current sensor monitors the motor's power consumption to detect mechanical stalls when the gripper has firmly closed around an object. It provides feedback to the microcontroller to prevent motor burnout and ensure controlled gripping force.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: ACS712 Hall-Effect Sensor**<br>Estimated cost: $4.50 | • Galvanic isolation protects MCU.<br>• Low internal resistance.<br>• Simple analog voltage output. | • Susceptible to stray magnetic fields.<br>• High zero-current offset drift.<br>• Lower resolution at very small currents. |
| **Option 2: INA219 High-Side Shunt Monitor**<br>Estimated cost: $5.20 | • High accuracy via I2C digital output.<br>• Measures both current and bus voltage.<br>• Programmable calibration. | • Lacks galvanic isolation.<br>• Requires software overhead for I2C communication.<br>• Solder pads are small for basic prototyping. |
| **Option 3: Discrete Shunt Resistor & Op-Amp (TLV9002)**<br>Estimated cost: $0.83 (Op-Amp only) | • Extremely cost-effective.<br>• Highly customizable amplification gain.<br>• Minimal PCB footprint. | • High design complexity.<br>• Noise amplification issues.<br>• Requires precise component matching. |

### Final Selection: Discrete Shunt Resistor & Op-Amp (TLV9002)

The Discrete Shunt Resistor paired with the TLV9002 operational amplifier is selected to keep the board footprint and hardware budget as minimal as possible during prototyping. 

---

## Subsystem 3: Voltage Regulation

The voltage regulator steps down the incoming motor power bus supply to a stable 5 V logic rail to power the logic gates, sensors, and microcontroller instrumentation.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: L7805 / LM7805 Linear Regulator**<br>Estimated cost: $0.62 | • Simple drop-in design.<br>• Minimal external component requirements.<br>• Extremely low electrical output noise. | • Very low efficiency under high step-down ratios.<br>• High heat dissipation limits current capacity.<br>• Requires thermal heatsinking under load. |
| **Option 2: Murata OKI-78SR-5/1.5-W36-C**<br>![Switching voltage regulator](images/murata.jpg)<br>Estimated cost: $4.85 | • Switching conversion is generally more efficient.<br>• Reduces heat for large voltage drops.<br>• Compact power-conversion solution. | • Higher cost.<br>• Switching ripple may affect analog measurements.<br>• Requires suitable decoupling and layout. |

### Final Selection: L7805 / LM7805

The L7805 is a candidate for the initial low-current logic rail because it is simple to integrate. Its thermal performance must be checked using the actual input voltage and load current.

For a 12 V input, 5 V output, and 50 mA load:

$$P_{\text{loss}}=(12-5)(0.05)=0.35\text{ W}$$

The regulator dissipates approximately 0.35 W as heat under these assumptions. A heatsink requirement depends on the package and operating conditions.

A switching regulator should be reconsidered if battery runtime or heat becomes a major concern.

---

# Estimated Component Budget

| Component | Estimated Cost |
|---|---:|
| Cytron MD13S | $15.30 |
| TLV9002 Op-Amp | $0.83 |
| Pololu 25D gearmotor | $19.95 |
| L7805 / LM7805 | $0.62 |
| **Estimated subtotal** | **$36.70** |

The subtotal excludes the lead screw, shaft coupler, mounting hardware, limit switches, shunt resistor, passive components, wiring, and power supply. Prices must be confirmed before purchasing.

---

# Conclusion

Board 2 controls the gripper actuator, monitors motor load, and communicates operating status to the bus master. The proposed components prioritize cost, integration, and flexibility during prototyping.

Before final implementation, the team must verify motor current requirements, driver compatibility, overload response, regulator thermal performance, and microcontroller pin assignments. The assembled lead-screw mechanism must also be tested to ensure that it produces the required gripping force and travel safely.

The final design should include a reliable method for stopping the motor during an overload, detecting excessive travel, and entering a safe state during an emergency stop.

*Note: All listed prices are estimates for comparison. Final component selection must be based on verified datasheets, exact model numbers, and the electrical and mechanical requirements of the assembled gripper.*


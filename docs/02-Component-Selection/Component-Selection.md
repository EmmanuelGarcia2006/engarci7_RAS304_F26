# Individual Component Selection Report
**Your Name:** Emmanuel Garcia  
**Your Assigned Module:** Board 2 – Grip Actuation Node  
**Course:** RAS 304 Embedded Systems Design  

---

## Part 1: Major Component Selections

### Subsystem 1: Bidirectional H-Bridge
This subsystem translates logic-level PWM and direction signals from the microcontroller into high-current polarity-reversing drive paths for the DC Grip Actuator Motor and Lead-Screw assembly.

| Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: Cytron MD13S Module**<br>• Unit Cost: $15.30<br>• [Product Link](https://makermotor.com)<br><img src="https://makermotor.com" width="100"> | • High continuous current handling up to 13A easily covers motor stall conditions.<br>• Through-hole header connections allow for easy prototyping and soldering.<br>• Employs a full NMOS design that generates minimal operational heat. | • Physical module size takes up significant real estate inside a tight enclosure.<br>• Large screw terminals add weight and layout bulk to the PCB design.<br>• Unit cost is substantially higher than deploying discrete IC components. |
| **Option 2: L298N Dual H-Bridge Driver**<br>• Unit Cost: $3.15<br>• [Product Link](https://digikey.com)<br><img src="https://digikey.com" width="100"> | • Classic Multiwatt-15 package features wide, easy-to-solder through-hole pins.<br>• Extensively documented with standard TTL logic interfaces.<br>• Provides independent dual-channel architecture for driving secondary actuators if needed. | • Outdated bipolar transistor technology causes a high internal forward voltage drop.<br>• Dissipates large amounts of waste energy entirely as heat, requiring a bulky external heatsink.<br>• Inefficient power conversion reduces total battery efficiency. |
| **Option 3: Custom Discrete H-Bridge Circuit**<br>• Unit Cost: $4.20 ~<br>• <br><img src="https://electronics-tutorials.ws" width="120"> | • Features fully customizable through-hole TO-220 power MOSFET profiles.<br>• Ultra-low channel resistance minimizes thermal dissipation sink needs.<br>• Maximizes physical placement paths on the custom printed board. | • Introduces a severe risk of destructive shoot-through short circuits if PWM control logic overlaps.<br>• Requires an elevated discrete component count including independent gate-drivers and protection flyback diodes.<br>• Greatly increases hardware debugging time if trace parasitics compromise switching speeds. |

#### Final Selection for Subsystem 1:
* **Choice:** Option 1: Cytron MD13S Module
* **Rationale:** The Cytron MD13S is selected as our optimal choice because its integrated NMOS design safely handles up to 13A continuously, making it completely immune to the high thermal stress and physical stall currents typical of our gripper's high-torque lead-screw assembly. While the L298N is cheaper, its outdated bipolar design drops significant voltage internally and generates excessive waste heat, which would require a heavy, space-wasting external heatsink. Furthermore, using this pre-certified module isolates high-current inductive switching noise away from our primary custom PCB routing layers.

---

### Subsystem 2: Current Sense & Overload Module
This module utilizes a low-side shunt setup to capture active motor current spikes and sets an immediate digital hardware alert flag using a dedicated comparator loop before a mechanical failure occurs.

| Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: Texas Instruments TLV9002**<br>• Unit Cost: $0.83<br>• [Product Link](https://digikey.com)<br><img src="https://digikey.com" width="100"> | • Dual rail-to-rail configuration fits both current shunt amplification and comparator roles simultaneously.<br>• Integrated RFI and EMI filter circuitry limits motor switching noise injection.<br>• Very low unit cost fits tight project budgets. | • Requires a complex external network of discrete tuning resistors to establish trip thresholds.<br>• Surface-mount footprints can be challenging to hand-solder without breakout adapters.<br>• Low 1MHz bandwidth bounds fast high-frequency peak tracking speeds. |
| **Option 2: Analog Devices AD8210**<br>• Unit Cost: $7.85<br>• [Product Link](https://analog.com)<br><img src="https://analog.com" width="100"> | • High-precision dedicated architecture offers highly accurate shunt tracking capabilities.<br>• Provides reliable handling of high common-mode voltage fluctuations during inductive kicks.<br>• Minimizes additional hardware overhead requirements across analog trace paths. | • The component is primarily sold in surface-mount formats, requiring an assembly adapter board.<br>• High baseline cost budget allocation for a basic current feedback interface function.<br>• Lacks an integrated threshold comparator, necessitating a secondary IC component. |
| **Option 3: Custom Discrete BJT Threshold Switch**<br>• Unit Cost: $0.35 ~<br>•  | • Absolute lowest baseline components cost metric.<br>• Dirt-simple routing architecture via basic components.<br>• Avoids active operational amplifier chip allocations. | • Highly vulnerable to thermal drifts shifting trip thresholds.<br>• Soft turn-on regions yield undefined transitional states.<br>• High shunt drop voltage required to trigger base junction. |

#### Final Selection for Subsystem 2:
* **Choice:** Option 1: Texas Instruments TLV9002
* **Rationale:** The TLV9002 is selected because its dual rail-to-rail channels allow us to implement both the low-side current shunt amplification stage and the overcurrent threshold comparator loop using a single, highly cost-effective through-hole integrated circuit. This completely eliminates the extra component count, weight, and board complexity associated with separate packages. While the custom BJT switch circuit is cheaper, its trip threshold is highly sensitive to ambient thermal drift, risking accidental emergency-stop triggers during prolonged motor operation.

---

## Microcontroller Pin Mapping Table

| Microcontroller Pin | Subsystem Connection | Signal Direction | Signal Type | Functional Description |
| :--- | :--- | :--- | :--- | :--- |
| **RC0** | Bidirectional H-Bridge | OUTPUT | Digital (PWM) | Controls the speed of the motor via duty-cycle modulation |
| **RC1** | Bidirectional H-Bridge | OUTPUT | Digital (GPIO) | Sets motor directional polarity (High/Low) |
| **RA1** | Current Sense Amp | INPUT | Analog (ADC) | Monitors real-time current scaling for load profiling |
| **RE1** | Overload Comparator | INPUT | Digital (GPIO Interrupt) | Instantly alerts MCU of hard mechanical motor stalls |
| **RD2** | Board 1 Hub Connector | INPUT | Digital (UART) | Receives active operational commands from Bus Master (RX) |
| **RD3** | Board 1 Hub Connector | OUTPUT | Digital (UART) | Transmits status packets back to Sakiya's Master Node (TX) |
| **RB0** | Board 1 Hub Connector | INPUT | Digital (GPIO) | Dedicated system-wide Emergency Stop safety wire (ESTOP) |

---

## Part 2: Power Budget Voltage Regulator Options

| Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: STMicroelectronics LM7805**<br>• Unit Cost: $0.62<br>• [Product Datasheet Link](https://digikey.com)<br><img src="https://digikey.com" width="100"> | • Standard, breadboard-friendly TO-220 through-hole package is very resilient and easy to hand-solder.<br>• Outputs clean, stable, noise-free DC voltage that protects sensitive analog current sensing traces.<br>• Built-in internal thermal overload and short-circuit current limiting safety protection loops. | • Disastrously inefficient when linearizing a wide drop from a 12V input down to a 5V logic line.<br>• Dissipates 100% of dropped power overhead entirely as ambient heat.<br>• Demands dedicated PCB area for a metal heatsink clip if logic current loops scale upward. |
| **Option 2: Murata OKI-78SR-5/1.5-W36-C**<br>• Unit Cost: $4.85<br>• [Product Datasheet Link](https://mouser.com)<br><img src="https://mouser.com" width="100"> | • Exceptional step-down switching power conversion efficiency metric (exceeding 85%+).<br>• Operates at very cool baseline device temperatures without needing physical heatsink footprints.<br>• Drop-in pin-for-pin footprint equivalent compatibility to clean standard LM7805 form factors. | • Introduces high-frequency switching harmonic ripple noise into nearby board layers.<br>• Requires extra external decoupling capacitor consideration to stabilize analog voltage nodes.<br>• Substantially higher module cost impact relative to basic discrete linear chips. |

#### Choice: Option 1: STMicroelectronics LM7805 Linear Regulator
* **Rationale:** The LM7805 is selected as our optimal logic rail regulator because our calculated total microcontroller and op-amp circuit current consumption is minimal (under 50mA). At this low demand, the total power dissipated as heat is tiny (7V × 0.05A = 0.35W), allowing the through-hole TO-220 package to run safely without requiring an external metal heatsink footprint. Choosing this linear architecture provides our current-sense amplification stage with an entirely quiet, ripple-free 5.0V source, avoiding the high-frequency switching noise lines that Option 2 would inject across our analog board layouts.

Update component selection report

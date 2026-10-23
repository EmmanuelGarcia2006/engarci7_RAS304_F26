# Individual Component Selection Report

**Your Name:** Emmanuel Garcia  
**Your Assigned Module:** Board 2 – Grip Actuation Node  
**Course:** RAS 304 Embedded Systems Design

---

# Part 1: Major Component Selections

## Subsystem 1: Bidirectional H-Bridge

This subsystem translates logic-level PWM and direction signals from the microcontroller into high-current polarity-reversing drive paths for the DC Grip Actuator Motor and Lead-Screw assembly.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: Cytron MD13S Module**<br>• Unit Cost: $15.30<br>• [Official Cytron MD13S Product Page](https://cytron.io)<br>![Cytron MD13S motor driver](Image-1.png) | • High continuous current handling up to 13A easily covers motor stall conditions.<br>• Through-hole header connections allow for easy prototyping and soldering.<br>• Employs a full NMOS design that generates minimal operational heat. | • Physical module size takes up significant real estate inside a tight enclosure.<br>• Large screw terminals add weight and layout bulk to the PCB design.<br>• Unit cost is substantially higher than deploying discrete IC components. |
| **Option 2: L298N Dual H-Bridge Driver**<br>• Unit Cost: $3.15<br>• [STMicroelectronics L298N on DigiKey](https://digikey.com)<br>![L298N H-bridge module](Image-2.png) | • Classic Multiwatt-15 package features wide, easy-to-solder through-hole pins.<br>• Extensively documented with standard TTL logic interfaces.<br>• Provides independent dual-channel architecture for driving secondary actuators if needed. | • Outdated bipolar transistor technology causes a high internal forward voltage drop.<br>• Dissipates large amounts of waste energy entirely as heat, requiring a bulky external heatsink.<br>• Inefficient power conversion reduces total battery efficiency. |
| **Option 3: Custom Discrete H-Bridge Circuit**<br>• Unit Cost: $4.20 ~<br>• [Discrete H-Bridge Circuit Reference Design](https://electronics-tutorials.ws) | • Features fully customizable through-hole TO-220 power MOSFET profiles.<br>• Ultra-low channel resistance minimizes thermal dissipation sink needs.<br>• Maximizes physical placement paths on the custom printed board. | • Introduces a severe risk of destructive shoot-through short circuits if PWM control logic overlaps.<br>• Requires an elevated discrete component count including independent gate-drivers and protection flyback diodes.<br>• Greatly increases hardware debugging time if trace parasitics compromise switching speeds. |

### Final Selection for Subsystem 1: Option 1: Cytron MD13S Module

**Rationale:** The Cytron MD13S is selected as our optimal choice because its integrated NMOS design safely handles up to 13A continuously, making it completely immune to the high thermal stress and physical stall currents typical of our gripper's high-torque lead-screw assembly. While the L298N is cheaper, its outdated bipolar design drops significant voltage internally and generates excessive waste heat, which would require a heavy, space-wasting external heatsink. Furthermore, using this pre-certified module isolates high-current inductive switching noise away from our primary custom PCB routing layers.

---

## Subsystem 2: Current Sense & Overload Module

This module utilizes a low-side shunt setup to capture active motor current spikes and sets an immediate digital hardware alert flag using a dedicated comparator loop before a mechanical failure occurs.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: Texas Instruments TLV9002**<br>• Unit Cost: $0.83<br>• [Texas Instruments TLV9002 Product Page](https://ti.com)<br>![TI TLV9002 Op-Amp](Image-TLV9002.png) | • Dual rail-to-rail configuration fits both current shunt amplification and comparator roles simultaneously.<br>• Integrated RFI and EMI filter circuitry limits motor switching noise injection.<br>• Very low unit cost fits tight project budgets. | • Requires a complex external network of discrete tuning resistors to establish trip thresholds.<br>• Surface-mount footprints can be challenging to hand-solder without breakout adapters.<br>• Low 1MHz bandwidth bounds fast high-frequency peak tracking speeds. |
| **Option 2: Analog Devices AD8210**<br>• Unit Cost: $7.85<br>• [Analog Devices AD8210 Current Shunt Page](https://analog.com)<br>![Analog Devices AD8210](Image-AD8210.png) | • High-precision dedicated architecture offers highly accurate shunt tracking capabilities.<br>• Provides reliable handling of high common-mode voltage fluctuations during inductive kicks.<br>• Minimizes additional hardware overhead requirements across analog trace paths. | • The component is primarily sold in surface-mount formats, requiring an assembly adapter board.<br>• High baseline cost budget allocation for a basic current feedback interface function.<br>• Lacks an integrated threshold comparator, necessitating a secondary IC component. |
| **Option 3: Custom Discrete BJT Threshold Switch**<br>• Unit Cost: $0.35 ~<br>• [Discrete BJT Switching Tutorial](https://electronics-tutorials.ws)<br>![Discrete BJT Switch Circuit](Image-BJT-Switch.png) | • Absolute lowest baseline components cost metric.<br>• Dirt-simple routing architecture via basic components.<br>• Avoids active operational amplifier chip allocations. | • Highly vulnerable to thermal drifts shifting trip thresholds.<br>• Soft turn-on regions yield undefined transitional states.<br>• High shunt drop voltage required to trigger base junction. |

### Final Selection for Subsystem 2: Option 1: Texas Instruments TLV9002

**Rationale:** The TLV9002 is selected because its dual rail-to-rail channels allow us to implement both the low-side current shunt amplification stage and the overcurrent threshold comparator loop using a single, highly cost-effective through-hole integrated circuit. This completely eliminates the extra component count, weight, and board complexity associated with separate packages. While the custom BJT switch circuit is cheaper, its trip threshold is highly sensitive to ambient thermal drift, risking accidental emergency-stop triggers during prolonged motor operation.

---

## Subsystem 3: DC Grip Actuator Motor

This subsystem provides the raw mechanical rotational torque required to drive the mechanical linkages, lead screw, and slider assembly for secure physical cargo clamping.

| Solution | Pros | Cons |
|---|---|---|
| **Option 1: Pololu 25D High-Torque Gearmotor**<br>• Unit Cost: $19.95<br>• [Pololu 25D Metal Gearmotors Category](https://pololu.com)<br>![Pololu 25D Motor](Image-Pololu25D.png) | • Integrated steel heavy-duty spur gearbox provides immense mechanical advantage.<br>• Compact 25mm outer diameter mounts easily inside custom structured grip configurations.<br>• Direct face-mounting tapped holes ease concentric coupling setup. | • High stall currents generate immense inductive flyback spikes during motor reversals.<br>• Mechanical brushes experience degradation over extended high-load duties.<br>• Bare metal output shaft requires precision couplers to prevent mechanical binding. |
| **Option 2: Basic Hobby 130-Size DC Motor**<br>• Unit Cost: $2.50<br>• [Brushed 130-Size DC Motors on DigiKey](https://digikey.com)<br>![Hobby 130 DC Motor](Image-Hobby130.png) | • Low-profile design minimizes weight on the distal arm joints.<br>• Dirt-cheap replacement procurement fits lean laboratory development cycles.<br>• Simple two-wire input keeps connectivity issues basic. | • Pathetically low output holding torque cannot maintain heavy structural grasp vectors.<br>• Extremely high loose-speed parameters require an external reduction assembly.<br>• Thin plastic press-fit output shafts warp easily under axial side-loads. |
| **Option 3: Standard Size NEMA 17 Stepper Motor**<br>• Unit Cost: $14.50<br>• [NEMA 17 Stepper Motors Lineup](https://digikey.com)<br>![NEMA 17 Stepper Motor](Image-NEMA17.png) | • Delivers perfect, precise non-accumulating open-loop position profiling paths.<br>• Eliminates the basic requirement for secondary external safety limit endstops.<br>• Solid holding torque signatures lock structural positions firmly when unpowered. | • Severe weight penalty adds excessive stress to the vertical linkage arm structures.<br>• Demands specialized complex phase-step driver indexing configurations instead of raw PWM lanes.<br>• Heavy continuous power drain drops system battery charge rapidly even while idling. |

### Final Selection for Subsystem 3: Option 1: Pololu 25D High-Torque Gearmotor

**Rationale:** The Pololu 25D series gearmotor is chosen because its integrated metal reduction gearbox generates the high linear holding force required to drive our gripper's lead-screw configuration without back-driving or slipping under load. The 130-size motor does not offer enough mechanical advantage to secure objects, while a NEMA 17 stepper introduces too much structural weight and bulk for a distal robot node.

---

## Microcontroller Pin Mapping Table

| Microcontroller Pin | Subsystem Connection | Signal Direction | Signal Type | Functional Description |
|---|---|---|---|---|
| **RC0** | **Bidirectional H-Bridge** | OUTPUT | Digital (PWM) | Controls the speed of the motor via duty-cycle modulation |
| **RC1** | **Bidirectional H-Bridge** | OUTPUT | Digital (GPIO) | Sets motor directional polarity (High/Low) |
| **RA1** | **Current Sense Amp** | INPUT | Analog (ADC) | Monitors real-time current scaling for load profiling |
| **RE1** | **Overload Comparator** | INPUT | Digital (GPIO Interrupt) | Instantly alerts MCU of hard mechanical motor stalls |





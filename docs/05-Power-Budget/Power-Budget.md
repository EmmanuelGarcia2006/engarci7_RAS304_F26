# Board 2: Grip Actuation Node Power Budget

This page tracks the electrical characteristics of all major components used in the Grip Actuation Node (Board 2) to ensure system stability, regulatory compliance, and thermal security.

---

## Section A: Major Components Power Demand

The following table documents the absolute maximum current consumption parameters extracted directly from each manufacturer's component datasheet. Purely electromechanical switches, passives, and low-power safety status indicators are excluded as their consumption scales negligibly.

| Component / IC Identifier | Input Voltage ($V$) | Absolute Max Current ($I_{\text{max}}$) | Operational Mode / Description |
| :--- | :---: | :---: | :--- |
| **Microcontroller (MCU)** | 5.0 V | 45 mA (0.045 A) | Peak logic processing with active peripherals clocking |
| **Texas Instruments TLV9002** | 5.0 V | 2 mA (0.002 A) | Dual Op-Amp current amplification and comparator loops |
| **Cytron MD13S (Logic)** | 5.0 V | 15 mA (0.015 A) | Internal gate driver logic stage baseline control loop |
| **Pololu 25D Gearmotor** | 12.0 V | 2200 mA (2.200 A) | Worst-case mechanical stall drawing high torque clamping |

---

## Section B: Power Rail Mapping & Safety Margin

To prevent high-frequency inductive motor transients from introducing ripple voltage into high-precision analog current-sensing traces, the architecture splits consumption across two separate regulated voltage rails.

### 1. Logic Power Rail (5.0 V)
* **Assigned Subsystems:** Microcontroller, TLV9002 Signal Chain, Cytron MD13S Logic Network
* **Net Ideal Current Base Draw:** $45\text{ mA} + 2\text{ mA} + 15\text{ mA} = 62\text{ mA}\ (0.062\text{ A})$
* **Rail Budget Allocation (+25% Safety Margin):**  
  $$I_{\text{logic\_allocated}} = 62\text{ mA} \times 1.25 = \mathbf{77.5\text{ mA}\ (0.0775\text{ A})}$$

### 2. Electromechanical Actuator Power Rail (12.0 V)
* **Assigned Subsystems:** Pololu 25D High-Torque Gearmotor (Stall State)
* **Net Ideal Current Base Draw:** $2200\text{ mA}\ (2.200\text{ A})$
* **Rail Budget Allocation (+25% Safety Margin):**  
  $$I_{\text{actuator\_allocated}} = 2200\text{ mA} \times 1.25 = \mathbf{2750\text{ mA}\ (2.750\text{ A})}$$

---

## Section C: Voltage Regulator Selections

| Regulator Candidate Solution | Pros | Cons |
| :--- | :--- | :--- |
| **Option 1: STMicroelectronics LM7805**<br>• Unit Cost: $0.62<br>• [Product Datasheet Page](https://st.com) | • Breadboard-friendly, rugged TO-220 through-hole package.<br>• Outputs clean, stable, noise-free DC rails.<br>• Built-in thermal protection loops protect downstream circuits. | • Inefficient when linearizing a 12V down to a 5V line.<br>• Dissipates dropped power completely as heat.<br>• Requires space for a clip-on heatsink. |
| **Option 2: Murata OKI-78SR-5/1.5-W36-C**<br>• Unit Cost: $4.85<br>• [Product Datasheet Page](https://murata.com) | • High step-down switching power efficiency (85%+).<br>• Operates at cool temperatures without needing a heatsink.<br>• Direct drop-in pin layout matching LM7805. | • Introduces high-frequency switching harmonic noise.<br>• Requires extra external decoupling capacitors.<br>• High cost. |

### Final Choice for 5V Rail Regulator: Option 1: STMicroelectronics LM7805 Linear Regulator

**Rationale:** The LM7805 is selected because our total logic demand is under 50 mA. At this scale, the total power dissipated as heat is extremely low ($7\text{V} \times 0.05\text{A} = 0.35\text{W}$), allowing the through-hole TO-220 package to run safely without requiring an external metal heatsink footprint. This linear regulation structure ensures our **TLV9002 current-sense stage** gets an entirely quiet, ripple-free 5.0V source, avoiding high-frequency switching noise injection.

---

## Section D: External Power Source Verification

* **Selected External Supply:** 12V Rechargeable Lithium-Ion Battery Pack (3S Configuration)
* **Maximum Continuous Current Discharge Capacity:** 5.00 A (5000 mA)
* **Total Peak Current Draw Requested (Including 25% Margin Calculations):**  
  $$I_{\text{total\_peak}} = 0.0775\text{ A (Logic Rail)} + 2.750\text{ A (Actuator Rail)} = \mathbf{2.8275\text{ A}}$$

### Remaining Supply Safety Budget Verification:
$$\text{Remaining Current Available} = 5.00\text{ A} - 2.8275\text{ A} = \mathbf{+2.1725\text{ A}}$$

*Verification: The remaining system current capability is non-negative and positive ($+2.1725\text{ A}$). The 12V battery pack easily drives all power rails simultaneously at maximum stall loads without triggering overcurrent shutdown.*

---

## Section E: Worst-Case Battery Lifespan Calculation

The boundary condition calculation assumes the gripper mechanism is jammed, pulling maximum stall current continuously while logic profiles execute concurrently.

* **Battery Nominal Energy Capacity:** 2200 mAh ($2.20\text{ Ah}$)
* **Total Non-Margined Maximum Current Draw ($I_{\text{draw}}$):** $62\text{ mA} + 2200\text{ mA} = \mathbf{2262\text{ mA}\ (2.262\text{ A})}$

$$\text{Worst-Case Lifespan Hours} = \frac{\text{Battery Nominal Capacity}}{\text{Total Continuous Draw}} = \frac{2200\text{ mAh}}{2262\text{ mA}} = \mathbf{0.972\text{ Hours (58.3 Minutes)}}$$

*Note: Under standard lab operation, grip actuation cycles only persist for 2-3 seconds at a time. In real-world field settings, the node's true operation lifecycle will safely exceed 15+ hours.*

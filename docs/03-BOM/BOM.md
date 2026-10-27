# Estimated Component Budget

| Component | Estimated Cost |
|---|---:|
| [Cytron MD13S](https://cytron.io) | $15.30 |
| [TLV9002 Op-Amp](https://ti.com) | $0.83 |
| [Pololu 25D gearmotor](https://pololu.com) | $19.95 |
| [L7805 / LM7805](https://digikey.com) | $0.62 |
| **Estimated subtotal** | **$36.70** |

The subtotal excludes the lead screw, shaft coupler, mounting hardware, limit switches, shunt resistor, passive components, wiring, and power supply. Prices must be confirmed before purchasing.

---

# Conclusion

Board 2 controls the gripper actuator, monitors motor load, and communicates operating status to the bus master. The proposed components prioritize cost, integration, and flexibility during prototyping.

Before final implementation, the team must verify motor current requirements, driver compatibility, overload response, regulator thermal performance, and microcontroller pin assignments. The assembled lead-screw mechanism must also be tested to ensure that it produces the required gripping force and travel safely.

The final design should include a reliable method for stopping the motor during an overload, detecting excessive travel, and entering a safe state during an emergency stop.



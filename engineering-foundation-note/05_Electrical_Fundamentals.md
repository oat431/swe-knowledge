---
title: "Electrical Fundamentals"
tags:
  - electrical
  - circuits
  - engineering
source: "Engineering Fundamentals Ch 12"
---

# Electrical Fundamentals

## Electric Current as a Fundamental Dimension

Electric current is one of seven fundamental dimensions in engineering. The **ampere (A)** was approved as a base unit by the General Conference on Weights and Measures (CGPM) in 1946/1954.

> **Definition:** The ampere is that constant current which, if maintained in two straight parallel conductors of infinite length, of negligible circular cross section, placed 1 meter apart in a vacuum, would produce between these conductors a force equal to 2 × 10⁻⁷ newton per meter of length.

### Subatomic Basis

- Atoms consist of **electrons** (negative charge), **protons** (positive charge), and **neutrons** (no charge)
- Neutrons and protons form the nucleus
- How a material conducts electricity depends on electron number and arrangement
- **Basic law:** Unlike charges attract; like charges repel

### Electric Charge

- Unit: **coulomb (C)**
- 1 coulomb = amount of charge passing a point in 1 second when 1 ampere flows
- Electric charge is **conserved** — it cannot be created or destroyed, only transferred

### Coulomb's Law

The electric force between two point charges:

$$F = \frac{k q_1 q_2}{r^2}$$

Where:
- k = 8.99 × 10⁹ N·m²/C²
- q₁, q₂ = point charges (C)
- r = distance between charges (m)

---

## Voltage

**Voltage** represents the work required to move charge between two points. **Electromotive force (emf)** is the electric potential difference between an area with excess free electrons (negative) and an area with electron deficit (positive).

- Voltage induces current flow in a circuit
- Common sources: **chemical reaction**, **light**, **magnetism**

### Batteries

- Electricity produced by chemical reaction within the battery
- **Primary cell:** Cannot be recharged (e.g., alkaline batteries)
- **Secondary cell:** Can be recharged (e.g., lead-acid, NiCd, NiMH)
  - NiCd: cordless phones, toys, some cell phones
  - NiMH: smaller cell phones, higher capacity

### Series vs. Parallel Connections

- **Series:** Voltages add up (four 1.5V batteries = 6V)
- **Parallel:** Same voltage, more current capacity

### Photoemission

When light strikes certain surfaces, electrons can be freed, generating electricity.

- **Photothermal:** Solar radiation heats water → steam → turbine → generator
- **Photovoltaic:** Light converts directly to electricity (gallium arsenide cells, ~20% efficiency)
- Solar farms: vast arrays of solar panels for power generation

### Power Plants

Electricity is generated in power plants via:
1. Fuel burned in boiler → heat
2. Heat converts water to steam
3. Steam drives turbine blades
4. Turbine runs generator (coil of wire in magnetic field)
5. Steam condenses and recirculates

> A conductor in a changing magnetic field has current induced in it — **magnetism** is the most common method for generating electricity.

---

## Direct Current and Alternating Current

### Direct Current (DC)

- Flow of electric charge in **one direction**
- Produced by batteries and DC generators
- Historically limited for long-distance transmission (high voltage transformation difficulties)
- Modern developments (1960s+) enable long-distance DC transmission

### Alternating Current (AC)

- Flow of electric charge **periodically reverses**
- Current starts at zero → increases to max → decreases to zero → reverses → repeats
- Key parameters:
  - **Period:** Time interval between peak values of successive cycles
  - **Frequency:** Number of cycles per second (Hz)
  - **Amplitude:** Peak (maximum) value of current in either direction
- U.S. domestic/commercial standard: **60 Hz**

### Kirchhoff's Current Law

> At any node, the sum of currents entering equals the sum of currents leaving.

$$\sum I_{in} = \sum I_{out}$$

This represents the physical fact that charge is conserved — it cannot accumulate or deplete at a node.

---

## Electrical Circuits and Components

An **electrical circuit** is a combination of connected electrical components: wires (conductors), switches, outlets, resistors, and capacitors.

### Wire Resistance

Resistance depends on:
- **Material** (resistivity)
- **Length**
- **Diameter**
- **Temperature**

**Resistivity** is a measure of a material's resistance to electric current.

### Residential Power Distribution

Typical residential systems include:
- **Service cable** and **meter** from utility
- **200-amp main circuit breaker panel**
- Dedicated circuits for high-draw appliances:
  - Range/oven: 50 A
  - Central air conditioner: 50 A
  - Dryer: 30 A
  - Dishwasher: 20 A
  - Kitchen outlets: 20 A (GFCI)
  - General outlets: 15–20 A
  - Lighting: 15 A

---

## Summary

- Electric current is a fundamental dimension measured in **amperes**
- **Voltage** drives current through a circuit; sources include batteries, photoemission, and power plants
- **DC** flows in one direction; **AC** periodically reverses (60 Hz in the U.S.)
- **Kirchhoff's current law:** Current in = Current out at any node
- Electrical circuits combine conductors, switches, resistors, and other components
- Residential wiring requires careful planning of circuits, amperage, and safety devices (GFCI)


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[06_Energy_and_Power]] — Electrical energy and power
- [[01_Dimensions_and_Measurement]] — Fundamental dimensions (current)

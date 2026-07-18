---
title: "Energy and Power"
source: "Engineering Fundamentals Ch 13"
tags:
  - energy
  - power
  - thermodynamics
  - engineering
---

# Energy and Power

## 13.1 Work, Mechanical Energy, Thermal Energy

**Energy** is the capacity to do work. When work is done on or against an object, it changes the object's energy.

### Kinetic Energy

Energy of a moving object:

$$KE = \frac{1}{2}mV^2$$

- **m** = mass (kg or slug)
- **V** = speed (m/s or ft/s)
- Units: **joule (J)** = kg·m²/s² = N·m (SI), **ft·lb_f** (US Customary)

**Change in kinetic energy** (used in engineering analysis):

$$\Delta KE = \frac{1}{2}mV_2^2 - \frac{1}{2}mV_1^2$$

### Potential Energy

Gravitational potential energy — work required to lift an object:

$$\Delta PE = mg\Delta h$$

- **g** = 9.81 m/s² (32.2 ft/s²)
- **Δh** = change in elevation (m or ft)
- Units: **joule (J)** or **ft·lb_f**

> Energy required to lift between any two adjacent floors is the same (same Δh).

### Elastic Energy

Energy stored in a spring when stretched or compressed:

$$EE = \frac{1}{2}kx^2$$

- **k** = spring constant (N/m)
- **x** = deflection from unstretched position (m)

**Change in elastic energy:**

$$\Delta EE = \frac{1}{2}kx_2^2 - \frac{1}{2}kx_1^2$$

### Conservation of Mechanical Energy

In the absence of heat transfer and losses, and with no external work:

$$\Delta KE + \Delta PE + \Delta EE = 0$$

> Energy content of a system can change form, but total energy is constant.

### Thermal Energy Units

| Unit | Definition |
|------|-----------|
| **Btu** | Heat to raise 1 lb of water by 1°F |
| **Calorie** | Heat to raise 1 g of water by 1°C |
| **Joule** | 1 N·m = 1 kg·m²/s² |

Conversions:
- 1 Btu = 778 ft·lb_f
- 1 Btu = 1055 J

**Internal energy** — molecular activity related to temperature; higher temperature → higher internal energy.

---

## 13.2 Conservation of Energy — First Law of Thermodynamics

> Energy is conserved. It cannot be created or destroyed; it can only change forms.

$$Q - W = \Delta E$$

- **Q** = net heat transfer *into* the system (J)
- **W** = net work done *by* the system (J)
- **ΔE** = net change in total energy (J) — includes internal, kinetic, potential, elastic, and other forms

**Sign convention:**
| Direction | Sign |
|-----------|------|
| Heat into system | + |
| Heat out of system | − |
| Work done by system | + |
| Work done on system | − |

> "When it comes to energy, the best you can do is break even." (And the second law says you can't even do that — there are always losses.)

---

## 13.3 Power

**Power** = time rate of doing work, or energy divided by time.

$$\text{Power} = \frac{\text{work}}{\text{time}} = \frac{\text{energy}}{\text{time}}$$

> If you want a task done in less time, more power is required. Same work, shorter time = more power.

| Concept | Meaning |
|---------|---------|
| Walk up stairs | Lower power (same work, more time) |
| Run up stairs | Higher power (same work, less time) |

---

## 13.4 Watts and Horsepower

### SI Units

$$\text{Power} = \frac{\text{N·m}}{\text{s}} = \frac{\text{J}}{\text{s}} = \text{watt (W)}$$

### US Customary Units

$$\text{Power} = \frac{\text{ft·lb}_f}{\text{s}}$$

$$1 \text{ hp} = 550 \text{ ft·lb}_f/\text{s}$$

### Unit Conversions

| Conversion | Value |
|-----------|-------|
| 1 ft·lb_f/s | 1.3558 W ≈ 1.36 W |
| 1 hp | 745.7 W ≈ 746 W |
| 1 hp | slightly less than 1 kW |
| 1 kW | 1000 W = 1000 J/s |
| 1 kWh | 3.6 MJ (energy, not power!) |

> **kWh is a unit of energy**, not power. 1 kWh = energy consumed in 1 hour at 1 kW.

### HVAC Units

| Unit | Definition |
|------|-----------|
| **Btu/h** | 1 Btu/h ≈ 1.055 kW |
| **Ton of refrigeration** | 12,000 Btu/h = capacity to freeze 1 ton of water at 32°F in 24 hours |
| Residential AC | typically 1–5 tons |
| Typical gas furnace | ~60,000 Btu/h |

---

## 13.5 Efficiency

$$\text{Efficiency} = \frac{\text{actual output}}{\text{required input}}$$

> All machines require more input than they produce as output.

### Power Plant Efficiency

$$\text{Power plant efficiency} = \frac{\text{energy generated}}{\text{energy input from fuel}}$$

| Type | Efficiency |
|------|-----------|
| Fossil fuel (coal, gas, oil) | ~40% |
| Nuclear | ~34% |

### Internal Combustion Engine Efficiency

$$\text{Thermal efficiency} = \frac{\text{power output}}{\text{heat power input (fuel burned)}}$$

| Engine Type | Efficiency |
|------------|-----------|
| Gasoline | 25–30% |
| Diesel | 35–40% |

### Motor and Pump Efficiency

**Motor:**
$$\text{Efficiency} = \frac{\text{power input to driven device}}{\text{electric power input to motor}}$$

**Pump:**
$$\text{Efficiency} = \frac{\text{power input to fluid by pump}}{\text{power input to pump by motor}}$$

### Heating, Cooling, and Refrigeration

**Vapor-compression cycle** components:
- **Evaporator** — absorbs heat (cold side)
- **Compressor** — raises refrigerant temperature/pressure
- **Condenser** — ejects heat (hot side)
- **Expansion valve** — drops temperature/pressure

**Coefficient of Performance (COP):**
$$COP = \frac{\text{heat removal from evaporator}}{\text{energy input to compressor}}$$

Typical COP: **2.9 to 4.9**

**Energy Efficiency Ratio (EER / SEER):**
$$EER = \frac{\text{heat removal (Btu)}}{\text{energy input (Wh)}}$$

$$COP = \frac{EER}{3.412}$$

- SEER range: 10–17 (minimum standard in US)
- Gas furnace AFUE: 80–96% (minimum 78% for new installations)

---

## 13.6 Energy Sources, Generation, Consumption

### U.S. Electricity Generation (~4,157 billion kWh in 2007)

| Source | Share |
|--------|-------|
| Coal | 48.5% |
| Natural gas | 21.6% |
| Nuclear | 19.4% |
| Hydroelectric | 5.8% |
| Petroleum | 1.6% |
| Other renewables | 2.5% |

### Key Energy Facts

- ~93% of coal mined in the U.S. is used for electricity generation
- U.S. natural gas network: 1.5 million miles of pipeline
- Nuclear fission used in power plants (fusion is still research-stage)
- Heating oil refined from crude oil, dyed red (tax-exempt, not for highway use)

### The Energy Challenge

- Global energy use per capita is increasing steadily
- World population expected to reach ~9 billion by mid-century
- Engineers face dual challenges: energy supply and emissions reduction

---

## Key Equations Summary

| Equation | Description |
|----------|-------------|
| $KE = \frac{1}{2}mV^2$ | Kinetic energy |
| $\Delta PE = mg\Delta h$ | Potential energy change |
| $EE = \frac{1}{2}kx^2$ | Elastic energy |
| $\Delta KE + \Delta PE + \Delta EE = 0$ | Conservation of mechanical energy |
| $Q - W = \Delta E$ | First law of thermodynamics |
| $P = \frac{W}{t}$ | Power |
| $\eta = \frac{\text{output}}{\text{input}}$ | Efficiency |
| $COP = \frac{Q_{evap}}{W_{comp}}$ | Refrigeration COP |
| 1 hp = 746 W | Unit conversion |
| 1 kWh = 3.6 MJ | Unit conversion |


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[03_Force_and_Mechanics]] — Work and force relationships
- [[04_Temperature_and_Thermal]] — Thermal energy and heat transfer
- [[05_Electrical_Fundamentals]] — Electrical energy
- [[01_Dimensions_and_Measurement]] — Fundamental dimensions

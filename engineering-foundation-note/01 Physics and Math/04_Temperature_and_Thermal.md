---
title: "Temperature and Thermal Parameters"
tags:
  - temperature
  - heat-transfer
  - thermal
  - engineering
source: "Engineering Fundamentals Ch 11"
---

# Temperature and Thermal Parameters

## Temperature as a Fundamental Dimension

Temperature is one of seven fundamental (base) dimensions in engineering — along with length, mass, time, electric current, amount of substance, and luminous intensity. It quantifies the level of molecular activity and internal energy of an object.

> **Key Insight:** Temperature represents, by a single macroscopic number, the level of microscopic molecular movement in a region. Molecules at higher temperature move, rotate, and bounce faster than those at lower temperature.

### Why Temperature Matters in Engineering

| Discipline | Application |
|---|---|
| **Electrical/Computer** | Cooling chips with heat sinks and fans to maintain operating temperature |
| **Civil** | Designing pavements, bridges for thermal expansion/contraction of concrete and steel |
| **Mechanical** | HVAC design — heat transfer, air temperature, moisture content |
| **Automotive** | Engine cooling system design |
| **Food Processing** | Monitoring drying and cooling process temperatures |
| **Materials** | Creating materials with desirable thermal properties |

### Material Properties Depend on Temperature

- **Air density** decreases as temperature rises (cold air is denser than warm air)
- **Oil viscosity** increases as temperature drops (harder to start a car in winter)
- Physical and thermal properties of solids, liquids, and gases all vary with temperature

---

## Temperature Scales

### Celsius (°C)

- Under standard atmospheric conditions:
  - **0°C** = freezing point of water
  - **100°C** = boiling point of water
- Calibration points assigned arbitrarily (could have been any numbers)

### Fahrenheit (°F)

- Under standard atmospheric conditions:
  - **32°F** = freezing point of water
  - **212°F** = boiling point of water

### Conversion Between Celsius and Fahrenheit

$$T_{°C} = \frac{5}{9}(T_{°F} - 32)$$

$$T_{°F} = \frac{9}{5} \cdot T_{°C} + 32$$

### Absolute Temperature Scales

Based on the behavior of an ideal gas. The **ideal gas law**:

$$PV = mRT$$

Where:
- P = absolute pressure (Pa or lb/ft²)
- V = volume (m³ or ft³)
- m = mass (kg or lbm)
- R = gas constant (J/kg·K or ft·lb/lbm·°R)
- T = absolute temperature (K or °R)

### Kelvin (K) — SI Absolute Scale

$$T_K = T_{°C} + 273.15$$

- Absolute zero = 0 K (theoretical lower limit of temperature)
- No theoretical upper limit to temperature

### Rankine (°R) — US Customary Absolute Scale

$$T_{°R} = T_{°F} + 459.67$$

### Kelvin ↔ Rankine

$$T_K = \frac{5}{9} \cdot T_{°R}$$

### Temperature Difference Equivalence

- A **1°C difference = 1 K difference** (size of degree is identical)
- A **1°F difference = 1°R difference** (size of degree is identical)
- This holds because the same base offset is added to both temperatures when converting, so it cancels in the difference

### Color-Temperature Relationship for Iron

| Color | Temperature (°F) |
|---|---|
| Dark blood red, black red | 990 |
| Dark cherry red | 1175 |
| Cherry, full red | 1375 |
| Orange | 1650 |
| Yellow | 1825 |
| White | 2200 |

---

## Temperature Measurement

### Mercury Thermometer

- Works on the principle of **thermal expansion/contraction** of mercury
- Graduated glass rod filled with mercury
- Modern thermometers can measure to 1/100°C increments

### Thermocouple

- Consists of **two dissimilar metals**
- A small voltage output is created when a temperature difference exists between the two junctions
- Voltage output is **proportional** to the temperature difference
- Common types:
  - **J-type:** Iron / Constantan
  - **T-type:** Copper / Constantan

### Thermistor

- Semiconductor device whose **electrical resistance changes significantly** with small temperature changes
- Resistance is correlated to a temperature value

### RTD (Resistance Temperature Detector)

- Uses the change in electrical resistance of a material to measure temperature

---

## Heat Transfer

Thermal energy transfer occurs whenever there exists a **temperature difference** within an object, between two bodies, or between a body and its surroundings.

> **Heat** is energy transferred between regions due to a temperature difference. **Temperature** represents the level of molecular movement. They are distinct concepts.

Heat **always flows** from a high-temperature region to a low-temperature region.

### Thermal Energy Units

| Unit | Definition |
|---|---|
| **Btu** | Heat to raise 1 lbm of water by 1°F |
| **Calorie (cal)** | Heat to raise 1 g of water by 1°C |
| **Food Calorie** | = 1000 calories (kilocalorie) |
| **Joule (J)** | 1 N·m = 1 kg·m²/s² (SI unit of energy) |

### Key Conversions

| Energy | Power |
|---|---|
| 1 Btu = 1055 J | 1 W = 1 J/s |
| 1 Btu = 252 cal | 1 W = 3.4123 Btu/h |
| 1 cal = 4.186 J | 1 cal/s = 4.186 W |

---

## Modes of Heat Transfer

### 1. Conduction

Heat transfer through a medium due to a temperature gradient — energy is transported from more-energetic molecules to less-energetic neighbors.

**Fourier's Law:**

$$q = kA \frac{T_1 - T_2}{L}$$

Where:
- q = heat transfer rate (W or Btu/h)
- k = thermal conductivity (W/m·K or Btu/h·ft·°F)
- A = cross-sectional area normal to heat flow (m² or ft²)
- T₁ − T₂ = temperature difference across material (°C or °F)
- L = material thickness (m or ft)

The **temperature gradient** (T₁ − T₂)/L must exist for conduction to occur.

### Thermal Conductivity

A material property indicating how well it transfers thermal energy. Higher k = better conductor.

**Typical Values at 300 K:**

| Material | k (W/m·K) |
|---|---|
| Silver | 429 |
| Copper (pure) | 401 |
| Gold | 317 |
| Aluminum (pure) | 237 |
| Silicon | 148 |
| Brass (70/30) | 110 |
| Iron (pure) | 80.2 |
| Stainless steel (302) | 15.1 |
| Concrete | 1.4 |
| Glass | 1.4 |
| Brick (fire clay) | 1.0 |
| Water (liquid) | 0.61 |
| Human muscle | 0.41 |
| Human skin | 0.37 |
| Sand | 0.27 |
| Human fat | 0.2 |
| Paper | 0.18 |
| Asphalt | 0.062 |
| Air (atmospheric) | 0.0263 |

**General rule:** Solids > Liquids > Gases (for thermal conductivity)

### 2. Convection

Heat transfer when a **fluid in motion** contacts a solid surface at a different temperature.

**Newton's Law of Cooling:**

$$q = hA(T_s - T_f)$$

Where:
- h = heat transfer coefficient (W/m²·K or Btu/h·ft²·°R)
- A = exposed surface area (m² or ft²)
- Tₛ = surface temperature
- T_f = fluid temperature

**Types of Convection:**

| Type | Description | Example |
|---|---|---|
| **Forced convection** | Flow caused by fan or pump | Fan cooling a CPU, blowing on hot food |
| **Free (natural) convection** | Flow caused by density variation from temperature distribution | Hot pie cooling on counter, oven exterior heat loss |

**Heat Transfer Coefficient Ranges:**

| Convection Type | h (W/m²·K) | h (Btu/h·ft²·°R) |
|---|---|---|
| Free — Gases | 2 to 25 | 0.35 to 4.4 |
| Free — Liquids | 50 to 1,000 | 8.8 to 175 |
| Forced — Gases | 25 to 250 | 4.4 to 44 |
| Forced — Liquids | 100 to 20,000 | 17.6 to 3,500 |

Key relationships:
- h is **higher** for forced convection than free convection
- h is **higher** for liquids than gases
- This is why you feel colder in 70°F water than 70°F air

### 3. Radiation

(Referenced in the chapter as the third mode; heat transfer via electromagnetic waves — no medium required.)

---

## Thermal Resistance

### Conduction Thermal Resistance

Rearranging Fourier's law:

$$q = \frac{T_1 - T_2}{L/(kA)} = \frac{\text{temperature difference}}{\text{thermal resistance}}$$

**Thermal resistance:** R' = L/(kA) — units: °C/W or K/W

**Analogy to Ohm's Law:**

| Heat Flow | Electrical |
|---|---|
| Temperature difference (ΔT) | Voltage (V) |
| Heat flow rate (q) | Current (I) |
| Thermal resistance (R) | Electrical resistance (Rₑ) |

### R-Value (R-Factor)

The **R-value** is thermal resistance expressed per unit area:

$$R = \frac{L}{k}$$

Units: m²·°C/W (or ft²·°F·h/Btu)

- **Higher R-value = more resistance to heat flow = better insulation**
- Composite R-values: **add R-values of each layer** for total resistance
- Typical attic insulation target: R-40

### Convection Thermal Resistance

$$R' = \frac{1}{hA}$$

**Film resistance** (per unit area):

$$R = \frac{1}{h}$$

Higher R = more resistance to heat flow to/from surrounding fluid.

---

## Specific Heat

The amount of thermal energy required to raise the temperature of a unit mass of a substance by one degree.

$$Q = mc\Delta T$$

Where:
- Q = thermal energy (J or Btu)
- m = mass (kg or lbm)
- c = specific heat (J/kg·K or Btu/lbm·°F)
- ΔT = temperature change (K or °F)

---

## Thermal Expansion

Materials expand when heated and contract when cooled.

### Linear Thermal Expansion

$$\Delta L = \alpha L \Delta T$$

Where:
- ΔL = change in length
- α = coefficient of linear expansion (1/°C or 1/°F)
- L = original length
- ΔT = temperature change

### Volume Thermal Expansion

$$\Delta V = \beta V \Delta T$$

Where β = coefficient of volume expansion ≈ 3α (for solids)

---

## Metabolic Rate

The rate at which the human body generates heat. Important in HVAC design for sizing heating/cooling systems for occupied spaces.

---

## Key Formulas Summary

| Formula | Equation | Description |
|---|---|---|
| Celsius ↔ Fahrenheit | T°C = (5/9)(T°F − 32) | Temperature scale conversion |
| Kelvin | T_K = T°C + 273.15 | Absolute temperature (SI) |
| Rankine | T°R = T°F + 459.67 | Absolute temperature (US) |
| Ideal Gas Law | PV = mRT | Gas behavior |
| Fourier's Law | q = kA(T₁−T₂)/L | Conduction heat transfer |
| Newton's Cooling | q = hA(Tₛ−T_f) | Convection heat transfer |
| R-value | R = L/k | Thermal resistance per unit area |
| Film Resistance | R = 1/h | Convection resistance per unit area |
| Specific Heat | Q = mcΔT | Energy for temperature change |
| Linear Expansion | ΔL = αLΔT | Length change with temperature |

---

## Quick Reference Conversions

| From → To | Formula |
|---|---|
| °C → °F | T°F = (9/5)·T°C + 32 |
| °F → °C | T°C = (5/9)·(T°F − 32) |
| °C → K | T_K = T°C + 273.15 |
| °F → °R | T°R = T°F + 459.67 |
| °R → K | T_K = (5/9)·T°R |
| Btu → J | 1 Btu = 1055 J |
| cal → J | 1 cal = 4.186 J |
| Btu/h → W | 1 W = 3.4123 Btu/h |


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[01 Physics and Math/06_Energy_and_Power|06_Energy_and_Power]] — Energy and thermodynamics
- [[01 Physics and Math/08_Engineering_Materials|08_Engineering_Materials]] — Thermal properties of materials
- [[01 Physics and Math/01_Dimensions_and_Measurement|01_Dimensions_and_Measurement]] — Fundamental dimensions

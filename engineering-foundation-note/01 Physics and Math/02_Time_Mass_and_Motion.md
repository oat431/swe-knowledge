---
tags: [time, mass, motion, engineering, fundamentals, physics]
---

# 02 — Time, Mass & Motion

> **Source:** *Engineering Fundamentals: An Introduction to Engineering* by Saeed Moaveni — Chapters 8–9

## Overview

Time and mass are two fundamental dimensions that, combined with length, describe most of the physical world engineers deal with. Time lets us describe **when** and **how fast** things happen; mass lets us describe **how much** matter is involved and **how hard** it is to change motion. Together they yield velocity, acceleration, momentum, flow rates, and rotational dynamics.

---

## 1. Time as a Fundamental Dimension (Ch 8)

### 1.1 Why Time Matters

Everything in the universe is in motion. Time is the parameter that lets us describe the rate of that motion and the duration of events. Engineering problems divide into two broad categories:

| Category | Description | Example |
|---|---|---|
| **Steady** | Physical quantity does not change with time | Dimensions of a credit card at constant temperature |
| **Unsteady (transient)** | Physical quantity changes with time | Cooling cookies, car suspension hitting a pothole |

### 1.2 Measurement of Time

| Era | Technology |
|---|---|
| Ancient | Sundials, water clocks, sand glasses (hourglasses) |
| 14th century | Weight-driven mechanical clocks |
| 16th century | Spring-loaded clocks → watches |
| ~17th century | Pendulum clocks |
| Mid-20th century | Quartz crystal clocks (piezoelectric vibration) |
| Modern standard | Cesium-133 atomic clock |

> **SI definition of a second:** The duration of 9,192,631,770 periods of radiation corresponding to the transition between two hyperfine levels of the ground state of cesium-133.

### 1.3 Time Zones & Daylight Saving

- Earth rotates 360° in 24 h → **15° of longitude = 1 hour**
- Zero longitude passes through Greenwich, England
- Standardized time zones were driven by **railroad scheduling** in the late 19th century
- **Daylight saving time** was introduced during WWI/WWII to save energy; the U.S. Uniform Time Act (1966) standardized it

---

## 2. Period and Frequency

| Quantity | Symbol | Definition | Unit |
|---|---|---|---|
| **Period** | T | Time for one complete cycle | seconds (s) |
| **Frequency** | f | Number of cycles per unit time | Hertz (Hz = 1/s) |

$$f = \frac{1}{T}$$

### Key Formulas

**Natural frequency of a spring–mass system:**

$$f_n = \frac{1}{2\pi}\sqrt{\frac{k}{m}}$$

where k = spring stiffness (N/m), m = mass (kg).

**Period of a pendulum:**

$$T = 2\pi\sqrt{\frac{L}{g}}$$

where L = pendulum length (m), g = 9.81 m/s². Note: the period is **independent of mass**.

### Frequency Examples

| System | Frequency |
|---|---|
| Alternating current (USA) | 60 Hz |
| AM radio | 540 kHz – 1.6 MHz |
| FM radio | 88 – 108 MHz |
| PC clock (~2010) | up to 3 GHz |
| Wireless router | 5 GHz |

---

## 3. Traffic Flow (Time-Based Engineering)

Traffic engineering uses time-related parameters to design highways and control devices.

| Parameter | Symbol | Formula | Units |
|---|---|---|---|
| **Traffic flow** | q | q = 3600n / T | vehicles/hour |
| **Traffic density** | k | k = 1000n / d | vehicles/km |
| **Average speed** | u | u = (1/n) Σ uᵢ | km/h |
| **Relationship** | — | q = k · u | — |

- **Congested flow:** high density → low speed → eventually zero flow (bumper-to-bumper)
- **Uncongested flow:** low density → high speed → flow decreases as speed continues to rise

---

## 4. Linear Velocity and Acceleration

### 4.1 Average Speed

$$\text{average speed} = \frac{\text{distance traveled}}{\text{time}}$$

**Scalar** — has magnitude only. Common units: m/s, km/h, ft/s, mph.

### 4.2 Instantaneous Velocity

**Velocity** = speed + **direction** → a **vector** quantity. The instantaneous velocity gives the speed and direction at any specific instant.

| Object | Speed |
|---|---|
| Person walking | ~1.3 m/s (2.9 mph) |
| Fastest 100 m runner | 10.2 m/s (22.8 mph) |
| Pro tennis serve | 58 m/s (130 mph) |
| Boeing 777 cruise | 248 m/s (555 mph) |
| Space shuttle orbital | 7,741 m/s (17,316 mph) |

### 4.3 Acceleration

$$\text{average acceleration} = \frac{\text{change in velocity}}{\text{time}}$$

- Unit: m/s² or ft/s²
- **Zero acceleration** = constant velocity
- A car turning at constant speed still has acceleration (centripetal, due to changing direction)

### 4.4 Acceleration Due to Gravity

Newton's law of gravitation:

$$F = \frac{G m_1 m_2}{r^2}, \quad G = 6.7 \times 10^{-11} \text{ m}^3/\text{kg·s}^2$$

At Earth's surface: **g = 9.81 m/s² = 32.2 ft/s²**

**Falling object (neglecting air resistance):**

| Time (s) | Speed (m/s) | Distance fallen (m) |
|---|---|---|
| 0 | 0 | 0 |
| 1 | 9.81 | 4.90 |
| 2 | 19.62 | 19.62 |
| 3 | 29.43 | 44.14 |
| 5 | 49.05 | 122.62 |

---

## 5. Volume Flow Rate

$$\text{volume flow rate} = \frac{\text{volume}}{\text{time}}$$

Units: m³/s, L/s, ft³/s (cfs), gal/min (gpm), gal/day (gpd)

**Forflow in pipes:**

$$Q = \bar{V} \times A$$

where Q = volume flow rate, V̄ = average velocity, A = cross-sectional area.

> **Key insight:** Reducing pipe diameter by factor of 2 increases fluid speed by factor of 4 (continuity).

---

## 6. Angular (Rotational) Motion (Ch 8)

### 6.1 Angular Speed

$$\omega = \frac{\Delta\theta}{\Delta t}$$

- Units: rad/s or **rpm** (revolutions per minute)
- Conversion: 1600 rpm = 1600 × (2π/60) ≈ 167.5 rad/s

**Common rotational speeds:**
- Dentist's drill: 400,000 rpm
- Hard drive: 7,200 rpm
- Earth: 1 rev / 24 h = 15°/h

### 6.2 Linear–Angular Velocity Relationship

$$V = r\omega$$

where r = radius, ω = angular speed, V = linear (translational) speed.

### 6.3 Angular Acceleration

$$\alpha = \frac{\text{change in angular speed}}{\text{time}}$$

Units: rad/s²

---

## 7. Mass as a Fundamental Dimension (Ch 9)

### 7.1 What Is Mass?

- **Mass** = quantitative measure of the amount of matter (atoms/molecules) in an object
- Mass is a **scalar** — has magnitude only
- Mass provides a **measure of resistance to motion** (inertia)
- The distribution of mass about a rotation axis determines **rotational resistance**

### 7.2 Mass Units

| System | Unit | Equivalences |
|---|---|---|
| SI | kilogram (kg) | — |
| British Gravitational (BG) | slug | 1 kg = 0.0685 slugs |
| U.S. Customary | lbm | 1 kg = 2.205 lbm, 1 slug = 32.2 lbm |

Mass is measured **indirectly** via weight (force due to gravity) using spring scales or force transducers.

> **Weight ≠ Mass.** Weight is the gravitational force on a mass: W = mg. Mass is intrinsic; weight depends on location.

---

## 8. Density, Specific Volume, Specific Gravity

### 8.1 Density

$$\rho = \frac{\text{mass}}{\text{volume}}$$

- SI: kg/m³ | BG: slugs/ft³ | USCS: lbm/ft³
- Density changes with temperature and pressure

### 8.2 Specific Volume

$$v = \frac{\text{volume}}{\text{mass}} = \frac{1}{\rho}$$

- SI: m³/kg — commonly used in thermodynamics

### 8.3 Specific Gravity

$$SG = \frac{\rho_{\text{material}}}{\rho_{\text{water at 4°C}}}$$

- **Unitless** — ratio of two densities
- Same value regardless of unit system

### 8.4 Specific Weight

$$\gamma = \frac{\text{weight}}{\text{volume}} = \rho g$$

### 8.5 Material Densities (Table 9.1 excerpt)

| Material | Density (kg/m³) | Specific Gravity |
|---|---|---|
| Aluminum | 2,740 | 2.74 |
| Iron (cast) | 7,210 | 7.21 |
| Steel (stainless 304) | 7,860 | 7.86 |
| Glass (soda lime) | 2,470 | 2.47 |
| Water (4°C) | 1,000 | 1.00 |
| Gasoline | 720 | 0.72 |
| Mercury | 13,550 | 13.55 |
| Standard air | 1.225 | 0.0012 |
| Wood (oak) | 750 | 0.75 |
| Wood (pine) | 430 | 0.43 |

---

## 9. Mass Flow Rate

$$\dot{m} = \frac{\text{mass}}{\text{time}}$$

Units: kg/s, kg/min, slug/s, lbm/s

**Relationship to volume flow rate:**

$$\dot{m} = \rho \times Q$$

where ρ = density, Q = volume flow rate.

---

## 10. Mass Moment of Inertia

The mass moment of inertia measures **resistance to rotational acceleration** — analogous to mass for linear motion. The farther mass is from the axis, the harder it is to rotate.

**For a point mass:**

$$I_{z-z} = r^2 m$$

**For a continuous body:**

$$I_{z-z} = \int r^2 \, dm$$

### Standard Formulas

| Shape | Formula |
|---|---|
| **Disk** | I = ½ mR² |
| **Circular cylinder** | I = ½ mR² |
| **Sphere** | I = ⅖ mR² |
| **Thin rectangular plate** | I = 1/12 mW² |

---

## 11. Momentum

$$\vec{L} = m\vec{V}$$

- **Vector** — direction is same as velocity
- SI: kg·m/s | BG: slug·ft/s
- A small-mass object at high velocity (bullet) can have large momentum
- At rest → momentum is **zero**

### Relationship to Impulse

Linear impulse = Force × time = change in momentum. This is why air bags reduce injury: they increase contact time, reducing peak force.

---

## 12. Conservation of Mass

> Mass cannot be created or destroyed.

**For a control volume:**

$$(\text{rate in}) - (\text{rate out}) = \text{rate of accumulation (or depletion)}$$

This applies to:
- Fluids in pipes, tanks, pumps
- Chemical processes
- Environmental systems (river flow, water treatment)
- Even queuing systems (people in lines, data in networks)

### Kinetic Energy (Preview)

$$KE = \frac{1}{2} m V^2$$

Mass-dependent; detailed treatment in the Energy & Power chapter.

---

## Summary

| Concept | Key Idea |
|---|---|
| **Period & Frequency** | Reciprocal pairs: f = 1/T |
| **Linear speed/acceleration** | Rate of change of distance / velocity |
| **Volume flow rate** | Q = V̄ · A |
| **Angular motion** | ω = Δθ/Δt; V = rω |
| **Density** | Mass per unit volume |
| **Specific gravity** | Density relative to water (unitless) |
| **Mass flow rate** | ṁ = ρQ |
| **Mass moment of inertia** | Resistance to angular acceleration |
| **Momentum** | L = mV (vector) |
| **Conservation of mass** | Mass in − mass out = accumulation |

---

## See Also

- [[01 Physics and Math/01_Dimensions_and_Measurement|01_Dimensions_and_Measurement]] — Fundamental dimensions, unit systems
- [[01 Physics and Math/03_Force_and_Mechanics|03_Force_and_Mechanics]] — Force, weight, Newton's laws, work, pressure
- [[01 Physics and Math/06_Energy_and_Power|06_Energy_and_Power]] — Kinetic energy, work-energy theorem

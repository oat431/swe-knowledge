---
tags:
  - force
  - mechanics
  - stress
  - engineering
source: Engineering Fundamentals Ch 10
---

# Force and Mechanics

> **Source:** Engineering Fundamentals — Chapter 10: Force and Force-Related Parameters

Force is fundamental to engineering mechanics. This chapter covers force definition, Newton's laws, moment/torque, work, pressure/stress, elastic modulus, and their engineering applications. Newton's laws form the foundation of mechanics and the analysis/design of structures, airframes, car frames, medical implants, machine parts, and satellite orbits.

---

## 1. What We Mean by Force

**Force** is the simplest form of interaction between two objects — a **push** or a **pull**.

- Forces can result from **direct contact** (hand on lawn mower, bumper hitch on trailer)
- Forces can also act **at a distance** (gravitational, magnetic forces)
- All forces are defined by their **magnitude**, **direction**, and **point of application**

### Tendencies of a Force

An unbalanced force acting on an object will tend to:

| Tendency | Description |
|----------|-------------|
| **Translate** | Move the object in the direction of the force |
| **Rotate** | Turn the object about an axis |
| **Elongate** | Stretch the object |
| **Shorten** | Compress the object |
| **Bend** | Flex the object |
| **Twist** | Torsion on the object |

The amount of deformation depends on **support conditions**, **material properties**, and **geometric properties** (length, area, area moment of inertia).

### Units of Force

| System | Unit | Definition |
|--------|------|------------|
| **SI** | Newton (N) | 1 N = 1 kg · m/s² |
| **British Gravitational** | Pound-force (lbf) | 1 lbf = 1 slug · ft/s² |

**Conversion:** 1 lbf = 4.448 N

---

## 2. Spring Forces and Hooke's Law

**Hooke's Law** states that over the elastic range, the deformation of a spring is directly proportional to the applied force:

$$F = kx$$

Where:
- $F$ = applied force (N or lbf)
- $k$ = spring constant (N/mm, N/cm, or lbf/in)
- $x$ = deformation of the spring (mm, cm, or in)

The **elastic range** is the range over which, if the applied force is removed, the spring returns to its original shape with no permanent deformation. The spring constant depends on material type, shape, and winding, and can be determined experimentally.

---

## 3. Friction Forces

### Dry Friction

Dry friction exists because of surface irregularities between contacting surfaces. The **maximum static friction force** is:

$$F_{\max} = \mu N$$

Where:
- $N$ = normal force (perpendicular to contact surface)
- $\mu$ = coefficient of static friction

Once motion begins, friction drops to the **dynamic (kinetic) friction** level.

### Viscous Friction (Fluid Friction)

- Quantified by **viscosity** — a measure of how easily a fluid flows
- Higher viscosity → more resistance to flow (e.g., honey vs. water)
- Viscosity of **gases** increases with temperature
- Viscosity of **liquids** decreases with temperature
- Important for selecting lubricants and calculating pipeline energy requirements

---

## 4. Newton's Laws in Mechanics

**Mechanics** deals with the study of behavior of objects or structural members when subjected to forces. It tracks:
- Linear/angular displacements, velocity, acceleration
- Linear/angular deformations and stresses
- Geometric characteristics (length, area, moments of area)
- Material properties (density, modulus of elasticity, shear modulus)

### Newton's First Law (Law of Inertia)

> If an object is at rest with no unbalanced forces, it remains at rest. If moving at constant speed with no unbalanced forces, it continues at constant speed in the same direction.

### Newton's Second Law

> The net effect of unbalanced forces equals mass times acceleration:

$$\sum \vec{F} = m\vec{a}$$

Where:
- $\sum \vec{F}$ = sum of all forces (N)
- $m$ = mass of object (kg)
- $\vec{a}$ = resulting acceleration of the mass center (m/s²)

### Newton's Third Law (Action-Reaction)

> For every action there exists an equal and opposite reaction — same magnitude, same line of action, opposite direction.

### Newton's Law of Gravitation

Any two masses attract each other with:

$$F = \frac{G m_1 m_2}{r^2}$$

Where:
- $G$ = universal gravitational constant = $6.673 \times 10^{-11}$ m³/(kg·s²)
- $m_1, m_2$ = masses of the two particles (kg)
- $r$ = distance between centers (m)

### Weight

Weight is the gravitational force on an object's mass:

$$W = mg$$

Where $g \approx 9.8$ m/s² (Earth) or $32.2$ ft/s². The value of $g$ decreases with altitude:

$$g = \frac{G M_{\text{Earth}}}{R_{\text{Earth}}^2}$$

| Location | $g$ (m/s²) |
|----------|------------|
| Earth surface | 9.8 |
| Moon surface | 1.6 |
| Mars surface | 3.7 |
| Space shuttle (250 km) | 9.07 |
| Space shuttle (965 km) | 7.38 |

**Note:** Mass is constant everywhere; weight varies with local gravity.

---

## 5. Moment and Torque — Force Acting at a Distance

**Moment** (or **torque**) measures the tendency of a force to rotate an object about a point or axis.

$$M = d \times F$$

Where:
- $M$ = moment (N·m)
- $d$ = moment arm — perpendicular distance from line of action of force to the point of rotation
- $F$ = force (N)

**Convention:** Assign positive to clockwise (or counterclockwise) and negative to the opposite; the net sign reveals the overall rotation tendency.

### Couple

Two forces equal in magnitude, opposite in direction, but **not** sharing the same line of action constitute a **couple**. The moment of a couple equals either force times the perpendicular distance between them.

### Internal, External, and Reaction Forces

| Force Type | Description |
|------------|-------------|
| **External** | Applied loads on an object |
| **Internal** | Forces developed inside material to hold it together |
| **Reaction** | Forces at supports to maintain position |

### Support Conditions

| Support Type | Degrees of Freedom Restrained |
|-------------|-------------------------------|
| **Fixed** | No translation (2D), no rotation — reaction forces + reaction moment |
| **Pin** | No translation — two reaction forces, free to rotate |
| **Roller** | One reaction force perpendicular to surface |

---

## 6. Work — Force Acting Over a Distance

**Mechanical work** is the component of force that moves an object through a distance:

$$W = F \cos\theta \cdot d$$

Where:
- $W$ = work (J or N·m)
- $F$ = applied force (N)
- $\theta$ = angle between force and direction of motion
- $d$ = distance moved (m)

**Key points:**
- The normal component of force does **no** work (no displacement in that direction)
- Pushing against a stationary wall does **zero** mechanical work
- 1 joule (J) = 1 N·m

### Power

Power is the **rate** of doing work:

$$P = \frac{W}{t}$$

- Unit: watt (W) = 1 J/s
- Same work done in less time requires more power

---

## 7. Pressure and Stress — Force Acting Over an Area

### Pressure

**Pressure** measures the intensity of force acting over a contact area:

$$P = \frac{F}{A}$$

- **SI unit:** Pascal (Pa) = 1 N/m²
- **US Customary:** psi (lbf/in²), psf (lbf/ft²)

**Key insight:** The smaller the contact area, the larger the pressure from the same force.

### Pascal's Law

> For a fluid at rest, **pressure at a point is the same in all directions**.

### Pressure Variation with Depth

$$P = \rho g h$$

Where:
- $\rho$ = fluid density (kg/m³)
- $g$ = acceleration due to gravity (9.8 m/s²)
- $h$ = height of fluid column above the point (m)

### Atmospheric Pressure

| Unit | Value |
|------|-------|
| 1 atm | 101,325 Pa = 101.3 kPa |
| 1 atm | 14.69 psi |
| 1 atm | 760 mm Hg |
| 1 atm | 29.92 in Hg |

Atmospheric pressure decreases with altitude (e.g., ~31 kPa at top of Mount Everest, ~22.7 kPa at 11,000 m cruising altitude).

### Absolute vs. Gauge Pressure

$$P_{\text{absolute}} = P_{\text{gauge}} + P_{\text{atmospheric}}$$

- Negative gauge pressure indicates **vacuum**
- Tire pressure gauges read gauge pressure above local atmospheric

### Stress

**Stress** measures the intensity of **internal** forces acting over an area within a material:

- **Normal stress:** normal component of force / area (tensile or compressive)
- **Shear stress:** parallel component of force / area

Stress depends on geometric properties (cross-sectional area, second moment of area) and material properties.

### Buoyancy

The buoyancy force equals the weight of displaced fluid:

$$F_B = \rho V g$$

### Hydraulic Systems

In an enclosed incompressible fluid system, pressure is transmitted equally:

$$\frac{F_1}{A_1} = \frac{F_2}{A_2}$$

This allows **force multiplication** — a small force on a small piston can lift a large load on a large piston. Volume displaced is conserved:

$$A_1 L_1 = A_2 L_2$$

---

## 8. Modulus of Elasticity, Rigidity, and Bulk Modulus

### Tensile Testing

A test specimen is placed in a tensile testing machine. As load increases:
1. **Elastic region:** material deforms but returns to original shape if load removed
2. **Elastic point:** limit of elastic behavior
3. Beyond elastic point → permanent deformation

### Modulus of Elasticity (Young's Modulus)

The slope of the stress–strain diagram in the elastic region:

$$\sigma = E \varepsilon$$

Where:
- $\sigma$ = normal stress (N/m² or psi)
- $E$ = modulus of elasticity / Young's modulus (N/m² or psi)
- $\varepsilon$ = normal strain = $\Delta L / L$ (dimensionless)

**Key insight:** A higher modulus of elasticity means the material is **stiffer** — more force is required to stretch or compress it by the same amount.

| Material | Relative Stiffness |
|----------|-------------------|
| Rubber | Low $E$ — stretches easily |
| Aluminum | Moderate $E$ |
| Steel | High $E$ — very stiff |

### Hooke's Law for Solid Materials

The same principle as spring Hooke's law applies to solid bodies in terms of stress and strain ($\sigma = E\varepsilon$), valid within the elastic range.

### Modulus of Rigidity

Relates shear stress to shear strain for materials under torsional (twisting) loads.

### Bulk Modulus of Compressibility

Describes how a fluid's volume changes under applied pressure — relevant for hydraulic systems and fluid mechanics.

---

## Summary Table of Key Formulas

| Concept | Formula | Variables |
|---------|---------|-----------|
| Hooke's Law (spring) | $F = kx$ | $k$ = spring constant, $x$ = deformation |
| Static friction | $F_{\max} = \mu N$ | $\mu$ = friction coefficient, $N$ = normal force |
| Newton's 2nd Law | $\sum F = ma$ | $m$ = mass, $a$ = acceleration |
| Gravitational force | $F = Gm_1m_2/r^2$ | $G$ = gravitational constant |
| Weight | $W = mg$ | $g$ = local gravity |
| Moment/Torque | $M = dF$ | $d$ = moment arm |
| Mechanical work | $W = Fd\cos\theta$ | $\theta$ = angle between force and displacement |
| Power | $P = W/t$ | $t$ = time |
| Pressure | $P = F/A$ | $A$ = area |
| Fluid pressure | $P = \rho g h$ | $\rho$ = density, $h$ = depth |
| Absolute pressure | $P_{\text{abs}} = P_{\text{gauge}} + P_{\text{atm}}$ | — |
| Buoyancy | $F_B = \rho V g$ | $V$ = displaced volume |
| Hydraulic force | $F_2 = (A_2/A_1)F_1$ | — |
| Hooke's Law (solid) | $\sigma = E\varepsilon$ | $E$ = Young's modulus, $\varepsilon$ = strain |

---

## Key Takeaways

- **Force** is defined by magnitude, direction, and point of application; it can act by contact or at a distance
- **Newton's three laws** and the **law of gravitation** form the foundation of mechanics
- **Moment/torque** quantifies the rotational tendency of a force about a point
- **Work** is force × distance (in the direction of motion); **power** is work per unit time
- **Pressure** is force per unit area; it increases with fluid depth and is uniform at a point in a static fluid
- **Stress** is the intensity of internal forces within a material (normal stress, shear stress)
- **Young's modulus** ($E$) quantifies material stiffness — higher $E$ means stiffer material
- **Hydraulic systems** exploit pressure transmission in incompressible fluids for force multiplication
- Mass is constant; weight varies with local gravitational acceleration


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[01 Physics and Math/01_Dimensions_and_Measurement|01_Dimensions_and_Measurement]] — Fundamental dimensions and units
- [[01 Physics and Math/02_Time_Mass_and_Motion|02_Time_Mass_and_Motion]] — Mass, momentum, and motion
- [[01 Physics and Math/06_Energy_and_Power|06_Energy_and_Power]] — Work, energy, and power relationships
- [[01 Physics and Math/08_Engineering_Materials|08_Engineering_Materials]] — Material properties and selection

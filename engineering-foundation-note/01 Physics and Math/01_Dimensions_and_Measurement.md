---
tags: [dimensions, units, measurement, engineering, SI, USCS, length, area, volume]
source: "Engineering Fundamentals: An Introduction to Engineering — Saeed Moaveni, Ch 6–7"
---

# Dimensions and Measurement

## Fundamental Dimensions

All physical phenomena can be described using **seven fundamental (base) dimensions**. Every engineering variable is built from these.

| Fundamental Dimension | Symbol | SI Unit | Related Engineering Variables |
|---|---|---|---|
| Length | L | meter (m) | Area (L²), Volume (L³), Radian (L/L), Strain (L/L), Area moment of inertia (L⁴) |
| Time | θ (t) | second (s) | Speed (L/t), Acceleration (L/t²), Angular speed (1/t), Volume flow rate (L³/t) |
| Mass | M | kilogram (kg) | Density (M/L³), Mass flow rate (M/t), Momentum (ML/t), Specific volume (L³/M) |
| Force | F | newton (N) | Pressure (F/L²), Stress (F/L²), Work/Energy (FL), Moment (FL), Power (F/t) |
| Temperature | θ | kelvin (K) | Thermal expansion (L/LT), Specific heat (FL/MT) |
| Electric Current | I | ampere (A) | Charge (It), Current density (I/L²) |
| Luminous Intensity | J | candela (cd) | — |

> All other engineering quantities are **derived** from these fundamentals.

---

## Systems of Units

### Three Major Systems

| System | Length | Mass | Force | Time |
|---|---|---|---|---|
| **SI** (International) | meter (m) | kilogram (kg) | newton (N) = kg·m/s² | second (s) |
| **BG** (British Gravitational) | foot (ft) | slug | pound-force (lbf) | second (s) |
| **USCS** (U.S. Customary) | foot (ft) | pound-mass (lbm) | pound-force (lbf) | second (s) |

### SI Prefixes (Commonly Used)

| Prefix | Symbol | Factor |
|---|---|---|
| giga | G | 10⁹ |
| mega | M | 10⁶ |
| kilo | k | 10³ |
| centi | c | 10⁻² |
| milli | m | 10⁻³ |
| micro | μ | 10⁻⁶ |
| nano | n | 10⁻⁹ |

### Key Conversion Factors

- 1 ft = 0.3048 m
- 1 in = 2.54 cm
- 1 mile = 1.609 km = 5280 ft
- 1 lb (force) = 4.448 N
- 1 lb (mass) = 0.4536 kg
- 1 slug = 14.59 kg
- °C = (5/9)(°F − 32)
- K = °C + 273.15

---

## Unit Conversion Method

**Always multiply by conversion factors equal to 1** (e.g., 1 ft / 0.3048 m) and cancel units:

$$\text{Value}_{\text{new}} = \text{Value}_{\text{old}} \times \prod \text{conversion factors}$$

**Example:** Convert 65 mph to m/s

$$65 \, \frac{\text{mi}}{\text{h}} \times \frac{5280 \, \text{ft}}{1 \, \text{mi}} \times \frac{0.3048 \, \text{m}}{1 \, \text{ft}} \times \frac{1 \, \text{h}}{3600 \, \text{s}} = 29.06 \, \text{m/s}$$

> **Rule:** Always show units with your calculations. Units must cancel properly — if they don't, your setup is wrong.

---

## Dimensional Homogeneity

> **Every valid engineering equation must be dimensionally homogeneous** — all additive terms must have the same dimensions.

**Example:** The bar deflection formula δ = PL / (AE)

- Left side: δ has units of **m**
- Right side: P(N) × L(m) / [A(m²) × E(??)]
- For homogeneity: E must have units of **N/m²** (pascals)

**Check any formula** by substituting fundamental units on both sides. If dimensions don't match, the equation is wrong.

---

## Numerical vs Symbolic Solutions

| Approach | Description | Advantage |
|---|---|---|
| **Numerical** | Substitute data values immediately | Direct computation |
| **Symbolic** | Keep variables, simplify first, substitute last | See variable relationships; easy to re-run with different inputs |

Symbolic solutions are generally preferred — they reveal how each variable affects the result.

---

## Significant Digits

**Significant digits** convey the precision of a measurement.

### Rules

- **Leading zeros** are NOT significant: 0.00125 → 3 significant digits
- **Trailing zeros** after a decimal point ARE significant: 1500.0 → 5 significant digits
- Use **scientific notation** to clarify: 1.5 × 10³ (2 sig figs) vs 1500.0 (5 sig figs)
- Record measurements as value ± least count (e.g., 71 ± 1 °F)

### Arithmetic Rules

| Operation | Rule |
|---|---|
| Addition / Subtraction | Result precision = least precise input (last common column) |
| Multiplication / Division | Result = fewest significant digits among inputs |

**Example:** 152.47 + 3.9 = 156.**3** (not 156.37 — 3.9 only has 1 decimal place)

---

## Length (Ch 7)

### Units of Length (Increasing Order)

| Unit | Equivalent |
|---|---|
| 1 angstrom | 10⁻¹⁰ m |
| 1 micrometer (μm) | 10⁻⁶ m |
| 1 mil | 1/1000 in = 2.54 × 10⁻⁵ m |
| 1 millimeter (mm) | 10⁻³ m |
| 1 centimeter (cm) | 10⁻² m |
| 1 inch (in) | 2.54 cm |
| 1 foot (ft) | 12 in |
| 1 yard (yd) | 3 ft |
| 1 meter (m) | ≈ 1.094 yd |
| 1 kilometer (km) | 1000 m |
| 1 mile | 1.609 km ≈ 5280 ft |

### Measurement Tools

- **Ruler / tape measure** — general use
- **Micrometer / Vernier caliper** — precision to 0.001 in
- **Electronic Distance Measuring (EDMI)** — surveying
- **GPS** — satellite-based location

### Radians and Strain (Dimensionless Length Ratios)

- **Radian**: θ = S / R (arc length / radius) → unitless
- 2π rad = 360°; 1 rad ≈ 57.3°
- **Strain**: ε = ΔL / L (deformation / original length) → unitless

---

## Area (L²)

### Why Area Matters

- Heat transfer rate ∝ exposed surface area
- Drag force ∝ frontal area
- Pressure = Force / Area
- Buoyancy, mass transfer, insulation — all area-dependent

> **Volume-to-area insight:** Dividing a 1m cube into 64 smaller cubes preserves volume (1 m³) but quadruples surface area (6 → 24 m²). This explains why crushed ice cools faster than ice cubes.

### Area Units

| Unit | Equivalent |
|---|---|
| 1 mm² | 10⁻⁶ m² |
| 1 cm² | 100 mm² |
| 1 in² | 645.16 mm² |
| 1 ft² | 144 in² |
| 1 m² | ≈ 1.196 yd² |
| 1 acre | 43,560 ft² |
| 1 km² | 247.1 acres |

### Area Formulas

| Shape | Formula |
|---|---|
| Triangle | A = ½bh |
| Rectangle | A = bh |
| Trapezoid | A = ½(a + b)h |
| Circle | A = πR² |
| Ellipse | A = πab |
| n-sided polygon | A = nb² cot(180°/n) |
| Cylinder (lateral) | A = 2πRh |
| Cone (lateral) | A = πRs = πR√(R² + h²) |
| Sphere | A = 4πR² |

### Approximating Irregular Areas

1. **Trapezoidal rule**: A ≈ h[½y₀ + y₁ + y₂ + ⋯ + yₙ₋₁ + ½yₙ]
2. **Counting squares** on a grid
3. **Subtracting** unwanted areas from a bounding primitive
4. **Weighing** cut-out paper profiles (uniform density assumption)

---

## Volume (L³)

### Volume Units (Increasing Order)

| Unit | Equivalent |
|---|---|
| 1 milliliter (mL) | 1/1000 L |
| 1 teaspoon | ≈ 4.93 mL |
| 1 fluid ounce | ≈ 29.6 mL |
| 1 cup | 8 fl oz |
| 1 pint | 2 cups |
| 1 quart | 2 pints |
| 1 liter (L) | 1000 cm³ ≈ 4.2 cups |
| 1 gallon | 4 quarts |
| 1 ft³ | 7.48 gallons |
| 1 m³ | 1000 L ≈ 264 gal ≈ 35.3 ft³ |

### Volume Formulas

| Shape | Formula |
|---|---|
| Cylinder | V = πR²h |
| Right circular cone | V = ⅓πR²h |
| Sphere | V = ⁴⁄₃πR³ |
| Section of a cone | V = ⅓h(R₁² + R₂² + R₁R₂) |
| Section of a sphere | V = ⅙h(3a² + 3b² + h²) |

### Buoyancy

$$F_B = \rho V g$$

where ρ = fluid density (kg/m³), V = displaced volume (m³), g = 9.81 m/s².

> Buoyancy force equals the weight of the fluid displaced — used in ship design, submarine ballast, astronaut training (underwater neutral buoyancy simulation).

---

## Nominal vs Actual Sizes

Manufacturers use **nominal sizes** (round numbers) for easy reference, but **actual dimensions** differ:

- **Lumber**: A "2×4" is actually ≈ 1.5 × 3.5 inches
- **Pipes/tubes**: Nominal bore ≠ actual OD/ID (varies by wall thickness schedule)
- **Structural beams**: W14×38 has actual depth of 14.10 in

> Always use **actual dimensions** for engineering calculations, not nominal sizes.

---

## Coordinate Systems

| Type | Coordinates | Common Use |
|---|---|---|
| **Cartesian** (rectangular) | (x, y, z) | General engineering, navigation |
| **Cylindrical** | (r, θ, z) | Pipes, rotation problems |
| **Spherical** | (r, θ, φ) | Aerospace, radar, GPS |

---

## Key Takeaways

1. **Seven fundamental dimensions** describe all physical quantities
2. **SI is the global standard** — learn it thoroughly; convert to/from USCS as needed
3. **Dimensional homogeneity** is a powerful error-detection tool
4. **Significant digits** communicate measurement precision
5. **Area** governs heat transfer, drag, pressure, and mass transfer
6. **Volume** governs density, buoyancy, capacity, and fluid systems
7. **Always show units** in calculations — they catch errors


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[01 Physics and Math/02_Time_Mass_and_Motion|02_Time_Mass_and_Motion]] — Next: time, mass, and motion fundamentals
- [[01 Physics and Math/03_Force_and_Mechanics|03_Force_and_Mechanics]] — Force, stress, and mechanical properties
- [[01 Physics and Math/09_Math_Stats_and_Economics|09_Math_Stats_and_Economics]] — Mathematical tools for engineering calculations

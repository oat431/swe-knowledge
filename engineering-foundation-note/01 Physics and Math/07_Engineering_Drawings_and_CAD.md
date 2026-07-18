---
tags: [engineering-drawings, cad, graphical-communication, engineering]
---

# Engineering Drawings and CAD

> **Source:** *Engineering Fundamentals: An Introduction to Engineering* — Ch 16, Saeed Moaveni

Engineering drawings convey product information — shape, dimensions, materials, assembly steps — in a standard format that engineers, machinists, and technicians can all read. Discipline-specific drawings also exist: civil engineers use land/topographic/construction drawings; electrical engineers use PCB assembly drawings, drill plans, and wiring diagrams.

---

## 16.1 Importance of Engineering Drawing

- A good technical drawing communicates shape, size, material, and assembly clearly
- Eliminates ambiguity — a machinist can make the part from the drawing alone
- "A picture is worth a thousand words" — in engineering, a good drawing is worth even more

## 16.2 Orthographic Views

**Orthographic views** are 2D projections of an object onto principal planes — what the object looks like from top, front, and side.

### Projection Concept

Imagine placing the object inside a glass box. Projecting perpendicular lines from the object's corners onto each face gives the **orthographic projection** on the horizontal, vertical, and profile planes. Unfolding the box gives the standard layout of views.

### Standard Views

Six possible views: **top, bottom, front, back, left-side, right-side**. Redundancy means you rarely need all six.

- **Three views** (top, front, right-side) are most common and sufficient for most objects
- Simple objects (washers, gaskets) may need only **one view** + thickness dimension
- Objects like bolts may need only **two views**

### Line Types

| Line Type | Purpose |
|---|---|
| **Solid (visible)** | Visible edges of planes or intersections |
| **Dashed (hidden)** | Edges/hole limits not visible from viewing direction — material exists between observer and edge |
| **Center line** | Center of holes, cylinders, axes of symmetry |

> Pay attention to the pattern differences between dashed lines and center lines.

## 16.3 Dimensioning and Tolerancing

Every engineering drawing must include: **dimensions, tolerances, material type, finished surface marks, part numbers**.

### Two Core Concepts

- **Size** — how wide, long, tall the object is
- **Location** — where the center of a hole or fillet is positioned

### Dimensioning Elements

| Element | Function |
|---|---|
| **Dimension lines** | Show the size of the object |
| **Extension lines** | Extend from measurement points; dimension lines sit between them |
| **Center lines** | Mark centers of holes/cylinders |
| **Leaders** | Arrows pointing to circles/fillets to specify their size |

### Drawing Information Box

Every drawing must contain: preparer name, drawing title, date, scale, sheet number, drawing number — usually in the upper- or lower-right corner.

### Fillets

**Fillets** are rounded edges. Their radius must always be specified. Sharp edges cause **stress concentrations** — rounding edges with fillets creates gradual cross-section transitions and reduces failure risk under load.

### Tolerancing

When you specify a dimension (e.g., 2.50 cm), the actual machined part must be close enough for parts to fit together. **Tolerancing** defines the acceptable range (e.g., ±0.01 cm).

- Governed by **ANSI** standards
- Includes linear tolerances, angular tolerances, and geometric tolerancing symbols

## 16.4 Isometric View

An **isometric drawing** shows all three dimensions (width, height, depth) in a single view — used in parts manuals, repair manuals, and product catalogs.

### Drawing Steps

1. **Draw isometric axes** — width and depth axes at **30°** to horizontal; height axis at **90°** to horizontal (60° to each other axis)
2. **Mark total width, height, and depth** on the axes
3. **Create work faces** — draw front, top, and side faces using parallel lines
4. **Complete the drawing** — finish lines and remove unwanted construction lines

### Key Rule

All lines on an isometric drawing are either **parallel to an isometric axis** or **perpendicular to one**. Lines not following this rule would distort the view.

## 16.5 Sectional Views

For objects with **complex interiors** (many holes, cavities), dashed lines on orthographic views become confusing. **Sectional views** reveal the interior by making an imaginary cut through the object.

### Creating a Sectional View

1. Define the **cutting plane** and **direction of sight** (marked with directional arrows and identifying letters, e.g., "A–A")
2. Show the cut surface with **cross-hatching** — parallel inclined lines marking solid material (cross-hatch patterns also indicate material type)
3. Voids (holes, cavities) are left un-hatched

### Section Types

| Type | Description |
|---|---|
| **Full section** | Cutting plane passes completely through the object |
| **Half section** | For symmetrical objects — half sectional, half exterior view; shows interior and exterior in one view |
| **Rotated section** | Cross section rotated 90° and shown in the plane of view — for uniform cross-sections with hard-to-visualize shapes |
| **Removed section** | Like rotated, but drawn adjacent to the view — for variable cross-sections; multiple cuts may be shown |

## 16.6 Civil, Electrical, and Electronic Drawings

### Civil Engineering

- Land/boundary, topographic, construction, connection/reinforcement detail, route survey drawings
- Based on **surveys** measuring distance, direction, and elevation

### Electrical/Electronic Engineering

- Printed circuit board (PCB) assembly drawings
- PCB drill plans
- Wiring diagrams

## 16.7 Solid Modeling

Solid modeling software (AutoCAD, IDEAS, Pro/E) creates 3D models with realistic surfaces and volumes.

### Why Use Solid Models

- **Visualization** — see parts before manufacturing
- **Assembly simulation** — test fit on screen before building
- **Quick iteration** — change shape/size rapidly
- **CNC integration** — send drawings directly to computer numerically controlled machines
- **Analysis** — run stress, thermal, or other simulations on the model

### Bottom-Up Modeling

Build from primitives upward:

```
Keypoints (vertices) → Lines (edges) → Areas (surfaces) → Volumes (solids)
```

**Area generation methods:**
- Drag a line along a path
- Rotate a line about an axis
- Area fillet (constant-radius tangent to two areas)
- Skinning (smooth surface over a set of lines)
- Offset (inflate/deflate existing area)

**Volume generation:**
- **Extrude** — drag an area along a path
- **Revolve** — rotate an area about an axis

### Top-Down Modeling

Start with **primitives** — simple geometric shapes:

| 2D Primitives | 3D Primitives |
|---|---|
| Rectangle, circle, polygon | Block, prism, cylinder, cone, sphere |

### Boolean Operations

Combine or modify solid entities:

| Operation | Effect |
|---|---|
| **Union (add)** | Merge two volumes into one |
| **Subtract** | Remove one volume from another (e.g., punching holes) |
| **Intersection** | Keep only the overlapping volume |

> Example: To create a bracket with holes — extrude the profile, create solid cylinders for the holes, then **subtract** the cylinders from the block.

## 16.8 Why Do We Need Engineering Symbols?

Engineering symbols are a **language** — they convey information quickly, effectively, and inexpensively. Like traffic signs (stop = octagon + red), engineering symbols use standardized shapes and conventions to communicate without words.

## 16.9 Common Engineering Symbols

### Electrical Symbols

- Fixed/variable resistor, diode, capacitor, LED, solar cell
- Photo resistor, lamp, battery, op-amp, meter, speaker
- Single-phase motor

### Logic Symbols

| Gate | Function |
|---|---|
| **AND** | Output ON only if ALL inputs are ON |
| **OR** | Output ON if ANY input is ON |
| **NOT** | Inverts the single input |
| **NAND** | NOT + AND |
| **NOR** | NOT + OR |

### Plumbing and Piping Symbols

- Fixtures: bathtub, shower, floor drain, drinking fountain
- Piping: 45°/90° elbows, expansion joints, tees, crosses
- Valves: gate valve, globe valve, check valve

### Other Common Symbols

- Natural gas line, electric service
- Point of Beginning (POB) in surveying
- Material indicators: concrete, glass, steel

---

## Key Takeaways

- Orthographic views (typically 3) describe an object's shape using solid, dashed, and center lines
- Dimensioning requires specifying both **size** and **location** per ANSI standards
- Tolerancing defines acceptable dimensional variation for proper part fit
- Isometric drawings show 3D objects in a single view using 30° axes
- Sectional views reveal complex interiors that hidden lines can't clearly show
- Solid modeling enables 3D design, simulation, and CNC manufacturing using bottom-up or top-down approaches with Boolean operations
- Engineering symbols (electrical, logic, plumbing) form a universal language for cross-discipline communication

## Related

- [[Engineering Foundation Overview]]
- [[01 Physics and Math/08_Engineering_Materials|08_Engineering_Materials]] — Material properties that drawings specify
- [[01 Physics and Math/09_Math_Stats_and_Economics|09_Math_Stats_and_Economics]] — Mathematical tools for engineering analysis

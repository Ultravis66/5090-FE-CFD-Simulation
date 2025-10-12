# This is a work in progress!
- I am still working on realistic physics for the PCB/Die/Vapor Chamber to match real world test data.
# 5090-FE-CFD-Simulation
This is a simulation of the Founders Edition 5090 Nvidia GPU:




## Domain and Material Setup

The GPU cooling simulation domain includes the following major components:

| Continua       | Type  | Density (kg/m³) | Specific Heat (J/kg·K) | Thermal Conductivity (W/m·K) | Notes |
|----------------------|-------|------------------|--------------------------|-------------------------------|-------|
| **Fluid Volume**     | Fluid | 1.18 | 1003.62  | 0.026 | Air domain for conjugate heat transfer (CHT) |
| **PCB**              | Solid | 1800.0 | 1100.0 | 0.5 | Represents PCB substrate (low-conductivity composite) |
| **Die**              | Solid | 2330.0 | 700.0  | 130.0 | Silicon die with high thermal conductivity |
| **Vapor Chamber**    | Solid | 8800.0 | 400.0  | Orthotropic: 20,000 (axial and traverse), 1500 (transverse) | vapor chamber with embedded heat pipes |
---
## Domain and Material Setup

The GPU cooling simulation domain includes the following major regions:

| Region / Part | Type | Notes |
|----------------|------|-------|
| **PCB**           | Solid | PCB substrate (low-conductivity composite) |
| **Die**           | Solid | Silicon die (high thermal conductivity) |
| **Vapor Chamber** | Solid | Orthotropic: 12,000 W/m·K (axial and vertical off the Die), 150 W/m·K perpendicular to the heat pipes |
| **Fan 1 / Fan 2** | Overset Region | Rotating meshes with **Rigid Body Motion (RBM)** applied |

### Heat Sink Porous Medium Properties

**Porosity:** 0.667  

**Orthotropic Inertial Resistance:**  
- xx: 10,000.0 kg/m⁴  
- yy: 300.0 kg/m⁴  
- zz: 500.0 kg/m⁴  

**Orthotropic Viscous Resistance:**  
- xx: 10,000.0 kg/m³·s  
- yy: 250.0 kg/m³·s  
- zz: 500.0 kg/m³·s  

**Thermal Conductivity (Orthotropic):**  
- xx: 0.026 W/m·K  
- yy, 237.0 W/m·K  
- zz, 237.0 W/m-k

---
**Case**
- sides set to walls
- inlet at the bottom set to 0.6 m/s
- outlet at the top
---
**Click To Play video on Youtube**
---
[![Transient Startup of Heat Transfer initialized at 300 K](https://img.youtube.com/vi/3fXF4ODPPvA/0.jpg)](https://www.youtube.com/watch?v=3fXF4ODPPvA "Play on YouTube")
- First test run to ensure thermals and fans are spinning
- Flow through porous medium working
- Heat transfer through all mediums working as intended
- Done to ensure all multiphysics is working as intended

💡 **Notes:**
- The conjugate heat transfer (CHT) interface couples the **die, vapor chamber, heat sink, and air** regions.
- Fans are represented as rotating reference frames (MRF) or rigid-body motion (RBM) zones depending on simulation setup (both cases were sim'ed).
- The **heat sink** is treated as a **porous region** to account for detailed fin flow resistance while retaining conjugate heat transfer effects.  
- The vapor chamber is modeled as an orthotropic solid to represent the high in-plane heat spreading provided by the embedded wick and vapor core. In detailed simulations, a true vapor chamber is often represented by a five-layer structure (top cover, top wick, vapor core, bottom wick, and bottom cover) to capture phase-change and capillary effects. For demonstration purposes, this model simplifies the vapor chamber into a single anisotropic solid block with equivalent thermal properties and can be off by as mubh as 10%-15%.

---

## Mesh Setup

Below are views of the GPU simulation mesh:

![GPU Mesh - Front View](GPU_Mesh_1.png)
![GPU Mesh - Isometric View](GPU_Mesh_2.png)

![GPU Mesh - Boundary Layer](BL_1.png)
![GPU Mesh - Boundary Layer Zoomed](BL_2.png)

![GPU Mesh - Internals](GPU_Mesh_Int1.png)
![GPU Mesh - Internals](GPU_Mesh_Int2.png)

- This visualization models transient heat transfer through the RTX 5090 FE under a 600 W thermal load. The GPU die receives 450 W, and the PCB receives 150 W (future project would actually model the memory modules).
- Dual fans are simulated with Rigid Body Motion (RBM) to generate realistic airflow and convection through the porous heat-sink region.
- The simulation captures full heat transfer across the die, vapor chamber, heat sink, and PCB.

[![Heat Transfer though Video Card](https://www.youtube.com/watch?v=_VzF4xa2fiY.jpg)(https://www.youtube.com/watch?v=_VzF4xa2fiY)

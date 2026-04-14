# Integrated_System_Architecture
lab1: Design and Implementation of an IIR Filter 
lab2: Integrated Systems Architecture: Digital Arithmetic and FPU Optimization

Based on the provided project documentation, here is a structured README.md file you can use for your GitHub repository.

**Design and Implementation of an IIR Filter**

This project focuses on the design, simulation, and VLSI implementation of a digital **Infinite Impulse Response (IIR) filter**, following both standard and advanced architectural approaches. It was developed as part of the **Integrated Systems Architecture** course at Politecnico di Torino (2024-2025).

***Project Overview**

The goal is to implement a digital filter with the following specifications:

- **Cut-off frequency:** 2 kHz
- **Sampling frequency:** 10 kHz
- **Filter Type:** Infinite Impulse Response (IIR)
- **Order (\$N\$):** 1
- **Coefficient Bits (\$nb\$):** 13 bits

***Repository Structure**

- /src/matlab: Filter design, coefficient quantization, and pseudo-fixed-point verification.
- /src/c: Fixed-point C model (myfilterii.c) for bit-true simulation and THD calculation.
- /src/vhdl: Structural VHDL implementation of the filter and testbench.
- /syn: Logic synthesis scripts and reports (area, timing, and power).
- /layout: Place and Route results generated using Cadence Innovus.

***Implementation Phases**

***1\. Reference Model Development**

- ***Matlab:** Used to calculate quantized coefficients and verify frequency response.
- ***C Model:** Validated the fixed-point behavior. A shift amount (**SHAMT**) of 17 was chosen to achieve a Total Harmonic Distortion (THD) of **\-41.9217 dB**.

***2\. VLSI Implementation**

The architecture was implemented in VHDL using a series of registers, adders, and multipliers.

- ***Logic Synthesis:*** Performed using Synopsys Design Compiler.
- ***Optimization:*** Compared designs with and without **Clock Gating**.
  - _Clock Gating Results:_ Improved minimum period to **5.48 ns** and reduced power consumption to **234.96 µW**.
- ***Place and Route:*** Final layout area measured at **4034 µm²** with total power consumption of **613 µW** at a reduced frequency.

***3\. Advanced Architecture (Lookahead)***

To increase throughput and reduce the critical path, advanced techniques were applied:

- ***J-Lookahead Approach:*** Modified filter equations to calculate future outputs in advance.
- ***Pipelining & Retiming:*** Reduced the critical path from \$2Tm + 2Ta\$ to \$Tm\$.
- **Performance Gain:*** Increased the maximum clock frequency to **568.18 MHz** with clock gating enabled.

***Tools Used***

- ***Matlab:*** Filter design and verification.
- ***Questasim (QSim):*** Functional and timing simulation.
- ***Synopsys Design Compiler:*** Logic synthesis.
- ***Cadence Innovus:*** Physical design (Place and Route).

***Authors***

- Xu Menghan
- Abedi Maryam
- Shanchong Shang
- Milad Rahmani Nezhad

***Professor:*** Guido Masera

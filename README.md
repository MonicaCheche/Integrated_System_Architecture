# Integrated_System_Architecture
**lab1**: Design and Implementation of an IIR Filter.

**lab2**: Integrated Systems Architecture: Digital Arithmetic and FPU Optimization

Based on the provided project documentation, here is a structured README.md file you can use for your GitHub repository.

## LAB1 Design and Implementation of an IIR Filter

This project focuses on the design, simulation, and VLSI implementation of a digital **Infinite Impulse Response (IIR) filter**, following both standard and advanced architectural approaches. It was developed as part of the **Integrated Systems Architecture** course at Politecnico di Torino (2024-2025).

**Project Overview**

The goal is to implement a digital filter with the following specifications:

- **Cut-off frequency:** 2 kHz
- **Sampling frequency:** 10 kHz
- **Filter Type:** Infinite Impulse Response (IIR)
- **Order (\$N\$):** 1
- **Coefficient Bits (\$nb\$):** 13 bits

**Repository Structure**

- /src/matlab: Filter design, coefficient quantization, and pseudo-fixed-point verification.
- /src/c: Fixed-point C model (myfilterii.c) for bit-true simulation and THD calculation.
- /src/vhdl: Structural VHDL implementation of the filter and testbench.
- /syn: Logic synthesis scripts and reports (area, timing, and power).
- /layout: Place and Route results generated using Cadence Innovus.

**Implementation Phases**

**1\. Reference Model Development**

- **Matlab:** Used to calculate quantized coefficients and verify frequency response.
- **C Model:** Validated the fixed-point behavior. A shift amount (**SHAMT**) of 17 was chosen to achieve a Total Harmonic Distortion (THD) of **\-41.9217 dB**.

**2\. VLSI Implementation**

The architecture was implemented in VHDL using a series of registers, adders, and multipliers.

- **Logic Synthesis:** Performed using Synopsys Design Compiler.
- **Optimization:** Compared designs with and without **Clock Gating**.
  - _Clock Gating Results:_ Improved minimum period to **5.48 ns** and reduced power consumption to **234.96 µW**.
- **Place and Route:** Final layout area measured at **4034 µm²** with total power consumption of **613 µW** at a reduced frequency.

**3\. Advanced Architecture (Lookahead)**

To increase throughput and reduce the critical path, advanced techniques were applied:

- **J-Lookahead Approach:** Modified filter equations to calculate future outputs in advance.
- **Pipelining & Retiming:** Reduced the critical path from \$2Tm + 2Ta\$ to \$Tm\$.
- **Performance Gain:** Increased the maximum clock frequency to **568.18 MHz** with clock gating enabled.

**Tools Used**

- **Matlab:** Filter design and verification.
- **Questasim (QSim):** Functional and timing simulation.
- **Synopsys Design Compiler:** Logic synthesis.
- **Cadence Innovus:** Physical design (Place and Route).

**Authors**

- Xu Menghan
- Abedi Maryam
- Shanchong Shang
- Milad Rahmani Nezhad

**Professor:** Guido Masera


## Integrated Systems Architecture: Digital Arithmetic and FPU Optimization

This project focuses on the implementation and optimization of a Floating Point Unit (FPU) supporting the **bfloat16** format, specifically targeting machine learning and near-sensor computing applications. It includes the design of a custom **R4-MBE Multiplier** using a **Dadda Tree** architecture to improve performance over standard library implementations.

+3

**Authors**

- **XU MENGHAN**
- **Abedi Maryam**
- **SHANCHONG SHANG**
- **Milad Rahmani Nezhad**
- **Professor:** Guido Masera

**1\. Project Overview**

The FPU operates on the 16-bit **bfloat16** format, which consists of:

+1

- **1 bit** for the sign.
- **8 bits** for the exponent.
- **7 bits** for the mantissa (fraction).

The project explores various logic synthesis strategies using **Synopsys Design Compiler** to optimize area, timing, and power.

+1

**2\. Synthesis & Optimization Strategies**

Five different synthesis methods were compared to evaluate the trade-offs between speed (Maximum Frequency) and silicon area:

| **Synthesis Method**            | **Min Clock Period (ns)** | **Max Frequency (MHz)** | **Area (µm²)** |
| ------------------------------- | ------------------------- | ----------------------- | -------------- |
| **Basic Compile**               | 2.87                      | 348.43                  | 2937.43        |
| **Compile + Retiming**          | 2.12                      | 471.70                  | 3634.35        |
| **Compile Ultra**               | 2.53                      | 395.26                  | 3351.60        |
| **Compile + Retiming + CSA**    | 2.31                      | 432.90                  | 3547.64        |
| **Compile + Retiming + PPARCH** | 2.35                      | 425.53                  | 3432.46        |

**Key Optimization Techniques:**

- **Retiming (optimize_registers):** Shortens the clock cycle by moving registers across logic boundaries, though it typically increases area.

+1

- **Carry-Save Adder (CSA):** Efficient for high-speed multiplication by handling carry signals in parallel.

+1

- **Parallel-Prefix Architecture (PPARCH):** Uses tree-based structures (e.g., Kogge-Stone) to propagate carries quickly with better area efficiency than CSA.

+2

**3\. Custom R4-MBE Multiplier Architecture**

To further optimize the FPU's mantissa multiplication, a custom module (topmultiplier.sv) was designed to replace the standard library multiplier.

**Features:**

- **Radix-4 Modified Booth Encoding (MBE):** Reduces the number of partial products by half.

+1

- **Dadda Tree Compression:** Systematically compresses the partial product bit matrix using Full Adders (FA) and Half Adders (HA) to minimize critical path delay.

+2

- **Sign Extension Logic:** Efficiently handles 2's complement representation for negative partial products.

+1

**Performance Impact:**

The custom multiplier improved the critical path from **2.53ns** (Compile Ultra default) to **2.51ns**, representing a successful optimization of the arithmetic datapath.

**4\. How to Run**

**Simulation**

Functional verification was performed using **QuestaSim**.

- To test the standalone multiplier: use tb_topmultiplier.v.
- To test the whole FPU: use the unzipped cvfpu_lite environment.

+1

**Synthesis**

Synthesis scripts are provided in the Appendix section of the report. The primary tool used is **Synopsys Design Vision**.

+1

- Example command for Compile Ultra:

Tcl

analyze -f sverilog -lib WORK {source_files}

elaborate fpnew_top

create_clock -name MY_CLK -period 2.53 clk_i

compile_ultra

**5\. Directory Structure**

- /src: Verilog/SystemVerilog source files for the FPU and custom multiplier.
- /tb: Testbench files for RTL and Netlist verification.
- /syn: Synthesis reports including area, timing, and resource utilization.

## Topic 1: Quantitative Design & Performance + ISA Review (RISC-V)

### I. The Necessity of a Quantitative Approach

The field of computer architecture is characterized by constant and rapid evolution. To navigate this landscape, architecture must be studied using **real examples and measurements on real computers**.

The methodology employed is fundamentally **quantitative**, relying on empirical observations of programs, experimentation, and simulation. This approach emphasizes balancing marketplace forces across three key factors: **cost-performance-power**.

**Current Challenges and Trends:**

1.  **Slowing of Scaling Laws:** Much of the improvement in performance over the past 40 years relied on Moore’s Law (transistor count doubling every $\approx 2$ years) and Dennard scaling (power density staying roughly constant as transistors shrink).
2.  **The End of Dennard Scaling** occurred about a decade ago.
3.  **Moore’s Law is Fading**.
4.  **The New Path:** The only path left to improve cost, performance, and energy efficiency is **specialization**, leading to the rise of **Domain-Specific Architectures (DSAs)** (Chapter 7).

### II. Quantitative Principles of Computer Design

Architects rely on core principles to analyze and improve system designs:

#### A. Exploiting Program Properties

1.  **Take Advantage of Parallelism:** This is the most important method for improving performance and is leveraged throughout computer architecture (e.g., pipelining, multiple cores, etc.).
2.  **Principle of Locality:** Programs tend to reuse data and instructions that were recently used. This is exploited through:
    *   **Temporal Locality:** Recently accessed items are likely to be accessed soon.
    *   **Spatial Locality:** Items whose addresses are near one another tend to be referenced close together in time.
3.  **90/10 Locality Rule:** A common rule of thumb stating that a program executes about **$90\%$ of its instructions in $10\%$ of its code**.

#### B. Performance Metrics and Laws

1.  **Performance Measurement:** Performance can be measured by **response time** (time between start and completion of an event) or **throughput** (total amount of work done in a given time).
2.  **Amdahl’s Law:** Used to calculate the overall speedup gained from enhancing a portion of a task.
    $$ Speedup_{overall} = \frac{1}{(1 - Fraction_{enhanced}) + \frac{Fraction_{enhanced}}{Speedup_{enhanced}}} $$
    Amdahl’s Law shows the **law of diminishing returns**: improving a portion of the computation yields smaller incremental speedup gains as more improvements are added.
3.  **The Processor Performance Equation:** CPU execution time is calculated as:
    $$ CPU time = Instruction\ count \times Cycles\ per\ instruction \times Clock\ cycle\ time $$
    This equation relies on:
    *   **Instruction Count (IC):** Determined by Instruction Set Architecture (ISA) and compiler technology.
    *   **Cycles Per Instruction (CPI):** Determined by organization (microarchitecture) and ISA.
    *   **Clock Cycle Time:** Determined by hardware technology and organization.

#### C. Energy/Power Trade-offs

Energy efficiency is now a **constant companion to performance** in design. Minimizing energy often involves architectural trade-offs:

*   A 32-bit DRAM access takes about **20,000 times as much energy** as an 8-bit integer addition.
*   The **Watt-Year Rule** suggests the fully burdened cost of a Watt per year in a Warehouse Scale Computer is about **\$2** (in North America in 2011).

### III. Instruction Set Architecture (ISA) Review

The **Instruction Set Architecture (ISA)** is the boundary between hardware and software, visible to the programmer or compiler writer. This material is now often found in appendices because the ISA is playing a smaller role today than in 1990.

#### A. Key ISA Design Decisions

1.  **Class of ISA:** Nearly all modern ISAs are **General-Purpose Register (GPR) architectures**, where operands are registers or memory locations.
    *   **Load-Store (Register-Register):** Access memory only with load or store instructions (e.g., RISC-V, ARMv8). This simplifies implementation and allows for simple, fixed-length instruction encoding, favorable for pipelining.
    *   **Register-Memory:** Can access memory as part of many instructions (e.g., 80x86).
2.  **Number of Registers:** Architects recommend at least **16, and preferably 32, general-purpose registers** to simplify register allocation by compilers.
3.  **Memory Addressing Modes:** Displacement and Immediate addressing dominate usage.
    *   Displacement field size should be at least 12–16 bits to capture most displacements.
    *   Immediate field size should be at least 8–16 bits.
4.  **Instruction Encoding:**
    *   **Fixed Length** (e.g., RISC-V core): Simplifies instruction decoding and pipelined implementation but generally results in larger program code size.
    *   **Variable Length** (e.g., 80x86): Can use less space, leading to smaller compiled programs, but complicates decoding.
    *   **Hybrid/Compressed ISAs** (e.g., RISC-V Compressed, ARM Thumb-2): Use a mix of 16-bit and 32-bit instructions to reduce program size, especially important for embedded systems and PMDs.

#### B. Compiler Interaction Principles (Simplicity is Key)

Architects must design ISAs to be efficient compiler targets. It is often better to err on the side of simplicity:

*   **Provide primitives instead of solutions**.
*   **Simplify trade-offs** between alternatives.
*   **Don’t bind constants at runtime**.

### IV. Focus: The RISC-V Architecture

The RISC-V ISA is a **modern, modular, open, and free instruction set** developed at the University of California, Berkeley, and is used as the primary example ISA in the current edition of the textbook. It builds on 30 years of RISC experience.

The adoption of RISC-V is a key update in the sixth edition, keeping pace with developments in open-sourced architecture. It is anticipated that RISC-V may become a **significant force** in the information technology industry, comparable to Linux in operating systems.

#### A. Key Architectural Features

1.  **Architecture Class:** **Load-Store Architecture** with a simple instruction set.
2.  **Registers:**
    *   **32 General-Purpose Registers (GPRs)** ($x0$ through $x31$).
    *   Register $x0$ is **hardwired to zero**.
    *   **32 Floating-Point Registers (FPRs)** ($f0$ through $f31$).
3.  **Data Types:** Supports 8-bit bytes, 16-bit half words, 32-bit words, and 64-bit double words for integer data. Floating point support includes 32-bit single precision and 64-bit double precision, using the IEEE 754 format.
4.  **Memory Access:** Memory is **byte addressable** with a 64-bit address (for RV64). Accesses need not be aligned, though unaligned accesses may run extremely slowly.
5.  **Addressing Modes (Data):** Only supports **Immediate and Displacement** (offset + base register). Register indirect addressing is achieved by setting the 12-bit displacement field to zero.
6.  **Instruction Encoding:** Uses **fixed-length 32-bit instructions** for its base sets. It uses different formats (R-type, I-type, S-type, U-type, J-type, B-type) but ensures all instructions are 32 bits long, which simplifies decoding.

#### B. Modular Extensions

RISC-V is defined by a base instruction set (e.g., RV64I) plus optional extensions. The textbook primarily uses **RV64G**, which is an abbreviation for RV64IMAFD:

*   **I** (Integer): Base integer instruction set.
*   **M** (Multiply/Divide): Adds integer multiplication and division.
*   **A** (Atomic): Adds atomic instructions for concurrent processing.
*   **F** (Float): Adds single-precision floating point.
*   **D** (Double): Extends floating point to double precision.
*   **C** (Compressed): Defines a compressed version (16-bit instructions) for small-memory embedded applications.
*   **V** (Vector): A future extension for vector operations (Chapter 4).

RISC-V explicitly includes support for features needed in modern systems, such as **virtualization**.

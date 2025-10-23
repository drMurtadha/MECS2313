# Topic 3: Instruction-Level Parallelism (ILP) and Dynamic Scheduling

## I. Instruction-Level Parallelism (ILP): Concepts and Challenges

Instruction-Level Parallelism (ILP) refers to the potential overlap among instructions, allowing them to be evaluated in parallel to improve performance. All processors designed since approximately 1985 have used pipelining, the simplest form of ILP, to overlap instruction execution.

### A. Exploiting Parallelism
1.  **Basic Blocks and Loops:** The amount of parallelism available within a basic block (a straight-line code sequence) is generally small. To achieve substantial performance gains, ILP must be exploited across multiple basic blocks.
2.  **Loop-Level Parallelism (L-LP):** The simplest and most common way to increase ILP is to exploit parallelism among iterations of a loop. This is often called loop-level parallelism. Techniques like **loop unrolling**, performed statically by the compiler or dynamically by hardware, convert L-LP into ILP.
3.  **Approaches to Exploitation:**
    *   **Dynamic Scheduling (Hardware-based):** Relies on hardware to dynamically discover and exploit parallelism. This approach dominates desktop and server markets (e.g., recent Intel and many ARM processors).
    *   **Static Scheduling (Software/Compiler-based):** Relies on the compiler technology to find parallelism statically at compile time.

### B. Dependencies and Hazards (Limits to ILP)
Determining how one instruction depends on another is critical for determining how much parallelism can be exploited.

1.  **Data Dependence (True Dependence / RAW Hazard):** Occurs when instruction $i$ produces a result that instruction $j$ uses. This order must be preserved and implies a chain of data hazards.
2.  **Name Dependence:** Occurs when two instructions use the same register or memory location (a name) but there is no flow of data between them. These must be eliminated or managed to allow out-of-order execution:
    *   **Antidependence (WAR Hazard):** Instruction $j$ writes a location that instruction $i$ reads. The original order must be preserved to ensure $i$ reads the correct value.
    *   **Output Dependence (WAW Hazard):** Instructions $i$ and $j$ write the same location. The original order must be preserved to ensure the final value corresponds to the later instruction ($j$).
3.  **Control Dependence:** Determines the order imposed by branches, requiring an instruction to execute only if the branch outcome dictates it. Control hazards can be eliminated or reduced using hardware or software techniques like branch prediction.

### C. Compiler Techniques for ILP Exposure
Compiler scheduling attempts to minimize stalls by physically separating dependent instructions in the code.
*   **Loop Unrolling:** Replicating the loop body multiple times to increase the size of straight-line code fragments (basic blocks), thereby increasing the available ILP for the pipeline to exploit. Unrolling often requires decisions such as using different registers to avoid unnecessary constraints (name dependences), eliminating extra overhead instructions (branches and index updates), and determining that loads and stores can be safely interchanged.
*   **Register Pressure:** A limiting factor where aggressive unrolling and scheduling increase the number of live values, potentially leading to a shortage of available registers.

## II. Dynamic Scheduling

Dynamic scheduling is a hardware technique where the processor reorders instruction execution to minimize stalls while maintaining data flow and precise exception behavior.

### A. Advantages of Dynamic Scheduling
1.  It allows code compiled for one pipeline architecture to run efficiently on a different one, eliminating the need for recompilation.
2.  It handles dependencies (like memory references or data-dependent branches) unknown at compile time.
3.  **Latency Tolerance:** Most importantly, it allows the processor to tolerate unpredictable delays, such as cache misses, by executing unrelated instructions while waiting for the delay to resolve.

### B. Tomasulo's Algorithm
The IBM 360/91 floating-point unit pioneered this sophisticated scheme, which uses reservation stations to allow out-of-order execution.

1.  **Avoiding RAW Hazards:** Instructions are executed only when all their operands are available.
2.  **Eliminating WAW and WAR Hazards (Register Renaming):** This is the key difference from simpler dynamic scheduling schemes like scoreboarding. Tomasulo's uses the reservation station number as a tag, effectively renaming destination registers to ensure that out-of-order writes do not affect instructions waiting for earlier values.
3.  **The Common Data Bus (CDB):** Results are broadcast on the CDB and monitored by reservation stations. This mechanism implements forwarding and bypassing dynamically, though it typically adds one cycle of latency compared to static forwarding in a simple pipeline.

### C. Dynamic Scheduling Stages (Three Steps)
Although execution may take an arbitrary number of clock cycles, every instruction follows three steps:
1.  **Issue:** The instruction is retrieved. If an empty reservation station (RS) is available, the instruction is issued, and operands (or tags pointing to the RS that will produce them) are placed in the RS. This step renames registers.
2.  **Execute:** The instruction waits for its operands to become available on the CDB. When all are present, the operation begins at the functional unit.
3.  **Write Result:** The result is available and written onto the CDB to update the register file and any waiting reservation stations.

## III. Advanced ILP Exploitation

### A. Hardware-Based Speculation
Speculation is an extension of dynamic scheduling designed to overcome the limitations of control dependence imposed by branches. Speculation fetches, issues, and executes instructions based on predicted branch outcomes (often using dynamic branch prediction).

1.  **Handling Mis-speculation:** Mechanisms are required to undo the effects of an incorrectly speculated sequence.
2.  **Reorder Buffer (ROB):** This structure is essential for speculation. Each instruction occupies an entry in the ROB until it commits.
    *   **Precise Exceptions:** The ROB ensures that instructions commit in order (at the head of the ROB). If an instruction causes an exception, the processor waits until it reaches the head of the ROB, takes the interrupt, and flushes any pending speculative instructions, guaranteeing a precise state.
    *   **Renaming Tags:** When using speculation, the ROB entry number is typically used as the tag for result renaming, replacing the RS number.

### B. Multiple Issue (Superscalar)
Multiple-issue processors aim to achieve a CPI of less than one by issuing multiple instructions per clock cycle. Modern processors often combine multiple issue with dynamic scheduling and speculation (e.g., Intel Core i7).

1.  **Complexity:** Issuing multiple dependent instructions in a single clock cycle is complex, as the issue logic must analyze all internal dependences within the bundle and update all reservation tables and renaming structures in parallel within one clock cycle. This complexity is a significant bottleneck, growing quadratically with issue width, limiting modern designs to typically 4–8 instructions per clock.
2.  **VLIW/EPIC (Static Multiple Issue):** These processors issue a fixed number of operations per instruction packet, relying on the compiler to explicitly package parallel operations and handle hazards. VLIW/EPIC faces challenges regarding code size growth and maintaining synchronization (or lockstep operation), though modern variants address these issues.

### C. Advanced Branch Prediction
Accurate branch prediction is crucial for high-performance ILP exploitation, especially in speculative processors.
*   **Tournament Predictors:** Use multiple predictors (typically a local predictor and a global predictor) and select the most effective predictor for a specific branch using a selector based on recent performance tracking. Tournament predictors can achieve better accuracy than single predictors, especially for integer benchmarks.
*   **Tagged Hybrid Predictors:** Combine multiple history lengths and use tags to index prediction tables, offering a complex but highly accurate approach.

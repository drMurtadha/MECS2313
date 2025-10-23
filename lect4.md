
# Topic 4: Thread-Level Parallelism (TLP) and Data-Level Parallelism (DLP)
## I. Data-Level Parallelism (DLP)

Data-level parallelism arises when there are many data items that can be operated on at the same time. Architectures exploiting DLP apply a single instruction to a collection of data in parallel. This style of parallelism is highly valuable for multimedia processing, image/sound processing, scientific matrix calculations, and machine learning algorithms.

### A. Architectures Exploiting DLP

DLP is exploited through several Single Instruction, Multiple Data (SIMD) variations:

1.  **Vector Architectures**
    *   **Mechanism:** These architectures extend pipelined execution by operating on many data items in parallel, using deep pipelines and/or parallel execution units. A key advantage is energy efficiency, as performance can be maintained by doubling resources while lowering clock rate and voltage.
    *   **Vector Instructions:** A single, short instruction can launch scores of independent operations. This allows memory latency to be paid only once per vector load/store, amortizing the latency over many elements (e.g., 32 elements).
    *   **Features:** They utilize large register files as compiler-controlled buffers. They support **flexible chaining**, which is similar to dynamic forwarding in scalar pipelines. They support complex data access patterns using **strided accesses** and **gather-scatter** addressing modes. Conditional execution is supported via **mask registers**.

2.  **SIMD Instruction Set Extensions (Multimedia)**
    *   These extensions apply a single instruction to a small to moderate number of data items in parallel (typically two to eight).
    *   **Comparison to Vector:** Multimedia SIMD typically has not offered the sophisticated addressing modes (strided or gather-scatter) or mask registers for conditional execution found in vector architectures, though this is changing.

3.  **Graphics Processing Units (GPUs)**
    *   GPUs are essentially **multithreaded SIMD Processors**. They combine multiple forms of parallelism, including multithreading, MIMD, SIMD, and ILP.
    *   **SIMT Model:** The programming model, such as CUDA, is classified as Single Instruction, Multiple Thread (SIMT).
    *   **Latency Hiding:** GPUs traditionally use smaller caches and rely on **extensive multithreading** (many independent threads of SIMD instructions) to hide long latency to DRAM, thus maximizing the usage of computing resources.
    *   **Hierarchy of Parallelism (NVIDIA Jargon):**
        *   **Grid (Vectorizable Loop):** The GPU code that runs on the entire data set.
        *   **Thread Block (Body of Vectorized Loop):** Groups of threads assigned to a multithreaded SIMD Processor. Thread Blocks must execute independently.
        *   **Warp (Thread of SIMD Instructions):** A traditional thread containing only SIMD instructions, typically 32 CUDA Threads wide.
        *   **SIMD Lane (Thread Processor):** The hardware functional unit that executes operations for a single element within a SIMD instruction.
    *   **Memory Access:** GPU loads and stores are inherently **gather-scatter** operations. Hardware handles non-sequential memory accesses using **Address Coalescing** units to recognize when SIMD lanes access sequential addresses, enabling high bandwidth block transfers.

### B. Detecting DLP

The primary source of DLP is **Loop-Level Parallelism**.
*   **Loop-Carried Dependence:** For a loop to be parallel (vectorizable), there must be an absence of data dependencies between successive iterations (loop-carried dependences).
*   **Dependence Analysis:** This compiler technique is performed near the source level to determine if data accesses in later iterations depend on values produced in earlier iterations. If a loop does not have a circular chain of dependence, it can often be restructured (like loop shifting) to enable parallelism.
*   **Reductions:** Operations like summation that depend on values accumulated across iterations (reductions) can sometimes be handled by special hardware or parallelized through techniques like recurrence doubling.

---

## II. Thread-Level Parallelism (TLP)

Thread-level parallelism arises when multiple tasks of work, typically consisting of hundreds to millions of instructions, operate independently and largely in parallel. TLP is primarily exploited through Multiple Instruction, Multiple Data (MIMD) architectures, such as **multiprocessors** and **multicores**.

### A. MIMD Architectures and Memory Organization

Multiprocessors are systems where tightly coupled processors coordinate under a single operating system and share memory through a shared address space.

1.  **Symmetric Shared-Memory Multiprocessors (SMP):**
    *   **Organization:** Feature centralized shared memory, typically supporting up to about 32 cores.
    *   **Access:** Also known as Uniform Memory Access (UMA) multiprocessors, as all processors theoretically have a uniform latency to memory. However, in multichip designs, memory access is often asymmetric (Non-Uniform Memory Access, NUMA), being faster to local memory attached to the core's chip.
    *   **Implementation:** Most current multicores are SMPs.

2.  **Distributed Shared Memory (DSM):**
    *   **Organization:** Memory is physically distributed among processors to support large processor counts (hundreds or more) and provide necessary memory bandwidth.
    *   **Access:** These systems are inherently NUMA, where access time to local memory is much faster than to remote memory.

### B. Challenges of TLP

1.  **Amdahl's Law:** Parallel program speedup is limited by the fraction of the application that remains serial. Scaling to high processor counts requires the serial fraction to be near zero (e.g., to achieve a speedup of 80 with 100 processors, only $0.2\%$ of the execution time can be serial).
2.  **Communication Latency:** Remote memory access between separate chips can incur 100 to 300 or more clock cycles of delay. This latency significantly increases the processor's effective CPI if communication is frequent.

### C. Cache Coherence and Consistency

When shared data is cached in a multiprocessor, hardware protocols are necessary to maintain **cache coherence** (a consistent view of memory).

1.  **Snooping Protocols:**
    *   **Mechanism:** Used primarily in centralized shared-memory architectures (SMPs). Processors use a shared bus or broadcast medium to communicate memory requests. All processors "snoop" (monitor) the bus.
    *   **Write Invalidate:** The most common protocol; a writing processor invalidates all other cached copies of the block before writing, guaranteeing exclusive access and enforcing **write serialization**.
    *   **Scalability Issue:** Snooping does not scale well because the centralized bus/broadcast medium becomes a bottleneck as processor count or memory demand increases.

2.  **Directory Protocols:**
    *   **Mechanism:** Necessary for scalable DSM architectures. A centralized directory keeps track of the state of every block and identifies which specific nodes (processors or caches) are sharing that block (the **Sharers** set). This allows the coherence protocol to avoid broadcast by sending invalidation messages only to nodes listed in the Sharers set.
    *   **States:** Directory states often mirror cache states: **Uncached**, **Shared** (memory is up-to-date, multiple copies exist), and **Modified** (exactly one node has a dirty copy and is the owner).

3.  **Memory Consistency Models:**
    *   While coherence ensures *what* value is seen, consistency determines *when* a written value must be seen by other processors.
    *   **Sequential Consistency (SC):** Requires that the result of any execution be the same as if all operations were executed in some sequential order, and the operations of each individual processor appear in program order. This model simplifies programming but can enforce long stalls by requiring a processor to wait for remote updates (like invalidations) to complete.
    *   **Relaxed Consistency Models:** Allow reads and writes to complete out of program order to improve performance. They rely on explicit **synchronization operations** (e.g., locks, acquires, releases) to enforce necessary ordering. A synchronized program using a relaxed model behaves as if the system were sequentially consistent. **Release Consistency** is the least restrictive model and is often found in modern architectures (e.g., Armv8, C/C++ specifications).

### D. Multithreading to Exploit TLP

**Multithreading (MT)** is a technique where multiple threads share the functional units of a single processor core, hiding latencies (especially long cache misses) without duplicating the entire processor core.

1.  **Fine-Grained Multithreading:** Switches between threads on every clock cycle, interleaving execution. This hides both short and long stalls but slows down the execution of any single thread.

2.  **Coarse-Grained Multithreading:** Switches threads only on costly stalls (e.g., L2 or L3 cache misses) to avoid slowing down single-thread performance. This is less effective at hiding short stalls due to pipeline start-up costs.

3.  **Simultaneous Multithreading (SMT):** A variation of fine-grained multithreading implemented on multiple-issue, dynamically scheduled processors (superscalars).
    *   **Mechanism:** It leverages dynamic scheduling hardware and register renaming to allow instructions from multiple independent threads to be issued and executed *simultaneously* in the same clock cycle, thereby increasing functional unit utilization.
    *   **Performance:** SMT combined with multicore provides significant performance gains (e.g., for certain workloads, speedup efficiency on four cores increased from $76\%$ (without SMT) to $97\%$ (with SMT)).

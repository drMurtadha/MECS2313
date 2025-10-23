## Topic 2 :Memory Hierarchy & Optimization

### I. Fundamentals and Structure of the Memory Hierarchy

The memory hierarchy provides an economical solution to the goal of having an indefinitely large amount of fast memory. This solution relies heavily on the **principle of locality** (temporal and spatial).

1.  **Structure and Organization:**
    *   The hierarchy is organized into levels, where each level is **smaller, faster, and more expensive per byte** than the level below it.
    *   The aim is to offer memory performance approaching the fastest level and cost approaching the cheapest level.
    *   The **inclusion property** states that data contained in a lower level (farther from the processor) are typically a superset of the next higher level; this is always required for the lowest level (main memory or secondary storage).
    *   Levels in a typical server memory hierarchy range drastically in size and speed  
  (time units shift from picoseconds to milliseconds):

  - Registers (300 ps, 4000 bytes)  
    → L1 Cache (1 ns, 64 KiB)  
    → L2 Cache (3–10 ns, 256 KiB)  
    → L3 Cache (10–20 ns, 8–32 MiB)  
    → Memory (50–100 ns, 8–64 GiB)  
    → Flash Storage (100–200 µs, 1–16 TiB)  
    → Disk Storage (5–10 ms, 16–64 TiB)
2.  **Key Terms and Concepts:**
    *   A **cache hit** occurs when requested data is found in the cache.
    *   A **cache miss** occurs when data is not found; a fixed-size **block** (or line) is retrieved from the lower level.
    *   **Virtual memory** uses pages (fixed-size blocks) to divide physical memory, treating it as a cache of secondary storage (disk or Flash).
    *   The **Translation Lookaside Buffer (TLB)** acts as a cache for address translations (page table).

3.  **Modern Design Considerations:**
    *   Traditionally, optimization focused on **average memory access time**.
    *   More recently, **power consumption** has become a major factor. In Personal Mobile Devices (PMDs), caches can consume 25% to 50% of the total power budget.

### II. Quantitative Metrics and Performance Formulas

Performance is evaluated using quantitative metrics, often based on extending the CPU execution time equation to include memory stalls.

1. **Average Memory Access Time (AMAT):**

$$
\text{AMAT} = \text{Hit time} + (\text{Miss rate} \times \text{Miss penalty})
$$

2.  **Performance Metrics:**
    *   **Memory Stall Cycles:** The number of cycles the processor is stalled waiting for memory access.
        $$ Memory\: stall\: cycles = Number\: of\: misses \times Miss\: penalty $$
        Or, relative to instruction count (IC):
        $$ Memory\: stall\: cycles = IC \times \frac{Memory\: accesses}{Instruction} \times Miss\: rate \times Miss\: penalty $$
    *   **Misses per instruction (MPI):**
        $$ Misses\: per\: instruction = Miss\: rate \times Memory\: access\: per\: instruction $$
    *   **CPU Execution Time:**
        $$ CPU\: execution\: time = (CPU\: clock\: cycles + Memory\: stall\: cycles) \times Clock\: cycle\: time $$

3.  **Cache Organization Metrics:**
    *   **Cache Index Size:**
        $$ 2^{index} = \frac{Cache\: size}{Block\: size \times Set\: associativity} $$
    *   **The Three C's Model of Misses:** Classifies misses to guide optimization:
        *   **Compulsory (Cold-start):** First access to a block.
        *   **Capacity:** Occurs because the cache cannot hold all needed blocks during program execution.
        *   **Conflict:** Occurs in set-associative or direct-mapped caches when multiple blocks map to the same set.

### III. Memory Technologies

The hierarchy employs different technologies based on speed, cost, and capacity goals.

1.  **DRAM (Dynamic Random-Access Memory):** Used for main memory.
    *   **Access Time** is the time until the desired word arrives.
    *   **Cycle Time** is the minimum time between unrelated requests.
    *   Uses address multiplexing (Row Access Strobe, RAS, followed by Column Access Strobe, CAS).
    *   High bandwidth is achieved through organizing DRAM chips into **multiple memory banks** and using a wider memory bus.

2.  **Flash Memory (NAND):** Nonvolatile EEPROM, meaning it holds content without power. Used in Solid-State Drives (SSDs) for secondary storage in PMDs, laptops, and servers due to performance, power, and density advantages over magnetic disks.

3.  **HBM (High Bandwidth Memory) / Stacked DRAMs:** A packaging innovation (2.5D or 3D stacking) that places multiple DRAMs in the same package as the processor. This lowers access latency and increases bandwidth. HBM is often proposed for use as large L4 caches.

### IV. Basic Cache Optimizations (Six Techniques)

These techniques focus on reducing miss rate, miss penalty, and hit time.

| Category | Optimization | Effect/Details |
| :--- | :--- | :--- |
| **Reduce Miss Rate** | **1. Larger Block Size** | Reduces compulsory misses (exploits spatial locality) but increases miss penalty. |
| | **2. Larger Cache Size** | Reduces capacity misses; drawback is increased hit time, cost, and power. |
| | **3. Higher Associativity** | Reduces conflict misses. **2:1 Cache Rule:** A direct-mapped cache of size $N$ has approximately the same miss rate as a two-way set-associative cache of size $N/2$. Drawback: higher associativity can increase hit time. |
| **Reduce Miss Penalty** | **4. Multilevel Caches (L1, L2, L3)** | Allows L1 to be small and fast (reducing hit time) while L2/L3 are large (reducing miss rate to main memory). |
| | **5. Read Priority Over Writes** | Uses a write buffer and prioritizes read misses (which stall the processor) over pending writes (which are often hidden in the buffer). |
| **Reduce Hit Time** | **6. Avoiding Address Translation** | Uses the page offset (virtual index/physical tag method) to index the cache, removing the Translation Lookaside Buffer (TLB) access from the critical path. |

### V. Advanced Cache Optimizations (Ten Techniques)

These techniques are organized by the cache metric they primarily target:

1.  **Reducing Hit Time:**
    *   **Small and Simple First-Level Caches:** Minimizes hit time and power consumption. Lower associativity (e.g., direct-mapped) often yields faster access.
    *   **Way Prediction:** Reduces hit time by predicting which way (block) of a set-associative cache is likely to be accessed, saving energy.

2.  **Increasing Cache Bandwidth:**
    *   **Pipelined Caches:** Allows high clock rates by overlapping cache access stages.
    *   **Multibanked Caches:** Divides the cache into independent banks to support simultaneous parallel accesses.
    *   **Nonblocking Caches:** Allows the processor to continue issuing instructions and handling hits even while a previous cache miss is outstanding, hiding memory latency.

3.  **Reducing Miss Penalty:**
    *   **Critical Word First and Early Restart:** The missing word is requested first from memory and sent immediately to the processor upon arrival, allowing execution to resume before the entire block loads.
    *   **Merging Write Buffer:** Combines multiple writes to the same address into a single buffer entry, reducing memory traffic and miss penalty, used commonly with write-through caches.

4.  **Reducing Miss Rate:**
    *   **Compiler Optimizations:** Software techniques to improve locality, such as **Loop Interchange** (improving spatial locality by reordering nested loops) and **Blocking** (improving temporal locality by operating on small data subsets, crucial for matrix operations).

5.  **Reducing Miss Penalty or Miss Rate via Parallelism:**
    *   **Hardware Prefetching:** Hardware automatically fetches data (often the next sequential line) into the cache based on predictions. Can increase power consumption if prefetched data is unused.
    *   **Compiler-Controlled Prefetching:** The compiler inserts specific prefetch instructions (register prefetch or cache prefetch) into the code to request data before it is needed.

6.  **Using HBM to Extend the Memory Hierarchy (L4 Cache):** In modern architectures, in-package DRAM (HBM) can be used to create massive L4 caches (128 MiB to 1 GiB or more). Techniques like **Alloy Cache** have been proposed to store tags and data together in the HBM row buffer, significantly reducing L4 hit latency.

### VI. Memory Hierarchy in Modern Architectures

The memory hierarchy faces challenges from the **slowing of Moore's Law and the end of Dennard scaling**. This has increased the importance of parallelism and specialization.

1.  **Handling Latency Hiding:**
    *   Processors like the Intel Core i7 (i7) use aggressive techniques like out-of-order pipelines with speculation, hardware prefetching, and multiple outstanding misses to tolerate unpredictable memory latency and hide the effects of cache misses.
    *   The complexity of modern pipelines makes measuring performance challenging, requiring differentiation between *demand accesses* (actual instruction/data access) and *prefetch accesses*.

2.  **Domain-Specific Architectures (DSAs) and Memory:**
    DSAs, like the Google Tensor Processing Unit (TPU) and Pixel Visual Core, bypass traditional CPU/GPU cache hierarchies to achieve high energy efficiency. Key guidelines related to memory include:
    *   **Use dedicated memories:** Minimizes data movement distance. Dedicated memories (like scratchpad memories in DSAs) consume significantly less energy than equivalent multi-way set-associative caches.
    *   **Reduce data size and type:** Using narrower or simpler data types (e.g., 8-bit integers instead of 32-bit floating point) increases effective memory bandwidth and allows more arithmetic units in the same area.
    *   The TPU utilizes a large **Unified Buffer** (24 MiB) and **Accumulators** (4 MiB) optimized for sequential block accesses, contrasting with the redundant, multilevel inclusive caches of general-purpose CPUs.

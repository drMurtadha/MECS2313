
# Topic 5: Warehouse-Scale Computers (WSCs) and Domain-Specific Architectures (DSAs)
## I. Warehouse-Scale Computers (WSCs)

WSCs are massive, highly coupled systems designed for Internet services, embodying the philosophy that **"The datacenter is the computer"**.

### A. Scale, Motivation, and Requirements

1.  **Scale and Scope:** WSCs are the foundation of Internet services such as search, social networking, and online shopping. They contain 50,000–100,000 servers and cost hundreds of millions of dollars, representing the modern descendant of the supercomputer.
2.  **Shared Requirements (with traditional servers):**
    *   **Cost-Performance:** Work done per dollar is critical due to the scale; a $40 million annual computer cost means a 10% improvement saves $4 million annually per WSC.
    *   **Energy Efficiency:** Work done per joule is vital because the majority of infrastructure costs are for power and cooling.
    *   **Dependability:** WSCs must provide at least **99.99% availability** (less than 1 hour downtime per year). This is achieved by relying on **redundant, inexpensive components** managed by software, rather than highly expensive, reliable hardware.
3.  **Unique WSC Characteristics:**
    *   **Parallelism:** WSCs rely on **Request-Level Parallelism (RLP)** (e.g., millions of independent user requests) and **Data-Level Parallelism (DLP)** (e.g., billions of webpages).
    *   **Operational Costs (OPEX):** These costs (energy, power distribution, cooling) represent over 30% of the total cost over 10 years, contrasting with server architecture where operational costs are often ignored.
    *   **Utilization and Efficiency:** WSC servers typically operate between **10% and 50% utilization**. Therefore, it is critical for servers to compute efficiently at low utilization levels.
    *   **Failure Management:** Failures are commonplace (e.g., expected disk failures can be one per hour in a 100,000-server facility). Software systems must be resilient to cope with this reality and deliver high availability.

### B. Architecture and Performance

1.  **Programming Models:**
    *   **MapReduce** (and its twin Hadoop) is a popular framework for batch processing (e.g., creating search indexes). It uses a programmer-supplied **Map** function (applied to input) and a **Reduce** function (collects and collapses the output).
    *   WSC systems must cope with **tail latency** (variability in performance from slow tasks) by starting backup executions of tasks and taking the result from whichever finishes first.
    *   WSC storage software often uses **relaxed consistency models** (like eventual consistency) rather than strictly following ACID requirements, enabling easier scaling.
2.  **Memory Hierarchy and Locality:**
    *   The WSC memory hierarchy includes local server DRAM, Flash, and Disk, extending to rack-level and array-level resources connected by networks.
    *   Network overhead drastically impacts latency and bandwidth. Local server DRAM access (0.1 µs) is much faster than rack DRAM (300 µs).
    *   **Locality is vital** for performance; if 9% of accesses are within the rack and 1% are within the array, the average memory access time can be hundreds of times slower than if all accesses were local.
3.  **Networking:** WSCs use a **hierarchy of networks**. Custom switches built from commodity chips are employed to scale bandwidth, often using centralized control rather than traditional decentralized routing protocols. Google’s Jupiter switch uses a **Clos topology**, offering fault tolerance and high bisection bandwidth.

### C. Cost and Cloud Computing

1. **Efficiency Metric (PUE):**  
The efficiency of a **Warehouse-Scale Computer (WSC)** is measured using the Power Usage Effectiveness (PUE), defined as:

$$
\text{PUE} = \frac{\text{Total Facility Power}}{\text{IT Equipment Power}}
$$

The **median PUE** for data centers is **1.69**, meaning that **about 70% of the power** is overhead (cooling, lighting, etc.).  
In contrast, **Google's data center fleet** achieved an average **PUE of 1.12 by 2017**, reflecting high efficiency.
2.  **Cloud Computing (Utility Computing):** WSCs enable **cloud computing**, realizing the goal of computing as a utility.
    *   Cloud services like Amazon EC2 (Elastic Computer Cloud) leverage massive economies of scale (e.g., 5–7 times reduction in storage and networking costs compared to smaller data centers).
    *   VMs (Virtual Machines) are used to offer isolation between customers, provide multiple price points by controlling physical processor access, and hide hardware identity.
    *   Cloud computing offers **cost associativity** (1000 servers for 1 hour costs the same as 1 server for 1000 hours) and the illusion of infinite scalability.

## II. Domain-Specific Architectures (DSAs)

DSAs are customized processors designed to accelerate a narrow range of tasks extremely well, resulting from the inability of general-purpose architectures to sustain historical performance growth.

### A. Motivation and Principles

1.  **The End of Scaling:** The design trend toward DSAs is necessitated by the **end of Dennard scaling** and the **recent slowdown of Moore’s Law**. DSAs represent the only remaining path to improve energy-performance-cost.
2.  **DSA Advantages:** Customized DSAs can offer higher performance, lower power, and less silicon area than general-purpose processors. They can provide performance and power benefits equivalent to three or more historical generations of Moore’s Law and Dennard scaling.
3.  **Five Design Guidelines for DSAs:**
    1.  **Dedicated Memories:** Use software-controlled memories (scratchpads) to minimize the distance data must travel, saving area and energy (e.g., a two-way set-associative cache uses 2.5 times the energy of a scratchpad).
    2.  **Larger Arithmetic Units:** Invest resources saved from dropping complex microarchitectural optimizations (like out-of-order execution, speculation, or redundant caches) into more arithmetic units or larger memories.
    3.  **Easiest Parallelism:** Exploit the natural granularity of parallelism in the domain using simpler mechanisms like SIMD or VLIW, which are easier for programmers and more energy-efficient than out-of-order execution.
    4.  **Smaller Data Size:** Reduce data size (e.g., using 8-bit integers instead of 32-bit floating point) to increase effective memory bandwidth and pack more arithmetic units.
    5.  **Domain-Specific Language (DSL):** Use tailored languages like TensorFlow (for DNNs) or Halide (for image processing) to port code efficiently.

### B. Example Domain: Deep Neural Networks (DNNs)

DNNs are highly computation-intensive algorithms driving the DSA trend, applicable to speech, vision, translation, and search ranking.

*   DNN architectures require intensive matrix-oriented operations, including **vector-matrix multiply, matrix-matrix multiply, and stencil computations**.
*   An optimization in DNNs is processing **batches** of inputs to reuse weights fetched from memory, increasing operational intensity.

### C. DSA Implementations (Case Studies)

| DSA | Design Target/Hardware | Key Architectural Features | Cost-Performance Insight |
| :--- | :--- | :--- | :--- |
| **Google TPU** | Inference Accelerator (ASIC) in data center | Uses 8-bit integers; large **Matrix Multiply Unit (MMU)** with **systolic data flow**; relies on dedicated memory (24 MiB Unified Buffer, 4 MiB Accumulators). | Achieved **34 times better total-performance/watt** and **83 times better incremental-performance/watt** compared to a Haswell CPU and K80 GPU, respectively, for DNN inference workload. |
| **Microsoft Catapult** | Flexible Data Center Accelerator (FPGA) | Reconfigurable to support various applications (e.g., search ranking, CNNs); uses a **"bump-in-the-wire"** configuration to accelerate network infrastructure. Utilizes multiple forms of parallelism (2D SIMD for CNN, MISD for scoring). | Provides performance gains over CPUs (e.g., 1.5–1.75 gain in performance/TCO for ranking). Lower NRE than ASICs. |
| **Pixel Visual Core** | Image Processing Unit (ASIC IP block) for PMDs | Optimized for image processing; uses two-dimensional parallelism; leverages **VLIW, SIMD, and MPMD** parallelism; programmed via Halide or TensorFlow. | Designed to be integrated into a system-on-a-chip (SOC), addressing energy constraints of mobile devices. |

## III. DSA Convergence and Future

The end of the Moore's Law era, combined with WSC innovations, means sustained performance improvement must come from specialization.

*   **Heterogeneity:** Future computers will be heterogeneous, consisting of standard processors for conventional programs and domain-specific processors for narrow tasks.
*   **Historical Recycling:** Ideas that failed for general-purpose computing (like systolic arrays, decoupled-access/execute, and CISC instructions) are proving effective for DSAs.
*   **The Renaissance:** The confluence of the end of Dennard scaling, productivity advances in hardware development, reduced hardware costs due to open instruction sets (like RISC-V), and the massive upside of DSAs is predicted to lead to a renaissance in computer architecture.

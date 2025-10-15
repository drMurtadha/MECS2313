# Chapter 1: Fundamental of Quantitative Design and Analysis

## 1. Classes of Computers

Computers are categorized based on their use, performance, and cost:

- **Personal Computers (PCs):** General-purpose, cost-effective machines for individual users.
- **Servers:** High-capacity, reliable systems for multi-user environments. Prioritize throughput, scalability, and availability.
- **Supercomputers:** Extremely high-performance systems for scientific and engineering problems, often with thousands of processors.
- **Embedded Computers / Mobile Devices:** Purpose-built systems embedded in other devices (e.g., phones, cars), optimized for efficiency, size, and real-time tasks.

---

## 2. Defining Computer Architecture

Two key components define computer architecture:

- **Instruction Set Architecture (ISA):** Interface between hardware and software. Defines processor commands (e.g., x86, ARM, RISC-V).
- **Computer Organization (Microarchitecture):** Implementation of the ISA (e.g., pipelines, caches, execution units). ISA may be shared across different microarchitectures.

---

## 3. Trends in Technology

Technological growth has driven architectural innovation:

- **Moore’s Law:** Transistor count doubles approximately every 2 years.
- **Post-Moore Era Shifts:**
  - **Multicore architectures** to improve performance.
  - **Domain-Specific Architectures (DSAs)** like TPUs and GPUs for specific workloads.
  - **Energy efficiency** prioritized due to diminishing returns from frequency scaling.

---

## 4. Trends in Power and Energy in Integrated Circuits

Power is a central design concern:

- **Dynamic Power:**
  - Depends on switching activity.
  - Formula: `Power ∝ Capacitance × Voltage² × Frequency`.
- **Static Power:**
  - Caused by leakage current in idle transistors.
- **The Power Wall:**
  - Thermal limitations restrict frequency scaling.
  - Shift toward parallelism and energy-efficient computing.

---

## 5. Trends in Cost

Cost-efficiency drives architecture decisions:

- **Cost per Die:**  
  `Cost per die = Cost per wafer / (Dies per wafer × Yield)`
- **Key Factors Influencing Cost:**
  - **Volume:** Higher volumes reduce unit cost.
  - **Yield:** Proportion of working chips per wafer.
  - **Die Size:** Larger dies reduce yield and increase cost.
  - **Process Node:** Advanced technologies offer better performance but are costlier.

---

## 6. Dependability

Ensuring system reliability is crucial, especially for servers and embedded systems:

- **Definitions:**
  - **Fault:** A defect in hardware/software.
  - **Error:** A fault resulting in an incorrect state.
  - **Failure:** The system deviates from expected service.
- **Metrics:**
  - **Reliability:** Likelihood of error-free operation over time (MTTF).
  - **Availability:**  
    `Availability = MTTF / (MTTF + MTTR)`  
    Measures system uptime.
- **Improvement Techniques:**
  - Redundancy (components, data)
  - Error detection and correction
  - Modular and maintainable design

---

## 🔍 Summary

This chapter lays the foundation for evaluating computer systems using a **quantitative approach**. It addresses:

- Classes of computing devices
- Architectural abstractions and organization
- The influence of technology, power, and cost trends
- The importance of system dependability

Understanding these fundamentals helps in designing and analyzing **high-performance, cost-effective, and reliable systems** in modern computing environments.

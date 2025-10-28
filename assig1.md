# 📝 Assignment 1: Quantitative Design and RISC-V ISA Analysis

## 📌 Title:
**Analyzing Quantitative Design Principles and RISC-V ISA for High-Performance Architecture**

---

## 🎯 Objective

This assignment evaluates your understanding of:

- Fundamental **quantitative principles** of computer design.
- Key **performance metrics** including Amdahl’s Law and CPU performance equation.
- **Instruction Set Architecture (ISA)** features and trade-offs, focusing on **RISC-V**.
- How these principles affect real-world performance and energy trade-offs.

This supports the following Course Learning Outcomes (CLO):

- **CLO1**: Differentiate the organizational paradigms that determine the capabilities and performance of computer systems.
- **CLO2**: Analyze the interactions between the computer’s architecture and its software.

---

## 🧾 Instructions

Prepare a **short technical report** (**maximum 6 pages**, excluding references), in your own words, structured with the following sections:

### 1. **Introduction** *(~½ page)*  
Briefly explain the relevance of quantitative analysis in modern computer architecture and the need for updated ISAs like RISC-V.

### 2. **Quantitative Design Principles in Practice** *(~1.5 pages)*  
Discuss:
- Amdahl’s Law and its impact on system speedup.
- CPU Performance Equation:  
  `CPU Time = IC × CPI × Clock Cycle Time`  
  Explain how each factor can be optimized.
- Use an application example (e.g., matrix multiplication, compression) to demonstrate how performance metrics shift when optimizing one factor.

### 3. **ISA Trade-offs: Fixed vs Variable Length** *(~1 page)*  
- Compare **RISC-V’s fixed-length** encoding with **x86’s variable-length**.
- Explain the impact on decoding complexity, pipelining efficiency, and performance.
- Discuss the relevance of compressed ISAs for embedded systems.

### 4. **RISC-V Feature Exploration** *(~1.5 pages)*  
- Describe RISC-V’s:
  - Register structure
  - Instruction formats (R, I, S, B, U, J)
  - Modular extensions (RV64G = RV64IMAFD + optional C)
- Discuss how these features contribute to performance, energy efficiency, and compiler simplicity.

### 5. **Energy vs Performance Trade-off** *(~1 page)*  
- Explain the **Watt-Year Rule** and contrast energy costs (e.g., DRAM access vs integer addition).
- Discuss real-world design trade-offs between energy efficiency and raw performance.

### 6. **Conclusion** *(~½ page)*  
Summarize the architectural trade-offs (cost vs performance vs power) and how RISC-V aligns with modern hardware demands.

---

## 📤 Deliverables

- Submit a **PDF report** to the course portal before the deadline.
- **Filename format**: `MECS2313_Assignment1_YourName.pdf`
- Include **citations** in **IEEE or APA style** (minimum 3 references from textbook, lecture, and other scholarly sources).
- Original work only. **Plagiarism = automatic zero** and possible disciplinary action.

---

## ⏰ Deadline

**[6 November 2025, 11:59 PM (GMT+8)]**

---

## ✅ Marking Rubric (Total: 100%)

| Section | Description | Weight |
|--------|-------------|--------|
| **1. Introduction** | Clear explanation of relevance and context | 10% |
| **2. Quantitative Design Principles** | Correct and deep application of Amdahl’s Law, CPU Performance Equation, with examples | 20% |
| **3. ISA Trade-offs** | Technical comparison, clarity, implications on performance and pipelining | 15% |
| **4. RISC-V Features** | Detailed and accurate explanation of ISA components and extensions | 20% |
| **5. Energy vs Performance Trade-off** | Insightful discussion with technical and real-world relevance | 15% |
| **6. Conclusion & Overall Structure** | Clarity, coherence, formatting, references, academic tone | 10% |
| **Language & Academic Integrity** | Grammar, citation format, originality | 10% |
| **Total** |  | **100%** |

---

### 🔒 Academic Honesty Reminder

> All submissions must be your own original work. Proper referencing is mandatory. Any instance of plagiarism will result in a **zero grade** and may invoke further disciplinary action per university regulations.

---

### 📚 Reference Materials

- Hennessy & Patterson, *Computer Architecture: A Quantitative Approach*, 6th Edition.
- Lecture 1 Notes: Quantitative Design & Performance + ISA Review.
- Additional scholarly articles or online resources (if used, must be cited).

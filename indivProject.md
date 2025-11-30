### Individual Project: Warehouse-Scale Computing Simulation and Analysis

**Advanced Computer System and Architecture**  
**MECS2313**  

**Individual Project (Project-Based Learning – CLO3 and CLO4)**  

**Topic:** Simulating Performance and Power Efficiency in a Warehouse-Scale Datacenter  

**🎯 Objective**  
This individual project evaluates your ability to apply advanced design features on modern processors and characterize real-world computing challenges, fulfilling:  
- CLO3: Adapt advanced design features to optimize data-level parallelism architectures for domain-specific applications using self-paced virtual labs and group projects via LMS, employing integrated problem-solving ESD competency to incorporate flexibility and sustainability in computing solutions.  
- CLO4: Characterize collaborative evaluations of real-world computing challenges in warehouse-scale systems through self-paced modules and peer reviews in LMS, fostering collaboration ESD competency to promote ethical and adaptable practices in advanced computer systems.  

The project focuses on simulating a warehouse-scale computer (WSC) datacenter case study, analyzing performance metrics, power efficiency (e.g., PUE - Power Usage Effectiveness), and sustainability implications. Reference Chapter 6: Warehouse-Scale Computers from *Computer Architecture: A Quantitative Approach* (6th Edition) by Hennessy & Patterson, and Weeks 11-12 syllabus content on WSCs.  

**Case Study: Datacenter for Web Search Workload**  
Simulate a simplified datacenter handling a web search workload (e.g., processing queries with data-level parallelism in vector/SIMD units). Analyze how architectural adaptations (e.g., multi-core scaling, memory hierarchy optimizations) impact performance, energy consumption, and sustainability (e.g., reducing carbon footprint through efficient designs). Use quantitative metrics like execution time, CPI, power draw, and PUE to evaluate trade-offs.  

**🧾 Instructions**  
Prepare a technical report (8-10 pages, excluding references and appendices) in your own words, structured as follows:  
1. **Introduction (~1 page)**  
   - Describe the relevance of WSCs in modern computing (e.g., cloud services like Google Search).  
   - Explain the case study: Simulate a datacenter with 4-8 nodes processing web search queries.  
   - Link to CLO3 (adaptation of designs for optimization) and CLO4 (characterization of challenges like scalability and power).  

2. **Simulator Setup and Configuration (~2 pages)**  
   - Choose one simulator from the options below and justify your choice.  
   - Detail your simulation setup: e.g., multi-core processors (RV64G RISC-V or x86), memory hierarchy (L1/L2 caches, DRAM), workload (e.g., SPEC CPU or Parsec benchmarks mimicking web search).  
   - Incorporate ESD: Discuss how optimizations promote sustainability (e.g., lower power for green computing).  

3. **Simulation Execution and Results (~3 pages)**  
   - Run simulations varying parameters (e.g., core count, cache size, clock frequency).  
   - Collect metrics: Execution time, CPI, cache hit/miss rates, power consumption, PUE.  
   - Include screenshots as specified below.  
   - Adapt designs (CLO3): Optimize for data-level parallelism (e.g., add SIMD extensions).  
   - Characterize challenges (CLO4): Evaluate scalability issues and ethical aspects (e.g., energy waste in overprovisioning).  

4. **Analysis and Discussion (~2 pages)**  
   - Analyze results: How do adaptations improve performance/sustainability?  
   - Discuss trade-offs: Performance vs. power, flexibility in datacenter designs.  
   - Suggest improvements: e.g., domain-specific accelerators for AI-driven searches.  

5. **Conclusion (~1 page)**  
   - Summarize findings and alignment with CLO3/CLO4.  
   - Reflect on real-world implications (e.g., adaptable, ethical datacenter practices).  

**Simulator Options**  
Choose one of the following free, open-source simulators suitable for WSC modeling. All run on standard laptops (Linux/Windows/Mac). Focus on multi-node setups for datacenter simulation.  

1. **gem5 (Recommended for Detailed Architecture Simulation)**  
   - **Why?** Full-system simulator supporting RISC-V/x86, multi-core, memory hierarchies, and power modeling. Ideal for CLO3 adaptations.  
   - **Installation Instructions:**  
     - Download from https://www.gem5.org/download/ (stable version).  
     - Prerequisites: Install Python 3, SCons, GCC (via apt/brew).  
     - Command: `git clone https://gem5.googlesource.com/public/gem5 && cd gem5 && scons build/X86/gem5.opt -j$(nproc)`.  
     - Test: Run `./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello`.  

2. **SST (Structural Simulation Toolkit - Good for Large-Scale Datacenter)**  
   - **Why?** Modular for simulating datacenters with networks, power, and multi-node parallelism. Supports CLO4 characterization of WSC challenges.  
   - **Installation Instructions:**  
     - Download from https://sst-simulator.org/SSTPages/SSTMainRelease1301/.  
     - Prerequisites: Boost, MPI, Python.  
     - Command: `./configure --prefix=/usr/local && make && sudo make install`.  
     - Test: Run a sample config: `sst examples/sst-elements/simpleElementExample/simpleSimulation.py`.  

3. **ZSim (Fast for x86-Based Simulations)**  
   - **Why?** High-speed simulation for multi-core systems; integrate with McPAT for power. Focuses on performance metrics for CLO3/CLO4.  
   - **Installation Instructions:**  
     - Download from https://github.com/s5z/zsim.  
     - Prerequisites: Pin (Intel tool), GCC.  
     - Command: `make -j`.  
     - Test: `./zsim tests/simple.cfg`.  

**Thorough Instructions on Using the Simulator**  
1. **Setup Workload:** Use a benchmark like Parsec (websearch) or SPEC CPU2017 (download from spec.org). Configure for web search: e.g., in gem5, use `configs/example/fs.py` with multi-core and add SIMD via extensions.  
2. **Run Simulation:**  
   - For gem5: `./build/RISCV/gem5.opt configs/learning_gem5/part1/simple.py --cpu-type=DerivO3CPU --caches --l2cache`. Vary params like `--num-cpus=8` for datacenter scaling.  
   - For SST: Create a Python config file defining nodes, memory, and network (e.g., Merlin for interconnect). Run `sst myconfig.py`.  
   - For ZSim: Edit cfg file for cores/caches, run `./zsim mycfg.cfg`.  
3. **Collect Metrics:** Enable stats output (e.g., gem5's m5out/stats.txt for CPI, power). Simulate baseline vs. optimized (e.g., add vector units).  
4. **Screenshots Needed (Embed in Report with Captions):**  
   - Configuration screen/file: Screenshot of your config script/file showing parameters (e.g., core count, cache sizes).  
   - Simulation run command/output: Terminal screenshot of command execution and initial logs.  
   - Performance metrics: Screenshot of stats dump (e.g., CPI, execution time, cache hits).  
   - Power/Energy graphs: If available (e.g., via McPAT integration), screenshot of power trace or PUE calculation.  
   - Visualization: Screenshot of any graphical output (e.g., gem5's timeline or SST's network graph). Minimum 5 screenshots.  

**📤 Deliverables**  
- Submit a PDF report to the LMS portal.  
- Filename: MECS2313_IndividualProject_YourName.pdf  
- Include citations (APA/IEEE, minimum 5, including textbook and simulator docs).  
- Appendices: Simulator config files, full stats outputs.  
- Original work only; plagiarism results in zero and disciplinary action.  

**⏰ Duration and Deadline**  
- Start: December 1, 2025  
- Duration: 3 weeks  
- Deadline: December 22, 2025, 11:59 PM (GMT+8)  

**✅ Marking Rubric (Total: 100%)**  

| Criteria | Weight | Description |
|----------|--------|-------------|
| 1. Introduction and Case Study Relevance (CLO3, CLO4) | 15% | Clear context, justification of simulator choice, and linkage to WSC/datacenter challenges with ESD emphasis. |
| 2. Simulator Setup and Configuration | 20% | Detailed, accurate setup; justification of parameters; step-by-step adherence to instructions. |
| 3. Simulation Execution and Results | 25% | Comprehensive runs (baseline vs. optimized); required screenshots with captions; metrics collection (performance, power, PUE). |
| 4. Analysis and Discussion (CLO3 Adaptation) | 20% | Insightful adaptation of designs (e.g., parallelism optimizations); trade-offs; sustainability reflections. |
| 5. Conclusion and Characterization (CLO4) | 10% | Strong summary of challenges; ethical/adaptable practices; real-world implications. |
| 6. Report Quality, Citations, and Originality | 10% | Structure, grammar, visuals, proper citations; no plagiarism. |
| **Total** | **100%** | |

**🔒 Academic Honesty Reminder**  
All work must be individual and original. Proper referencing is mandatory. Any plagiarism will result in a zero grade and may invoke university disciplinary action.  

**📚 Reference Materials**  
- Hennessy & Patterson, *Computer Architecture: A Quantitative Approach*, 6th Edition (Chapter 6).  
- Simulator documentation (e.g., gem5.org/documentation).  
- Additional articles (e.g., Barroso et al., "The Datacenter as a Computer").  

This project aligns with CLO3 by requiring hands-on adaptation via simulation and CLO4 through characterization of WSC evaluations, promoting integrated problem-solving and collaboration competencies in an ODL context.

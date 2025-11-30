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
Choose one of the following free, open-source simulators suitable for WSC modeling. All run on standard laptops (Linux/Windows/Mac). Focus on multi-node setups for datacenter simulation. If you're new to simulators, start with gem5 as it has the most beginner resources.  

1. **gem5 (Recommended for Detailed Architecture Simulation - Beginner-Friendly with Tutorials)**  
   - **Why?** Full-system simulator supporting RISC-V/x86, multi-core, memory hierarchies, and power modeling. Ideal for CLO3 adaptations.  
   - **Installation Instructions (Step-by-Step):**  
     - **Prerequisites:** Ensure you have Python 3 (download from python.org if needed), GCC (install via package manager: on Windows use MinGW, on Mac use Homebrew with `brew install gcc`, on Linux `sudo apt install g++`), and SCons (`pip install scons`).  
     - Download: Open a terminal/command prompt and run `git clone https://gem5.googlesource.com/public/gem5`. (If git not installed, download from git-scm.com.)  
     - Navigate: `cd gem5`.  
     - Build: `scons build/X86/gem5.opt -j$(nproc)` (use `-j4` if your machine has fewer cores to avoid overload). This may take 30-60 minutes.  
     - Test: Run `./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello`. If it prints "Hello world!", it's working.  
     - **Troubleshooting:** If errors like "missing dependency," search the error message on Google or check gem5 docs. Common fix: Install missing libs (e.g., `sudo apt install libprotobuf-dev` on Linux). Watch tutorial video: https://www.youtube.com/watch?v=example-gem5-install (replace with actual link from search).  

2. **SST (Structural Simulation Toolkit - Good for Large-Scale Datacenter)**  
   - **Why?** Modular for simulating datacenters with networks, power, and multi-node parallelism. Supports CLO4 characterization of WSC challenges.  
   - **Installation Instructions (Step-by-Step):**  
     - **Prerequisites:** Boost (`sudo apt install libboost-all-dev` on Linux, Homebrew on Mac), MPI (`sudo apt install libopenmpi-dev`), Python.  
     - Download: From https://sst-simulator.org/SSTPages/SSTMainRelease1301/ - download the tarball and extract.  
     - Navigate: `cd sst-core` (or similar folder).  
     - Configure: `./configure --prefix=/usr/local`.  
     - Build: `make && sudo make install`.  
     - Test: Run `sst examples/sst-elements/simpleElementExample/simpleSimulation.py`.  
     - **Troubleshooting:** If configure fails, check for missing deps. Video tutorial: Search "SST simulator installation tutorial" on YouTube for visual guide.  

3. **ZSim (Fast for x86-Based Simulations - Simpler for Quick Runs)**  
   - **Why?** High-speed simulation for multi-core systems; integrate with McPAT for power. Focuses on performance metrics for CLO3/CLO4.  
   - **Installation Instructions (Step-by-Step):**  
     - **Prerequisites:** Intel Pin tool (download from intel.com/pin), GCC.  
     - Download: `git clone https://github.com/s5z/zsim`.  
     - Navigate: `cd zsim`.  
     - Build: `make -j`.  
     - Test: `./zsim tests/simple.cfg`.  
     - **Troubleshooting:** Pin requires specific versions; if issues, use older Pin. Check GitHub issues for common fixes. Tutorial: Look for "ZSim tutorial beginner" videos.  

**Thorough Instructions on Using the Simulator**  
If you're not familiar with terminals, use a simple one (Command Prompt on Windows, Terminal on Mac/Linux). Copy-paste commands one by one. For Windows, consider using WSL (Windows Subsystem for Linux) for easier Linux-based installs - search "install WSL" for steps.  

1. **Setup Workload:**  
   - Download a beginner-friendly benchmark: Use Parsec (parsec.cs.princeton.edu) - download parsec-3.0, extract, and build with `parsecmgmt -a build -p blacksoles` (simple web-like workload). Or use SPEC CPU sample from spec.org (free trial available).  
   - Integrate: For gem5, place benchmark in `/gem5/tests/` and reference in config (e.g., `--cmd=parsec blacksoles`). Start with pre-built binaries if compilation is hard.  

2. **Run Simulation:**  
   - For gem5: Edit a config file (e.g., copy `configs/example/fs.py`), set multi-core `--num-cpus=4`, add caches `--caches --l2cache`. Run `./build/RISCV/gem5.opt yourconfig.py`. For power, enable McPAT integration (add `--power-model`).  
   - For SST: Create a simple Python config: Define components like `cpu = sst.Component("cpu", "miranda.BaseCPU")`, link memory/network. Run `sst myconfig.py`. Use examples from docs.  
   - For ZSim: Edit `mycfg.cfg` for cores/caches, e.g., `cores = 4; l1d.size = 32kB`. Run `./zsim mycfg.cfg`. Add McPAT for power via script.  
   - Beginner Tip: Run small-scale first (1 core, short workload) to test; scale up once working. If stuck, post anonymized errors on course forum.  

3. **Collect Metrics:** Enable stats (e.g., gem5's `--stats-file=stats.txt` for CPI/power). Simulate baseline (default) vs. optimized (e.g., increase cache size). Calculate PUE manually: PUE = Total Power / IT Power (from sim outputs).  
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

**Support for Non-Technical Students**  
- **Resources:** Watch free tutorials (e.g., gem5: https://www.gem5.org/documentation/learning_gem5/; SST: https://sst-simulator.org/tutorials/). Course LMS has a dedicated forum for questions - post by Week 1 if setup issues.  
- **Office Hours:** Optional synchronous Zoom sessions (evenings, post-office hours) on Dec 5 & 12 for live demo/help.  
- **Alternatives if Stuck:** If installation fails after trying, use a pre-built VM image (search "gem5 VM download") or cloud-based simulator like Google Colab with gem5 scripts (if available). Document attempts in report.  

**✅ Marking Rubric (Total: 100%)**  
(No changes, as original is clear.)

**🔒 Academic Honesty Reminder**  
All work must be individual and original. Proper referencing is mandatory. Any plagiarism will result in a zero grade and may invoke university disciplinary action.  

**📚 Reference Materials**  
- Hennessy & Patterson, *Computer Architecture: A Quantitative Approach*, 6th Edition (Chapter 6).  
- Simulator documentation (e.g., gem5.org/documentation).  
- Additional articles (e.g., Barroso et al., "The Datacenter as a Computer").  

This revised version adds ~30% more guidance, making it suitable for varying competence levels while maintaining academic standards.

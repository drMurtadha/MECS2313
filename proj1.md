# MCSS2313 – Advanced Computer System & Architecture

## Group Assignment (Project-Based Learning – CLO2)

### Topic: Memory Hierarchy Design: Principles, Technologies, Optimizations, and SDRAM Power Reduction Strategies

⸻

1. Group Structure
	•	5 groups
	•	3 members per group
	•	Collaborative work allowed, but each group must produce original analysis.

⸻

2. Assignment Overview

This project evaluates your ability to analyze the interactions between computer architecture and software, fulfilling CLO2 (C3).

All analysis must reference:
	•	Chapter 2: Memory Hierarchy Design of
Computer Architecture: A Quantitative Approach, 6th Edition by Hennessy & Patterson
	•	Week 3–4 syllabus content

You must deliver:
	1.	Technical Report (10–12 pages)
	2.	YouTube Video Presentation (10 minutes)
	3.	Slide Deck
	4.	Submission of YouTube Link inside the report and in the submission form

⸻

3. Assignment Requirements

A. Memory Hierarchy Foundations with Software Behaviour Profiling

Explain memory hierarchy with explicit linkage to software behavior.

Include:
	•	Temporal & spatial locality
	•	How loops, arrays, recursion, and branching affect cache
	•	Interpretation of memory hierarchies (desktop, mobile, server)
	•	A simple profiling example (any programming language)
	•	e.g., array stride experiment, cache locality benchmark

⸻

B. Memory Technology Analysis (SRAM, DRAM, Flash)

Provide a structured comparison focusing on:
	•	SRAM cache behavior
	•	DRAM internal organization
	•	RAS/CAS
	•	banks, row buffer
	•	Flash memory as secondary storage

Explain how software access style affects:
	•	Cache hit time
	•	DRAM row-buffer hits
	•	Flash responsiveness under sequential vs random I/O

⸻

C. Advanced Cache Optimizations

Select at least five optimizations from textbook Section 2.3, such as:
	•	Larger block size
	•	Higher associativity
	•	Multilevel caches (L1/L2/L3)
	•	Read-priority using write buffer
	•	Virtual-index/physical-tag indexing

For each optimization:
	1.	Explain the hardware concept
	2.	Show how software patterns influence effectiveness
	3.	Provide a real example (matrix multiplication, sorting, etc.)

⸻

D. SDRAM Power Consumption Reduction

Analyze SDRAM power using textbook discussions on:
	•	Static power
	•	Dynamic power
	•	Refresh penalties
	•	Bank-level accesses

Explain:
	•	Architectural energy-saving techniques
	•	Software-level behaviour that reduces DRAM energy
	•	batching I/O
	•	improving locality
	•	reducing random accesses

⸻

E. Case Study: Intel Core i7 6700 vs ARM Cortex-A53

Compare both memory architectures:
	•	Cache sizes and levels
	•	Memory bandwidth
	•	DRAM access behaviour
	•	Typical software workloads (mobile app vs desktop compute tasks)

Discuss how different software categories benefit from each architecture.

⸻

F. YouTube Presentation Requirement

Each group must produce a 10-minute YouTube video.

Video Requirements:
	•	All members must appear or speak
	•	Slides must be shown clearly
	•	Use visuals (diagrams, graphs, tables)
	•	Include your profiling experiment results (recommended)
	•	Video can be Public or Unlisted

Submission Requirement:

Place the YouTube link:
	•	On the report cover page
	•	In the submission form
	•	Inside the slide deck

⸻

4. Deliverables Checklist

1. Report (PDF, 10–12 pages)
	•	IEEE formatting
	•	Minimum 10 citations
	•	Redraw all textbook figures (no direct copying)

2. YouTube Video
	•	10 minutes
	•	All members participate
	•	Professional presentation quality

3. Slide Deck
	•	8–10 slides
	•	Clear and concise visuals

4. Final Submission

Submit as one ZIP file:
	•	Report (PDF)
	•	Slides (.pptx or .pdf)
	•	YouTube link included in all documents

⸻

5. Assessment Rubric (100%)

Criteria	Weight	Description
1. Technical Understanding of Memory Hierarchy (CLO2)	20%	Accurate explanation of locality and hierarchy with software interaction emphasis.
2. Analysis of Memory Technologies	15%	Clear comparison of SRAM, DRAM, Flash with correct DRAM internal details.
3. Cache Optimization Analysis	20%	At least 5 optimizations, linked to actual software behaviour.
4. SDRAM Power Analysis	15%	Strong explanation of static/dynamic power and realistic software-driven effects.
5. Case Study (Intel i7 vs ARM A53)	10%	Correct, insightful comparison with software workload relevance.
6. YouTube Video Presentation	10%	Clarity, professionalism, technical depth, group participation.
7. Report Quality & Citations	10%	Structure, grammar, diagrams, IEEE citation style.

Total: 100%

⸻

6. CLO2 Alignment

This assignment directly supports CLO2 by requiring students to:
	•	Analyze how software behaviour affects cache and DRAM performance
	•	Evaluate memory hierarchy design choices using real program patterns
	•	Demonstrate understanding through profiling experiments and case studies

⸻

If you want, I can also prepare:
	•	Student instruction PDF
	•	Editable slide template
	•	Video script template
	•	Group allocation list

Just let me know.

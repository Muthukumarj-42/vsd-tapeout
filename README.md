# 📘 RISC-V Reference SoC Tapeout Program Progress

<h3 align="center" style="color:#FF4500;">
From concepts to Silicon, <span style="color:#1E90FF;">Shaping INDIA's Semiconductor Program 🤞</span>
</h3>  

<p align="center">
  <img width="491" height="279" alt="image" src="https://github.com/user-attachments/assets/8c1416df-2490-43b8-906b-666b3d9aad75" />
</p>  

---

## 📌 Organisers

This program is organised by [**Indian Institute of Technology, Gandhinagar (IITGN)**](https://iitgn.ac.in/) in collaboration with:

* [**VSD (VLSI System Design)**](https://www.vlsisystemdesign.com/)
* [**SCL**](https://www.scl.gov.in/)
* [**Synopsys**](https://www.synopsys.com/)
* [**Platforms**](https://platforms.synopsys.com/)
* [**RISC-V Core Team**](https://riscv.org/)
* [**Efabless**](https://efabless.com/)

using open-source EDA community tools.

---

## 📌 Program Overview

* **Duration**: 20 weeks
* **Objective**: Complete hands-on journey from RTL design to silicon tapeout
* **Tools**: Industry-standard Synopsys EDA tools and SCL180 nm PDK
* **Mission**: Contributing to India’s semiconductor self-reliance and the *One Tapeout Per Student* vision

### Program Outcomes

Participants will:

* Master the complete VLSI design flow from RTL to GDSII
* Gain hands-on experience with industry-grade tools and methodologies
* Develop a fabrication-ready RISC-V System-on-Chip (SoC)
* Acquire skills essential for India’s growing semiconductor industry
* Contribute to the national framework for academic silicon tapeouts

---

## 📌 Weekly Summary Table

| Week       | Contents                                                                                                                                                                                                                                                                                                                       | Duration                | Notes               |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------- | ------------------- |
| **Week 0** | Task 1: Ubuntu Installation with Oracle VirtualBox <br> Task 2: Essential EDA Tools Installation                                                                                                                                                                                                                               | 18/09/2025 - 20/09/2025 | [Week0](week-0) |
| **Week 1** | Day 1 – Introduction to Verilog RTL design and Synthesis <br> Day 2 – Timing libs, hierarchical vs flat synthesis and efficient flop coding styles <br> Day 3 – Combinational and sequential optimizations <br> Day 4 – GLS, blocking vs non-blocking and Synthesis-Simulation mismatch <br> Day 5 – Optimization in synthesis | 21/09/2025 - 27/09/2025 | [Week1](Week-1)      |

---

## 📌 Week 0: Environment Setup

### Task 1: Ubuntu Installation with Oracle VirtualBox

* **Host OS**: Windows 11 Home Single Language (24H2)
* **Virtual Machine**: Oracle VirtualBox 7.0
* **Guest OS**: Ubuntu 22.04.4 LTS (Jammy Jellyfish)
* **Allocated RAM**: 8 GB
* **Storage**: 50 GB dynamic disk
* **Processor**: 4 cores allocated

### Task 2: Essential EDA Tools Installation

| Tool           | Version | Purpose                |
| -------------- | ------- | ---------------------- |
| Yosys          | 0.34+43 | RTL Synthesis          |
| Icarus Verilog | 12.0    | Verilog Simulation     |
| GTKWave        | 3.3.104 | Waveform Visualization |

---
## 🌟 Key Learnings from Week 0

* 🛠️ **Tools Installed & Verified:** Successfully installed **Iverilog**, **Yosys**, and **GTKWave**, and verified their functionality.
* 💻 **Environment Setup:** Learned how to set up the **basic RTL design and synthesis environment** for future experiments.
* 🏗️ **System Prepared:** Configured the system and prepared it for the upcoming **RTL → GDSII flow** experiments.

## 🌟 Key Learnings from Week 1

* 📘 Basics of Verilog RTL coding (combinational & sequential).
* 🕒 Timing libraries, hierarchical vs flat synthesis.
* 🔁 Logic optimizations for area, power, and speed.
* ⚡ Gate-Level Simulation (GLS) and handling mismatches.
* 🛠️ Synthesis optimization techniques.

---

## 🖥️ Device Specifications

### Host Machine

* **Device Name**: MRSPY
* **Processor**: 12th Gen Intel(R) Core(TM) i5-12450HX (2.40 GHz)
* **Installed RAM**: 24 GB (23.7 GB usable)
* **System Type**: 64-bit operating system, x64-based processor
* **OS**: Windows 11 Home Single Language (24H2, Build 26100.6584)

### Virtual Machine

* **Guest OS**: Ubuntu 22.04.4 LTS (Jammy Jellyfish)
* **Kernel**: 5.x (default Jammy generic)
* **Architecture**: x86_64
* **RAM Allocated**: 8 GB
* **Storage**: 50 GB (dynamic)
* **Cores Allocated**: 4

---

## 🙏 Acknowledgment

I am thankful to 🤞<a href="https://github.com/kunalg123" target="_blank"><span style="color:#00008B;"><b>Kunal Ghosh</b></span></a> and Team <a href="https://vsdiat.vlsisystemdesign.com/" target="_blank"><span style="color:#00008B;"><b>VLSI System Design (VSD)</b></span></a> for the opportunity to participate in the ongoing <span style="color:#00008B;"><b>RISC-V SoC Tapeout Program</b></span>.

I also acknowledge the support of <span style="color:#00008B;"><b>RISC-V International</b></span>, <span style="color:#00008B;"><b>India Semiconductor Mission (ISM)</b></span>, <span style="color:#00008B;"><b>VLSI Society of India (VSI)</b></span>, and <a href="https://github.com/efabless" target="_blank"><span style="color:#00008B;"><b>Efabless</b></span></a> for making this initiative possible.

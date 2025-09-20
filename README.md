# vsd-tapeout-w0
The **RISC-V Reference SoC Tapeout Program** aims to train participants in the full digital ASIC flow using open-source EDA tools: from writing RTL, synthesis, placement & routing, through to GDSII generation and tapeout readiness.
Over a 10-week program, you'll build skills and a project portfolio documenting each stage of the flow.
## Tools Installation Status

| Tool            | Purpose                               | Status |
|:---------------:|:-------------------------------------:|:------:|
| **Yosys**       | RTL synthesis & logic optimization    | ✅ Installed |
| **Icarus Verilog** | RTL simulation                     | ✅ Installed |
| **GTKWave**     | Waveform viewing                      | ✅ Installed |

## Installing Yosys
<pre>
  sudo apt-get update 
   git clone https://github.com/YosysHQ/yosys.git 
   cd yosys 
   sudo apt install make (If make is not installed please install it)  
   sudo apt-get install build-essential clang bison flex \ 
     libreadline-dev gawk tcl-dev libffi-dev git \ 
     graphviz xdot pkg-config python3 libboost-system-dev \ 
     libboost-python-dev libboost-filesystem-dev zlib1g-dev 
   make config-gcc 
   make  
   sudo make install
</pre>
---
## Installing Iverilog
<pre>
  sudo apt-get update 
  sudo apt-get install iverilog
</pre>

---
## Installing GTKWave
<pre>
  sudo apt-get update 
  sudo apt-get install gtkwave
</pre>

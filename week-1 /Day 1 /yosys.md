# Yosys – Converting RTL to Netlist

### 1. What is Synthesis?

Synthesis is the process of turning a Verilog RTL description into a **gate-level circuit**.
Here we use **Yosys** as the synthesizer.
![yosys](https://github.com/Muthukumarj-42/vsd-tapeout/blob/7e3b9e0a37550999c90a1cfa88c9fd9f12853d4e/week-1%20/%20pictures/yosys_flow.png)


### 2. Netlist Output

* The result is a **netlist file** – a Verilog description made only of gates, flip-flops, and connections.
* High-level RTL constructs (`always`, `if`, `case`) disappear, leaving only hardware structure.

### 3. Checking the Netlist

* The netlist can be simulated again with **iverilog** to confirm it behaves like the RTL code.
* The **same testbench** that was used for RTL can be reused for this check.

### 4. Key Point

The **inputs and outputs** of the design stay unchanged after synthesis.
That’s why you can run RTL and netlist verification using the same testbench.

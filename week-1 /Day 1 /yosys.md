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
![check](https://github.com/Muthukumarj-42/vsd-tapeout/blob/afe24945b6407691f0075109e25efa6c6bf30d0a/week-1%20/%20pictures/verification_of_synthesis.png)

### 4. Key Point

The **inputs and outputs** of the design stay unchanged after synthesis.
That’s why you can run RTL and netlist verification using the same testbench.

### Explanation of `.lib`

* A **`.lib` file** is a **library of logical modules**.
* It contains standard building blocks like **AND, OR, NOT gates**.
* Each gate can have **multiple versions (flavors)**:

  * 2-input AND gate may come in **slow, medium, fast** versions.
  * The difference between versions is mainly in **speed, power consumption, and area**.
  * Faster versions switch quickly but need more power and take more space.
  * Slower versions save power and area but respond slower.

The **use of `.lib`** in VLSI/ASIC design is:

* It acts as a **standard cell library file**.
* Provides **information about logic gates and cells** (AND, OR, NOT, Flip-Flops, etc.).
* Stores **timing, power, and area details** for each cell.
* Helps the **synthesis tool (like Yosys, Design Compiler, etc.)** choose the right cells during circuit implementation.
* Ensures that the final design meets **speed (timing), power, and area requirements**.


| Type of Cell   | Switching Speed     | Power Usage | Chip Area | Best Use Case                                                |
| -------------- | ------------------- | ----------- | --------- | ------------------------------------------------------------ |
| **Fast Cells** | Very quick          | High        | Bigger    | Used in paths where timing is critical (must meet deadlines) |
| **Slow Cells** | Deliberate (slower) | Low         | Smaller   | Used in non-critical paths (where extra delay is acceptable) |

![lib](https://github.com/Muthukumarj-42/vsd-tapeout/blob/2f2a86daf1bbe4f0f6beed2d24bf6afa3514f5d4/week-1%20/%20pictures/lib.png)

### Yosys lab setup
Move the folder which has teh verilog files
```
cd verilog_files
```
Initiate **Yosys** in the folder which has verilog files
```
yosys
```
In yosys 
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![yosys]()


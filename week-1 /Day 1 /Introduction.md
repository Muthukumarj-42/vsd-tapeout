# Day 1: Getting Started with Icarus Verilog (iverilog) and Yosys - Converting RTL to Netlist

**Goal:** Learn the basics of RTL simulation using Icarus Verilog and understand the importance of testbenches.

## **1. What is a Simulator?**

* A **simulator** is a tool used to **verify digital designs** before hardware implementation.
* In this workflow, we use **Icarus Verilog (`iverilog`)** as the simulator.

## **2. What is a Testbench?**

* A **testbench** is a special Verilog file created to **test the functionality of a design**.
* It applies **input signals (stimuli)** and observes the **output response**.
* **Key Point:**

  * **Design module** → has **primary inputs and outputs**.
  * **Testbench** → has **no primary inputs or outputs**, it only drives and monitors the design.
  ![testbench](https://github.com/Muthukumarj-42/vsd-tapeout/blob/06684ce6edfae7450a923903edf8f8c745cb6aa7/week-1%20/%20pictures/teshbench.png)

## **3. How Simulation Works**

* The simulator **monitors all input signals** given to the design.
* When an input changes → the output is **re-evaluated**.
* If inputs remain stable → the output stays **unchanged**.
![flow](https://github.com/Muthukumarj-42/vsd-tapeout/blob/08369b1ec28a67f7e5f76922f7696de57000c5b7/week-1%20/%20pictures/flow.png)
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

## **Important Notes**
✔ A **design module** can have one or more primary **inputs/outputs**.
✔ A **testbench** itself does **not** have inputs/outputs — it only wraps around the design to test it.

---

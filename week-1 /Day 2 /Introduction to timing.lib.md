# **RISC-V SoC — Day 2: Timing Libraries, Synthesis Strategies & Flip-Flop Design**

This session dives into how to use timing libraries, compare synthesis approaches, and design efficient flip-flops in an SoC flow using the SKY130 PDK.

## 📚 Table of Contents

1. Timing Libraries & PDK Overview
2. Synthesis Strategies: Hierarchical vs Flattening
3. Coding Styles for Flip-Flops
4. Combined Simulation & Synthesis Workflow
5. Practical Optimization Techniques
6. Expanded Concepts & Advanced Topics
7. Summary & Takeaways

## 1. Timing Libraries & PDK Overview

### SKY130 & the Role of `.lib`

* The **SKY130 PDK** is an open-source 130 nm CMOS technology offering standard cell libraries, layout rules, parasitic models, and timing files.
* Among its critical pieces is a **Liberty file** (e.g. `sky130_fd_sc_hd__tt_025C_1v80.lib`) that defines timing, power, and area metadata for all cells.
* The naming convention decodes as:

  * `tt`: typical process (typical NMOS & PMOS behavior)
  * `025C`: 25 °C temperature
  * `1v80`: 1.80 V supply

Liberty files are the bridge between abstract RTL logic and physical cell implementation.

📌 **Loading a .lib file (example)**

```sh
cd ~/projects/sky130_libs
vim sky130_fd_sc_hd__tt_025C_1v80.lib
```

You’ll see sections such as `cell ( DFF_X1 )`, `timing`, `power`, `pin` definitions, etc.

#### PVT Corners & Their Importance

| Corner      | Process Behavior | Voltage | Temp   | Use in Timing Checks             |
| ----------- | ---------------- | ------- | ------ | -------------------------------- |
| **SS**      | Slow NMOS & PMOS | 1.60 V  | 125 °C | Worst-case (setup)               |
| **TT**      | Typical devices  | 1.80 V  | 25 °C  | Nominal/typical design           |
| **FF**      | Fast devices     | 1.95 V  | –40 °C | Best-case (hold)                 |
| **SF / FS** | Mixed corners    | 1.80 V  | 25 °C  | Stress or skewed device behavior |

Why PVT matters:

* Setup time must be met in worst-case conditions (SS corner).
* Hold time must be safe even in best-case conditions (FF corner).
* Power and leakage vary strongly with temperature and voltage.

---

## 2. Synthesis Strategies: Hierarchical vs Flattening

### Example Design for Illustration

```verilog
module sub1(input a, input b, output y);
  assign y = a & b;
endmodule

module sub2(input p, input q, output z);
  assign z = p | q;
endmodule

module top_mod(input a, b, c, output y);
  wire w;
  sub1 u1(.a(a), .b(b), .y(w));
  sub2 u2(.p(w), .q(c), .z(y));
endmodule
```

#### **Hierarchical Synthesis**

* The design hierarchy (top → submodules) is preserved through synthesis.
* You synthesize each block (or module) independently, then compose them.
* Good for large designs: easier debugging, reusability, faster incremental builds.

**Typical Yosys flow:**

```sh
yosys
read_liberty -lib path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog top_mod.v sub1.v sub2.v
hierarchy -check  # ensures module graph is correct
synth -top top_mod
abc -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog top_mod_hier.v
show top_mod
```

*(Insert a figure showing hierarchical structure / module tree)*

#### **Flattened / Flat Synthesis**

* The synthesis tool merges all modules into one flat netlist, removing hierarchy boundaries.
* Advantages:

  * Cross-module optimizations (logic merging, pushing logic across module boundaries).
  * Potentially better area/timing.
* Drawbacks:

  * Harder to debug or trace to original RTL.
  * Larger runtime for big designs.

**Yosys commands:**

```sh
yosys
read_liberty -lib path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog top_mod.v
synth -top top_mod
flatten
abc -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog top_mod_flat.v
show top_mod
```

*(Insert a figure showing flattened netlist or flattened logic)*

#### **Submodule-First Synthesis (Modular Synthesis)**

* Instead of only top or full flatten, you can *selectively* synthesize submodules—useful when you have repeated blocks or modules instantiated many times.
* This helps reuse synthesized netlists, speeds up iterative design, and supports parallel workflows.

Use commands like:

```sh
synth -top sub1
abc -liberty path/to/lib
write_verilog sub1_synth.v
```

---

## 3. Flip-Flop Coding Styles & Best Practices

The way you write flip-flops in RTL affects whether synthesis tools infer them cleanly, whether they map to proper standard-cell DFFs, and how they behave under corner conditions.

### Common Styles & Trade-offs

#### **Asynchronous Reset D Flip-Flop**

```verilog
module dff_async_reset(
  input  clk,
  input  async_rst,
  input  d,
  output reg q
);
  always @(posedge clk or posedge async_rst) begin
    if (async_rst)
      q <= 1'b0;
    else
      q <= d;
  end
endmodule
```

* Reset is immediate (no dependency on clock).
* Good for fast reset response, but asynchronous resets can cause metastability or reset deassertion glitches if not handled carefully.

#### **Synchronous Reset D Flip-Flop**

```verilog
module dff_sync_reset(
  input clk,
  input sync_rst,
  input d,
  output reg q
);
  always @(posedge clk) begin
    if (sync_rst)
      q <= 1'b0;
    else
      q <= d;
  end
endmodule
```

* Reset is applied in the clock edge. Easier timing analysis and avoids asynchronous reset hazards.
* But reset assertion/deassertion consumes a clock edge — slower recovery.

#### **Combined Asynchronous + Synchronous Reset**

```verilog
module dff_async_sync(
  input        clk,
  input        async_rst,
  input        sync_rst,
  input        d,
  output reg   q
);
  always @(posedge clk or posedge async_rst) begin
    if (async_rst)
      q <= 1'b0;
    else if (sync_rst)
      q <= 1'b0;
    else
      q <= d;
  end
endmodule
```

* Provides fast asynchronous reset and synchronous reset coverage.
* Be cautious about reset priority and ensure no race or conflict.

---

### Best Practices & Tips

* Use **nonblocking assignment (`<=`)** inside flip-flops to avoid race conditions and simulation vs synthesis mismatches.
* Do **not mix blocking (`=`) and nonblocking (`<=`)** for registers in the same always block.
* Avoid using multiple always blocks that drive the same `reg` — that causes ambiguity or undefined behavior.
* If your library has **dedicated flip-flop cells**, use a **`dfflibmap`** (or similar) pass to map your inferred registers to library-specific DFFs. (Many flows require this)
* In many synthesis flows, asynchronous resets are directly supported by the library; synchronous resets may be translated through combinational logic or muxes.

---

## 4. Integrated Simulation & Synthesis Workflow

Here’s a combined flow to simulate, synthesize, and verify a flip-flop design:

### **Simulation (Icarus Verilog / GTKWave)**

Assume we have `dff_async_reset.v` and a simple testbench `tb_dff.v`.

```sh
iverilog dff_async_reset.v tb_dff.v   # compile
./a.out                                # run simulation
gtkwave tb_dff.vcd                     # view waveform
```

*(Insert waveform image of flip-flop transitions here)*

### **Synthesis with Yosys + SKY130 PDK**

```sh
yosys
read_liberty -lib path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_reset.v
synth -top dff_async_reset
dfflibmap -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog dff_async_reset_synth.v
```

* The `dfflibmap` step ensures your generic flip-flops are translated into the target DFF cells from the library.
* `abc` does technology mapping and logic optimization.
* You can inspect the netlist, timing, area reports, and leverage cross-check with your simulation.

---

## 5. Practical Optimization Techniques

Here are a few common tricks & techniques used in RTL and synthesis to produce more efficient logic:

### **Multiplications by Constants**

* Multiplying by powers of 2 can be done by shifting:

  ```verilog
  assign mul_by_8 = in << 3;
  ```
* For non-power-of-2 constants, decompose:

  ```verilog
  // Multiply by 9 = 8 + 1
  assign mul_by_9 = (in << 3) + in;
  ```

### **Boolean Logic Refactoring**

* Simplify complex conditional logic by recognizing patterns or truth tables.
* Example: nested ternary operations or repeated expressions can often be collapsed or simplified.

### **Constant Propagation / Dead-Code Removal**

* If certain signals are constant or never change, synthesis tools can remove logic or reduce gates.
* You should write clean RTL to make it easier for optimizations to kick in.

### **Selecting NAND Over NOR**

* NAND gates scale better when adding inputs: their PMOS network is parallel (thus lower resistance).
* NOR gates have series PMOS, which slows them down as you increase input count.
* Many synthesizers prefer NAND-based implementations implicitly in technology mapping.

---

## 6. Expanded Concepts & Advanced Topics

### **Retiming & Optimal Placement of Flip-Flops**

* After synthesis, a tool can move (retime) flip-flops to better balance path delays across logic.
* Retiming doesn’t change logic functionality, but can help close timing across long paths.

### **VirtualSync / Sequential Optimization (Research Idea)**

* In advanced flows, techniques allow signals to “bypass” certain flip-flop timing constraints in critical paths while still preserving interface timing correctness. This is part of advanced timing optimization (e.g., VirtualSync+, which can push performance beyond normal sequential limits) ([arXiv][1])
* Use with caution: such techniques are research-level and require strong verification.

### **Timing Closure & Slack Analysis**

* Once a netlist is generated, you compute **arrival times**, **required times**, and **slack** to see where the design fails or is comfortable. ([users.ece.utexas.edu][2])
* Worst slack < 0 means timing violation.
* Tools will optimize buffering, resizing, and restructuring to recover slack.

---

## 7. Summary & Key Takeaways

* **Timing libraries** describe how each cell behaves under PVT corners—crucial for mapping RTL into real hardware.
* **Synthesis strategy** matters: preserve hierarchy for modularity or flatten for cross-module optimization.
* **Flip-flop coding style** (async vs sync reset, usage of nonblocking, etc.) directly impacts synthesis, timing, and correctness.
* A proper **simulation → synthesis flow** includes mapping inferred registers to library DFFs and verifying equivalence.
* Use **RTL-level optimizations** (shifts, logic simplification, constant propagation) to lighten downstream logic.
* Advanced topics (retiming, virtual sync, slack-based optimization) push designs closer to theoretical performance limits.

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
read_verilog good_counter.v
synth -top good_counter
```
![yosys](https://github.com/Muthukumarj-42/vsd-tapeout/blob/94b7a251debf5d045f4ffab26308c53eb196d0ed/week-1%20/%20pictures/yosys.png)
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![abc](https://github.com/Muthukumarj-42/vsd-tapeout/blob/c2dd39242c7db48f80762518442a51de20ffe762/week-1%20/%20pictures/abc%20liberty.png)

The show command will display the schematic of the design file
![schematic](https://github.com/Muthukumarj-42/vsd-tapeout/blob/ef71d4eb131f65f59e388559285a21c1082d81e5/week-1%20/%20pictures/xdot.png)
```
write_verilog good_counter_netlist.v
!gvim good_counter_netlist.v
write_verilog -noattr good_counter_netlist.v
!gvim good_counter_netlist.v
```
The code in **good_counter_netlist.v**
```
(* top =  1  *)
(* src = "good_counter.v:1.1-18.10" *)
module good_counter(clk, reset, cnt);
  (* src = "good_counter.v:1.28-1.31" *)
  input clk;
  wire clk;
  (* src = "good_counter.v:1.40-1.45" *)
  input reset;
  wire reset;
  (* src = "good_counter.v:1.65-1.68" *)
  output [1:0] cnt;
  reg [1:0] cnt;
  (* src = "good_counter.v:6.1-14.4" *)
  wire [1:0] _0_;
  (* src = "good_counter.v:6.1-14.4" *)
  wire _1_;
  (* src = "good_counter.v:6.1-14.4" *)
  wire _2_;
  (* src = "good_counter.v:1.65-1.68" *)
  wire _3_;
  (* src = "good_counter.v:1.65-1.68" *)
  wire _4_;
  sky130_fd_sc_hd__nor2_1 _5_ (
    .A(_4_),
    .B(_3_),
    .Y(_1_)
  );
  sky130_fd_sc_hd__nor2b_1 _6_ (
    .A(_4_),
    .B_N(_3_),
    .Y(_2_)
  );
  (* src = "good_counter.v:6.1-14.4" *)
  always @(posedge clk, posedge reset)
    if (reset) cnt[0] <= 1'h0;
    else cnt[0] <= _0_[0];
  (* src = "good_counter.v:6.1-14.4" *)
  always @(posedge clk, posedge reset)
    if (reset) cnt[1] <= 1'h0;
    else cnt[1] <= _0_[1];
  assign _4_ = cnt[1];
  assign _3_ = cnt[0];
  assign _0_[0] = _1_;
  assign _0_[1] = _2_;
endmodule
```

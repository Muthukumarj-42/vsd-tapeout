# Day 3: Combinational and Sequential Optimization

Today’s session focuses on **optimisation techniques** in digital design. Synthesis tools automatically perform many of these steps, but understanding them helps us interpret results and write better RTL.

Optimisations broadly fall into two categories:

* **Combinational Logic Optimisation**: Simplifying logic equations and removing redundancy.
* **Sequential Logic Optimisation**: Improving state machines, balancing paths, or retiming registers for timing closure.

---

## Table of Contents

1. [Combinational Logic Optimisation](#combinational-logic-optimisation)

   * Constant Propagation
   * Boolean Logic Simplification
   * Labs: `opt_check.v`, `opt_check2.v`, `opt_check3.v`

2. [Sequential Logic Optimisation](#sequential-logic-optimisation)

   * State Optimisation
   * Cloning
   * Retiming
   * Labs: `dff_const1.v`, `dff_const2.v`, `dff_const3.v`

3. [Summary](#summary)

---

## Combinational Logic Optimisation

### 1. Constant Propagation

If a signal is tied to a constant, the tool replaces it directly and cleans up the logic.

Example:

<pre>
assign y = a & 1’b1;   // → y = a  
assign z = b & 1’b0;   // → z = 0  
</pre>  

Benefits:

* Simpler netlist
* Fewer gates
* Lower power

---

### 2. Boolean Logic Simplification

Boolean algebra, K-Maps, and tool-driven optimisations reduce the number of gates without changing functionality.

Example:

<pre>
F = A·B + A·B' + A'·B   →   F = A + B  
</pre>  

This reduces both area and delay.

---

### Labs on Combinational Logic Optimisation

#### `opt_check.v`

Navigate to the file and open it:

```bash
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
vim opt_check.v
```

**Verilog Code:**

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

Here,

<pre>
y = a ? b : 0  
</pre>  

behaves like `a & b`.

Run synthesis as in Day 2, but insert this line before `abc -liberty`:

```bash
opt_clean -purge
```

This cleans up unused logic and produces a smaller netlist.

**Optimised Netlist View:**
![opt\_check\_show]()

---

#### `opt_check2.v`

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

This reduces to `y = a | b`.

**Netlist after optimisation:**
![opt\_check2\_show]()

---

#### `opt_check3.v`

```verilog
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

This effectively performs `y = a & b & c`.

**Netlist after optimisation:**
![opt\_check3\_show]()

---

## Sequential Logic Optimisation

### 1. State Optimisation

FSMs often contain unused or redundant states. Tools remove unreachable states and merge equivalent ones. Re-encoding (binary, one-hot, gray) may also be applied to balance area and timing.

Benefits:

* Fewer flip-flops
* Simplified next-state logic
* Better power/timing tradeoffs

---

### 2. Cloning

When one driver feeds many loads, tools may duplicate the driver to reduce fanout.

* **Before:** one AND gate drives 20 FFs.
* **After cloning:** two AND gates, each drive 10 FFs.

This improves timing but uses slightly more area.

---

### 3. Retiming

Registers can be shifted across logic to balance delays between pipeline stages.

* Critical path is reduced.
* Functionality remains identical.
* Clock frequency can be increased.

---

## Labs on Sequential Logic Optimisation

### `dff_const1.v`

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

* Async reset sets `q=0`.
* Otherwise, `q` loads constant 1.

![dff\_const1\_show]()

### `dff_const2.v`

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

Here, `q` is always 1 regardless of clock or reset.

![dff\_const2\_show]()

---

### `dff_const3.v`

```verilog
module dff_const3(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

* On reset: `q=1`, `q1=0`.
* After reset: `q1` is set to 1, and `q` takes the previous value of `q1`.
* Effectively: `q` outputs 0 for one cycle after reset, then stays at 1.

![dff\_const3\_show]()


## Summary

* **Combinational**:

  * Constant propagation removes redundant logic.
  * Boolean simplification minimises equations.
  * Verified via labs (`opt_check`, `opt_check2`, `opt_check3`).

* **Sequential**:

  * State optimisation reduces FSM size.
  * Cloning helps timing by splitting loads.
  * Retiming balances pipeline delays.
  * Verified via labs (`dff_const1`, `dff_const2`, `dff_const3`).

* **Tool support**:

  * `opt_clean -purge` in Yosys performs logic cleanup.
  * Optimisation improves **area, power, and timing** while preserving functionality.

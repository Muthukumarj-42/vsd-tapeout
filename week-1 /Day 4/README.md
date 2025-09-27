# Day 4: Gate-Level Simulation (GLS), Blocking vs Non-Blocking, and Synthesis-Simulation Mismatch

Day 4 covers:

* **Gate-Level Simulation (GLS)**
* **Blocking vs. Non-Blocking Assignments in Verilog**
* **Synthesis-Simulation Mismatch**

We’ll learn the **theory** first and then apply it in **hands-on labs**.

---

## Table of Contents

1. [Gate-Level Simulation (GLS)](#gate-level-simulation-gls)
2. [Synthesis-Simulation Mismatch](#synthesis-simulation-mismatch)
3. [Blocking vs. Non-Blocking Statements](#blocking-vs-non-blocking-statements)

   * [3.1 Blocking Statements](#31-blocking-statements)
   * [3.2 Non-Blocking Statements](#32-non-blocking-statements)
   * [3.3 Comparison Table](#33-comparison-table)
4. [Labs](#labs)

   * [4.1 Ternary Operator MUX](#41-ternary-operator-mux)
   * [4.2 Bad MUX](#42-bad-mux)
   * [4.3 Blocking Assignment Caveat](#43-blocking-assignment-caveat)
5. [Summary](#summary)
6. [Conclusion](#conclusion)

---

## 1. Gate-Level Simulation (GLS)

**Definition:**
Gate-Level Simulation verifies the **synthesized netlist** (made of gates + flops) instead of RTL.

**Why GLS?**

* **Synthesis Validation** → ensures synthesis didn’t break functionality.
* **Timing Verification** → checks real-world delays using SDF.
* **Testability** → confirms scan chains, resets, gated clocks.
* **Confidence** → ensures post-synthesis netlist = RTL.

**Types of GLS:**

* **Zero-delay** → logic equivalence.
* **Unit-delay** → exposes race conditions.
* **SDF back-annotated** → real timing.

**Challenges:**

* Slower than RTL sim.
* Debugging X-propagation & init issues.
* Requires more memory and setup.

![gls]()

---

## 2. Synthesis-Simulation Mismatch

Occurs when **RTL simulation** results differ from **post-synthesis netlist or hardware**.

**Causes:**

* Missing sensitivity list.
* Blocking vs non-blocking misuse.
* Non-synthesizable constructs (`initial`, delays).
* Incomplete coding (missing else).
* Tool interpretation differences.

**Example:**

```verilog
always @(a) begin
  y = a & b;   // ‘b’ missing in sensitivity list
end
```

* RTL sim ignores changes in `b`.
* Hardware uses full sensitivity.
* Fix: `always @(*)`.

---

## 3. Blocking vs Non-Blocking Statements

### 3.1 Blocking (`=`)

* Executes sequentially, immediate update.
* Good for **combinational logic**.
* Example:

  ```verilog
  always @(*) y = a & b;
  ```

### 3.2 Non-Blocking (`<=`)

* RHS evaluated first, all LHS updated at timestep end.
* Correct for **sequential/clocked logic**.
* Example:

  ```verilog
  always @(posedge clk) q <= d;
  ```

### 3.3 Comparison

| Blocking (`=`)       | Non-Blocking (`<=`)     |
| -------------------- | ----------------------- |
| Sequential           | Concurrent              |
| Updates instantly    | Updates end of timestep |
| Use in combinational | Use in sequential       |
| Infers gates         | Infers flip-flops       |

![blockingvsnonblocking]()

---

## 4. Labs

### 4.1 Ternary Operator MUX

**Code:**

```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

**RTL Simulation:**

![waveform1]()

**GLS Command:**

```bash
iverilog -o ~/Documents/Verilog/Labs/ternary_operator_mux_gls.vvp \
~/Documents/Verilog/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v \
~/Documents/Verilog/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v \
~/Documents/Verilog/Labs/ternary_operator_mux_net.v \
~/Documents/Verilog/sky130RTLDesignAndSynthesisWorkshop/verilog_files/tb_ternary_operator_mux.v
```

**GLS Waveform:**

![waveform2]()

✅ Matches RTL → No mismatch.

---

### 4.2 Bad MUX

**Code (wrong):**

```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @(sel) begin
    if (sel) y <= i1;
    else     y <= i0;
  end
endmodule
```

**Issues:**

* Missing sensitivity list.
* Wrong use of non-blocking in combinational logic.

**RTL vs GLS:**

![comparison]()

⚠ Mismatch → Hardware correct, RTL sim wrong.

---

### 4.3 Blocking Assignment Caveat

**Code:**

```verilog
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

**Problem:**

* `d` uses old `x`, since blocking executes in order.
* Hardware is parallel.

**Fix:**

```verilog
always @ (*) begin
  x = a | b;
  d = x & c;
end
```

**Comparison:**

![comparisonbc]()

---

## 5. Summary

* **GLS** validates netlist timing & testability.
* **Mismatch** caused by coding errors (sensitivity list, blocking misuse).
* **Blocking vs Non-blocking** → use `=` for combinational, `<=` for sequential.
* **Labs** showed correct vs incorrect styles.

---

## 6. Conclusion

GLS may be slow but is **essential** before tape-out.
It catches subtle mismatches between RTL and netlist, ensuring silicon correctness.

# **Day 5 – Optimization in Synthesis**

Welcome to **Day 5** of the RTL workshop!
Today, we focus on optimization in Verilog synthesis: using `if-else` and `case` statements, avoiding inferred latches, and exploring loops and generate blocks for scalable hardware design. Labs are included for hands-on practice.

---

## Contents

* [1. If-Else Statements in Verilog](#1-if-else-statements-in-verilog)
* [2. Case Statements in Verilog](#2-case-statements-in-verilog)
* [3. If-Else with Case](#3-if-else-with-case)
* [4. Inferred Latches in Verilog](#4-inferred-latches-in-verilog)
* [5. Labs for If-Else and Case Statements](#5-labs-for-if-else-and-case-statements)

  * [Lab 1: Incomplete If Statement](#lab-1-incomplete-if-statement)
  * [Lab 2: Synthesis Result of Lab 1](#lab-2-synthesis-result-of-lab-1)
  * [Lab 3: Nested If-Else](#lab-3-nested-if-else)
  * [Lab 4: Synthesis Result of Lab 3](#lab-4-synthesis-result-of-lab-3)
  * [Lab 5: Complete Case Statement](#lab-5-complete-case-statement)
  * [Lab 6: Synthesis Result of Lab 5](#lab-6-synthesis-result-of-lab-5)
  * [Lab 7: Incomplete Case Handling](#lab-7-incomplete-case-handling)
  * [Lab 8: Partial Assignments in Case](#lab-8-partial-assignments-in-case)
* [6. For Loops in Verilog](#6-for-loops-in-verilog)
* [7. Generate Blocks in Verilog](#7-generate-blocks-in-verilog)
* [8. Ripple Carry Adder (RCA)](#8-ripple-carry-adder-rca)
* [9. Labs on Loops and Generate Blocks](#9-labs-on-loops-and-generate-blocks)
* [Summary](#summary)

---

## 1. If-Else Statements in Verilog

Used to create **priority logic** in behavioral modeling.

### Syntax

```verilog
always @(*) begin
    if (condition)
        statement1;
    else
        statement2;
end
```

### Example

```verilog
always @(*) begin
    if (sel)
        y = a;
    else
        y = b;
end
```

---

### Nested If-Else

```verilog
always @(*) begin
    if (sel1) begin
        if (sel2)
            y = a;
        else
            y = b;
    end else begin
        y = c;
    end
end
```

---

## 2. Case Statements in Verilog

Used for **multi-way selection**.

### Syntax

```verilog
always @(*) begin
    case(expression)
        value1: statement1;
        value2: statement2;
        ...
        default: statementN;
    endcase
end
```

### Example

```verilog
always @(*) begin
    case(sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
        default: y = 0;
    endcase
end
```

---

## 3. If-Else with Case

```verilog
always @(*) begin
    if (enable) begin
        case(sel)
            2'b00: y = a;
            2'b01: y = b;
            2'b10: y = c;
            default: y = 0;
        endcase
    end else begin
        y = 0;
    end
end
```

> [!Note]
>
> * Outer `if` handles **enable/disable conditions**.
> * Inner `case` handles **multi-way selection**.
> * Always assign outputs in **all paths** to avoid latches.

---

### If-Else vs Case

| Feature              | If-Else                      | Case                         |
| -------------------- | ---------------------------- | ---------------------------- |
| **Purpose**          | Conditional execution        | Multi-way selection          |
| **Usage**            | Binary or nested decisions   | Multiplexers / states        |
| **Default Handling** | Optional                     | Default strongly recommended |
| **Readability**      | Complex with many conditions | Cleaner for discrete options |
| **Evaluation**       | Sequential                   | Parallel matching            |

---

## 4. Inferred Latches in Verilog

A **latch** is inferred when a signal in a combinational block is **not assigned** in all possible conditions.
This often happens in incomplete `if-else` or `case` statements.

### Example of Latch Inference

```verilog
always @(*) begin
    if (enable)
        y = a;
    // Missing else → y retains previous value → latch inferred
end
```

### Corrected Version

```verilog
always @(*) begin
    if (enable)
        y = a;
    else
        y = 0; // fully defined
end
```

> [!Tip]
>
> * Always **complete if-else**.
> * Always include **default in case**.
> * Use `@(*)` for full sensitivity list.

---

## 5. Labs for If-Else and Case Statements

### Lab 1: Incomplete If Statement

```verilog
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @(*) begin
    if(i0)
        y <= i1;
end
endmodule
```

![workflow1]()
![waveform1]()

---

### Lab 2: Synthesis Result of Lab 1

![workflow2]()
![dotfile1]()

---

### Lab 3: Nested If-Else

```verilog
module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @(*) begin
    if(i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```

![workflow3]()
![waveform2]()

---

### Lab 4: Synthesis Result of Lab 3

![workflow4]()
![dotfile2]()

---

### Lab 5: Complete Case Statement

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default: y = i2;
    endcase
end
endmodule
```

![workflow5]()
![waveform3]()

---

### Lab 6: Synthesis Result of Lab 5

![workflow6]()
![dotfile3]()

---

### Lab 7: Incomplete Case Handling

```verilog
module bad_case (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // wildcard leads to overlap!
    endcase
end
endmodule
```

![workflow7]()
![waveform4]()
![workflow8]()
![dotfile4]()
![workflow9]()
![waveform5]()

---

### Lab 8: Partial Assignments in Case

```verilog
module partial_case_assign (
    input i0, input i1, input i2,
    input [1:0] sel,
    output reg y, output reg x
);
always @(*) begin
    case(sel)
        2'b00: begin
            y = i0;
            x = i2;
        end
        2'b01: y = i1; // x not assigned → latch!
        default: begin
            x = i1;
            y = i2;
        end
    endcase
end
endmodule
```

![workflow10]()

---

## 6. For Loops in Verilog

Synthesizable if iteration count is **fixed**.

```verilog
for (init; condition; step) begin
    // body
end
```

### Example: 4-to-1 MUX with For Loop

```verilog
module mux_4to1_for_loop (
    input wire [3:0] data,
    input wire [1:0] sel,
    output reg y
);
    integer i;
    always @(*) begin
        y = 1'b0;
        for (i = 0; i < 4; i = i + 1) begin
            if (i == sel)
                y = data[i];
        end
    end
endmodule
```

---

## 7. Generate Blocks in Verilog

Generate blocks allow **compile-time repetition**.

```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : gen_loop
        and_gate and_inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```

---

## 8. Ripple Carry Adder (RCA)

* Uses **n full adders** for an n-bit sum.
* Each stage’s carry-out connects to next stage’s carry-in.

![rca]()

---

## 9. Labs on Loops and Generate Blocks

### Lab 9: 4-to-1 MUX Using For Loop

```verilog
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```

![mux\_generate]()

---

### Lab 10: 8-to-1 Demux Using Case

```verilog
module demux_case (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
always @(*) begin
    y_int = 8'b0;
    case(sel)
        3'b000 : y_int[0] = i;
        3'b001 : y_int[1] = i;
        3'b010 : y_int[2] = i;
        3'b011 : y_int[3] = i;
        3'b100 : y_int[4] = i;
        3'b101 : y_int[5] = i;
        3'b110 : y_int[6] = i;
        3'b111 : y_int[7] = i;
    endcase
end
endmodule
```

![demux\_case]()

---

### Lab 11: 8-to-1 Demux Using For Loop

```verilog
module demux_generate (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
integer k;
always @(*) begin
    y_int = 8'b0;
    for (k = 0; k < 8; k = k + 1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```

![demux\_generate]()

---

### Lab 12: 8-bit Ripple Carry Adder with Generate Block

```verilog
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

**Full Adder Module**

```verilog
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```

![rca]()

---

## Summary

* Always use **complete if-else and case statements** to avoid unintended latches.
* Use **default assignments** in case statements.
* **For loops** and **generate blocks** provide scalable, synthesizable hardware structures.
* Ripple Carry Adder (RCA) demonstrates **generate + full adder chaining**.
* Labs reinforce concepts with Verilog code + synthesis results.

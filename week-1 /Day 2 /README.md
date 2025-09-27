# **Day 2: Timing Libraries, Hierarchical vs Flat Synthesis, and Flip-Flop Coding Styles**

---

## 1. Timing Library (`.lib`) in SKY130 PDK

* A `.lib` (Liberty file) contains **timing, power, and functional information** of every standard cell in the library.
* Example:

  ```
  sky130_fd_sc_hd__tt_025C_1v80.lib
  ```

  * `tt` → typical process
  * `025C` → 25 °C temperature
  * `1v80` → 1.8 V supply

### Open the `.lib` file

```
cd lib/sky130_fd_sc_hd__tt_025C_1v80.lib
gvim sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/Muthukumarj-42/vsd-tapeout/blob/b48ec2890da277fde617ceebb56d76684ff22108/week-1%20/Day%202%20/pictures/libfile.png)

## 2. Hierarchical vs Flat Synthesis

### Example RTL Code

```
module sub_module1 (input a, input b, output y);
  assign y = a & b;
endmodule

module sub_module2 (input x, input c, output z);
  assign z = x | c;
endmodule

module multiple_modules (input a, input b, input c, output y, output z);
  wire net;
  sub_module1 u1 (.a(a), .b(b), .y(net));
  sub_module2 u2 (.x(net), .c(c), .z(z));
endmodule
```

![image](https://github.com/Muthukumarj-42/vsd-tapeout/blob/06d74d9d6552ea3bf18caf3caf4918671fc91c30/week-1%20/Day%202%20/pictures/mutliple_module.png)

### **Hierarchical Synthesis Flow**

```
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog -noattr multiple_modules_hier.v
```
![image](https://github.com/Muthukumarj-42/vsd-tapeout/blob/7637b92eb4776ac78bb5d956ea0444b51bb032c2/week-1%20/Day%202%20/pictures/yosys-multiple.png)
![image](https://github.com/Muthukumarj-42/vsd-tapeout/blob/a0a85896a4ac268bc62b83ea01628519d1407862/week-1%20/Day%202%20/pictures/show%20multiple.png)

### **Flat Synthesis Flow**

```
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
flatten
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog -noattr multiple_modules_flat.v
```
![image](https://github.com/Muthukumarj-42/vsd-tapeout/blob/7637b92eb4776ac78bb5d956ea0444b51bb032c2/week-1%20/Day%202%20/pictures/yosys-multiple.png)
![image](https://github.com/Muthukumarj-42/vsd-tapeout/blob/7e19f1bb11ed5aa9e07c314e094f74de57f705df/week-1%20/Day%202%20/pictures/flatten.png)

### **Submodule Synthesis Flow**

```
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr sub_module1_netlist.v
```
![image](https://github.com/Muthukumarj-42/vsd-tapeout/blob/82b66da49211f0f159f9003f3bbe5604186dfdc3/week-1%20/Day%202%20/pictures/sub_module1.png)

---

## 3. Flip-Flop Coding Styles

### Asynchronous Reset

```verilog
module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @(posedge clk or posedge async_reset) begin
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
  end
endmodule
```

---

### Synchronous Reset

```
module dff_syncres (input clk, input sync_reset, input d, output reg q);
  always @(posedge clk) begin
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
  end
endmodule
```

---

### Asynchronous + Synchronous Reset

```verilog
module dff_async_syncres (input clk, input async_reset, input sync_reset, input d, output reg q);
  always @(posedge clk or posedge async_reset) begin
    if (async_reset)
      q <= 1'b0;
    else if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
  end
endmodule
```

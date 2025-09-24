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
![write](https://github.com/Muthukumarj-42/vsd-tapeout/blob/27b06df50751d7be2fb42c4a60c2280c95c25b85/week-1%20/%20pictures/write_verilog.png)
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

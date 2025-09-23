# Setting Up the Workspace

Before starting the workshop, let’s prepare the environment where we’ll run all the experiments.

### 🔹 Step 1: Make a Project Directory

First, create a directory called **`VSD`**.
This directory will serve as the central workspace throughout the sessions.

```
mkdir VSD
```

### 🔹 Step 2: Move Into the Directory

Switch into the newly created folder and createa another folder caller **VLSI** so that all files are kept organized in one place.

```
cd VSD
mkdir VLSI
```
### 🔹 Step 4: Move Into the Directory

Switch into the newly created folder so that all files are kept organized in one place.

```
cd VLSI
```

### 🔹 Step 5: Fetch the Repository

Now, clone the official OpenLane GitHub repository:

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
List the files in the folder using **ls** command
```
ls
cd sky130RTLDesignAndSynthesisWorkshop
ls
cd verilog_files
```
![gitclone](https://github.com/Muthukumarj-42/vsd-tapeout/blob/d599eae68919a34346f657b7544c31b71737f822/week-1%20/%20pictures/gitclone.png)

### 🔹 Step 6: Run the verilog files using iverilog

Using iverilog run the Design file code and testbench file

*Example*
* Design file (good_counter.v)
* Test_bench file (tb_good_counter.v)
```
iverilog  good_counter.v tb_good_counter.v
```
After running in the iverilog. The output file will be create in the name **a.out**.
![aout](https://github.com/Muthukumarj-42/vsd-tapeout/blob/1f1f18ed05e939215ee4508279e827adf4c75730/week-1%20/%20pictures/aout.png)
Run the a.out
```
./a.out
```
![vcd](https://github.com/Muthukumarj-42/vsd-tapeout/blob/7a312a3936d6a91f6d279caf50d925375332c2a5/week-1%20/%20pictures/vcdfile.png)

### 🔹 Step 7: GTKwave 

Using GTKwave we can see the output waveform in the vcd file
```
gtkwave tb_good_counter.vcd
```
![wave]()


# **Day 1: Getting Started with Icarus Verilog (iverilog)**

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

## **3. How Simulation Works**

* The simulator **monitors all input signals** given to the design.
* When an input changes → the output is **re-evaluated**.
* If inputs remain stable → the output stays **unchanged**.

## **Important Notes**
✔ A **design module** can have one or more primary **inputs/outputs**.
✔ A **testbench** itself does **not** have inputs/outputs — it only wraps around the design to test it.

---

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

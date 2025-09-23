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
  ![testbench](https://github.com/Muthukumarj-42/vsd-tapeout/blob/06684ce6edfae7450a923903edf8f8c745cb6aa7/week-1%20/%20pictures/teshbench.png)

## **3. How Simulation Works**

* The simulator **monitors all input signals** given to the design.
* When an input changes → the output is **re-evaluated**.
* If inputs remain stable → the output stays **unchanged**.

## **Important Notes**
✔ A **design module** can have one or more primary **inputs/outputs**.
✔ A **testbench** itself does **not** have inputs/outputs — it only wraps around the design to test it.

---

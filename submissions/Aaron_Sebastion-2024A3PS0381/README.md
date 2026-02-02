# Digital Stopwatch Controller

## Overview
This project implements a synchronous digital stopwatch controller in Verilog with a C++ software driver for simulation via Verilator. The design supports Start, Stop (Pause), and Reset functionality with a display range of 00:00 to 99:59.

## Directory Structure
* **rtl/**: Contains synthesizeable Verilog source code.
* **tb/**: Contains the Verilog testbench (`tb_stopwatch.v`).
* **verilator_sw/**: Contains the C++ controller (`main.cpp`) and Makefile.

## Design Choices
1.  **FSM Design (Moore Machine):**
    * A Moore FSM was selected for the Control Logic.
    * **Reason:** Moore outputs depend only on the current state, ensuring glitch-free control signals (`count_en`, `count_rst`) for the synchronous counters.
    
2.  **Counter Architecture:**
    * **Seconds Counter:** Mod-60 synchronous up-counter.
    * **Minutes Counter:** Mod-100 synchronous up-counter.
    * **Synchronization:** The minutes counter is enabled only when the FSM is RUNNING *and* the seconds counter reaches its terminal count (59). This avoids ripple delays and ensures all logic remains synchronous to the system clock.

3.  **Reset Logic:**
    * **Hardware Reset (`rst_n`):** Active-low asynchronous reset for system initialization.
    * **Functional Reset (`reset`):** Active-high synchronous reset controlled by the FSM to clear the timer while the system is powered.

## Tools Used
* **Simulation:** Verilator (v5.020 or compatible)
* **Compilation:** GNU Make and G++
* **Editor:** VS Code on WSL (Ubuntu)

## How to Run (Verilator)
1.  Navigate to the `verilator_sw` directory.
2.  Run the command: `make run`
3.  The simulation output will appear in the terminal, showing the timer behavior.
4.  After running the simution once, run the command: `make clean` to remove the existing object.

# sv_synchronous_fifo
Parameterized synchronous FIFO design and verification using SystemVerilog

# Synchronous FIFO Design and Verification (SystemVerilog)

## 🔍 Overview

This project implements a parameterized **Synchronous FIFO (First-In-First-Out)** buffer using SystemVerilog. FIFO structures are widely used in digital systems for temporary data storage, buffering, and managing data flow between producer and consumer modules operating under the same clock domain.

---

## ⚙️ Design Features

* Parameterized **data width (DWIDTH)** and **depth (DEPTH)**
* Circular buffer implementation using memory array
* Separate **write pointer (wptr)** and **read pointer (rptr)**
* Status flags:

  * **Full**: prevents overflow
  * **Empty**: prevents underflow
* Synchronous read and write operations (single clock domain)

---

## 🧠 Design Architecture

The FIFO consists of the following components:

* **Memory Array**: Stores incoming data
* **Write Pointer (wptr)**: Tracks write location
* **Read Pointer (rptr)**: Tracks read location
* **Control Logic**: Generates `full` and `empty` flags

### Key Logic

* **Write Operation**

  * Enabled when `wr_en = 1` and FIFO is not full
  * Data is written to memory and write pointer increments

* **Read Operation**

  * Enabled when `rd_en = 1` and FIFO is not empty
  * Data is read from memory and read pointer increments

* **Empty Condition**

  ```
  wptr == rptr
  ```

* **Full Condition**

  ```
  (wptr + 1) == rptr
  ```

> Note: This implementation sacrifices one memory location to distinguish between full and empty conditions.

---

## 🧪 Verification Strategy

A SystemVerilog testbench is developed to validate FIFO functionality under different conditions.

### Test Scenarios Covered

* Reset behavior verification
* Continuous write operations until FIFO is full
* Continuous read operations until FIFO is empty
* Simultaneous read and write operations
* Randomized stimulus for `wr_en`, `rd_en`, and input data
* Handling of boundary conditions (full and empty states)

### Testbench Features

* Clock generation
* Random input stimulus using `$random`
* Console logging using `$display` and `$monitor`
* Independent read and write processes

---

## 📊 Simulation Results

Simulation verifies:

* Correct sequential data storage and retrieval
* Proper assertion of **full** and **empty** flags
* Stable FIFO behavior under random read/write conditions

Waveforms demonstrate:

* FIFO filling phase → `full = 1`
* FIFO emptying phase → `empty = 1`
* Correct data order preservation (FIFO behavior)

*(Add waveform screenshot in `/results/waveform.png`)*

---

## ▶️ How to Run

Using ModelSim / QuestaSim:

```bash
vlog rtl/sync_fifo.sv tb/sync_fifo_tb.sv
vsim tb
run -all
```

---

## 📚 Key Learnings

* Implementation of circular buffer using pointers
* Handling pointer wrap-around conditions
* Understanding FIFO control logic and flag generation
* Importance of verification in ensuring design correctness
* Observing real-time hardware behavior through simulation

---

## ⚠️ Limitations

* Full detection logic sacrifices one memory location
* Designed only for **single clock domain** (not suitable for asynchronous FIFO)
* No advanced verification techniques like assertions or coverage implemented

---

## 🚀 Future Improvements

* Implement **Asynchronous FIFO** with clock domain crossing (CDC)
* Add **SystemVerilog Assertions (SVA)** for robust verification
* Introduce **functional coverage**
* Extend to **UVM-based verification environment**
* Optimize full/empty detection logic

---

## 📌 Tools Used

* SystemVerilog
* Xilinx Vivado (for simulation)

---

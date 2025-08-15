#  Asynchronous FIFO (First-In-First-Out) Buffer

The **Asynchronous FIFO** is a dual-clock, parameterizable Verilog implementation designed for safe and reliable data transfer between modules operating in **different clock domains**. This module uses **Gray code** synchronization techniques to prevent metastability and pointer errors, making it ideal for interfacing across independent clock domains in complex FPGA or ASIC systems.

---

##  Key Features

###  Dual Clock Domain Support
- Separate **write (`wr_clk`)** and **read (`rd_clk`)** clocks.
- Robust pointer synchronization using **Gray code** and **double-register synchronization** across clock domains.

###  Parameterized Design
- `data_width`: Configurable data width (default: 8 bits).
- `add_width`: Address width for FIFO depth (default: 4 bits ⇒ 16 entries).

###  Control & Status Signals
- `wr_en`, `rd_en`: Control write and read operations.
- `full`, `empty`: Flags indicating FIFO status.
- `wr_rst`, `rd_rst`: Separate asynchronous resets for each domain.

---

##  Pointer and Memory Architecture

- Internal **register array** used for FIFO storage.
- Binary write/read pointers converted to **Gray code** for cross-domain synchronization.
- **Gray to binary conversion** enables reliable full and empty detection.
- Full/Empty detection:
  - `full`: Write pointer equals synchronized read pointer with MSB inverted.
  - `empty`: Read pointer equals synchronized write pointer.

---

##  Port Descriptions

| Signal        | Direction | Description                                |
|---------------|-----------|--------------------------------------------|
| `data_in`     | Input     | Data input to FIFO (write side)            |
| `wr_en`       | Input     | Write enable                               |
| `wr_rst`      | Input     | Write domain reset                         |
| `wr_clk`      | Input     | Write domain clock                         |
| `full`        | Output    | FIFO full flag                             |
| `data_out`    | Output    | Data output from FIFO (read side)          |
| `rd_en`       | Input     | Read enable                                |
| `rd_rst`      | Input     | Read domain reset                          |
| `rd_clk`      | Input     | Read domain clock                          |
| `empty`       | Output    | FIFO empty flag                            |

---

##  Architecture Overview

The asynchronous FIFO is implemented using:

- A **depth-sized memory** array.
- **Binary and Gray-coded pointers** for write and read sides.
- **Cross-clock synchronization** using dual flip-flop synchronizers.
- Support for **safe full and empty** status detection.
- Prevents simultaneous access to memory from different clock domains using decoupled logic.

---

##  Testbench Overview

The included testbench demonstrates:

- FIFO write at **100 MHz** and read at **~71 MHz**.
- Overflow protection (attempts to write beyond `full`).
- Underflow protection (attempts to read beyond `empty`).
- Realistic timing and randomized data for stress-testing behavior.

---

##  Tools & Technologies

- **Verilog HDL**
- **Behavioral + RTL modeling**
- Simulatable with:
  - ModelSim / Questa
  - Vivado Simulator
  - Icarus Verilog
  - Any IEEE 1364-compliant Verilog simulator

---

##  Directory Structure
##  Directory Structure

```text
asynchronous-fifo-verilog/
├── src/
│   └── async_fifo.v       # Main FIFO RTL module
├── tb/
│   └── tb_async_fifo.v    # Testbench for async FIFO
├── README.md              # Project documentation
```



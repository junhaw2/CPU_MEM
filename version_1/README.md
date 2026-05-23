# Minimal CPU-Memory Subsystem in SystemVerilog

This project implements a minimal CPU-memory subsystem in SystemVerilog, starting from a simple CPU core and gradually evolving toward a more realistic memory-access architecture.

The first goal is to build a working CPU-memory closed loop:

- instruction fetch
- instruction decode
- register file read/write
- ALU execution
- data memory load/store
- writeback
- basic branch control
- self-checking simulation

Later stages will add realistic memory interfaces, variable memory latency, stalls, cache, and a simple bus structure.

---

## 1. Project Goal

The goal of this project is not to immediately build a complex processor, but to develop a CPU-memory subsystem step by step.

The initial version uses a small RISC-V-like instruction subset and a simple synchronous memory model. After the basic design is verified, the memory system will be abstracted into a valid-ready interface to support realistic memory latency and backpressure.

The long-term roadmap is:

1. Minimal single-cycle or multi-cycle CPU
2. Instruction memory and data memory access
3. Valid-ready memory interface
4. Variable-latency memory model
5. CPU stall control
6. Direct-mapped cache
7. Five-stage pipeline
8. AXI-Lite-like memory-mapped bus
9. Simple peripheral or accelerator interface

---

## 2. Current Scope

The first implementation focuses on a minimal CPU core with direct instruction and data memory access.

### Supported Instructions

The first version supports a small subset of RV32I-style instructions:

| Instruction | Type | Description |
|------------|------|-------------|
| `ADD` | R-type | Register-register addition |
| `SUB` | R-type | Register-register subtraction |
| `AND` | R-type | Bitwise AND |
| `OR`  | R-type | Bitwise OR |
| `ADDI` | I-type | Register-immediate addition |
| `LW` | I-type | Load word from data memory |
| `SW` | S-type | Store word to data memory |
| `BEQ` | B-type | Branch if equal |

Future instructions may include:

| Instruction | Type | Description |
|------------|------|-------------|
| `BNE` | B-type | Branch if not equal |
| `JAL` | J-type | Jump and link |
| `JALR` | I-type | Jump and link register |
| `SLT` | R-type | Set less than |
| `XOR` | R-type | Bitwise XOR |

---

## 3. Architecture Overview

The first version uses the following basic datapath:

```text
PC
 |
 v
Instruction Memory
 |
 v
Decoder / Immediate Generator
 |
 v
Register File
 |
 v
ALU
 |
 v
Data Memory
 |
 v
Writeback
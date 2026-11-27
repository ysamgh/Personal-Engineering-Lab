# Session 007 — معماری CPU و اجزای پردازنده

**Stage:** 1 · Computer Architecture & Operating Systems  
**Level:** Intermediate · **Time:** 120–160 minutes  
**Prerequisites:** Sessions 004–006 · **Safety:** GREEN

## 1. What a CPU is responsible for

A Central Processing Unit executes instructions defined by an Instruction Set Architecture (ISA). It reads operands, performs operations, makes control-flow decisions and coordinates data movement. The CPU is not the whole computer: memory, storage, devices, interconnects, firmware and the operating system participate in program execution.

Separate three levels:
1. **ISA:** programmer-visible contract: instructions, registers, data types, exceptions, memory model and privilege behavior.
2. **Microarchitecture:** internal implementation: pipelines, execution units, caches, predictors, queues and scheduling.
3. **Physical implementation:** transistors, standard cells, clock distribution, wiring, voltage and thermal constraints.

Different CPU designs can implement the same ISA and run the same machine-code program while having very different internal structures and performance.

## 2. Main CPU building blocks

### Register file

Registers are small, fast storage locations inside the processor. The ISA defines visible registers; microarchitecture may include additional internal registers.

Common roles:
- General-purpose registers: integer and pointer operands.
- Program Counter (PC): instruction address used by current execution flow.
- Stack Pointer (SP): conventionally tracks a stack region.
- Status/flags register: condition information where provided.
- Floating-point/vector registers: numeric and SIMD operations.

Names and semantics are ISA-specific. x86-64 and AArch64 expose different register models.

### Arithmetic Logic Unit (ALU)

The ALU performs integer arithmetic and logical operations such as add, subtract, AND, OR, XOR, shifts and comparisons. Floating-point operations are commonly handled by separate execution units, though implementation varies.

An ALU receives operands and control signals, computes a result and may provide status information. It does not independently decide which instruction should execute; control logic coordinates the datapath.

### Control unit

The control unit interprets instruction fields and orchestrates the datapath: which registers are read, which operation is selected, whether memory is accessed, where the result is written and how the PC changes.

A simple educational CPU may model control as a finite-state machine. Modern processors may decode instructions into internal micro-operations.

### Datapath

The datapath is the collection of registers, ALUs, shifters, buses, multiplexers and connections that move and transform data. Control signals select paths and operations.

- Datapath: where values move and computations happen.
- Control path: decides which operations and routes are active.

## 3. Buses and interconnects

A simplified teaching model separates:
- Address: identifies a location or target.
- Data: carries values.
- Control: indicates operation and coordination.

Modern systems use complex point-to-point and packetized interconnects, cache-coherence fabrics and memory controllers. The three-bus model is an introductory abstraction, not a literal diagram of every modern CPU.

Bandwidth, latency, arbitration, topology and coherence matter. A wide link does not guarantee low latency; high bandwidth does not guarantee that a single dependent operation completes quickly.

## 4. ISA as a contract

An ISA specifies:
- instruction encodings and semantics;
- register set and visible state;
- addressing modes;
- memory access rules and alignment constraints;
- exceptions and interrupts;
- privilege levels and system instructions;
- memory ordering rules.

Assembly is a human-readable representation of instructions. Machine code is the encoded bit pattern. An assembler maps mnemonics and operands to machine-code encodings.

Illustrative pseudo-assembly, not tied to a specific ISA:

    LOAD R1, [R2]
    ADD  R3, R1, R4
    STORE [R5], R3

Conceptually: read a value from memory through the address in R2, add it to R4, then store the result through R5. Actual syntax, addressing, register rules and ordering depend on the ISA.

## 5. RISC and CISC: avoid simplistic labels

RISC and CISC describe historical design philosophies and ISA characteristics, not a reliable ranking of modern CPU quality.

- Traditional RISC designs emphasize relatively regular instruction formats and load/store organization.
- Traditional CISC designs often expose richer instruction semantics and addressing forms.

Modern implementations blur the boundary. Some CISC processors decode complex instructions into simpler internal micro-operations; some RISC designs have sophisticated pipelines, vector units and complex memory systems. Evaluate actual ISA, microarchitecture, workload, power and performance—not the label alone.

## 6. Performance: frequency is not the whole story

A rough CPU execution-time model:

CPU time = Instruction Count × CPI × Clock Cycle Time

Where:
- Instruction Count: number of instructions executed for the workload.
- CPI: average cycles per instruction.
- Clock Cycle Time: duration of one clock cycle.

Clock frequency is the inverse of cycle time. Higher frequency does not automatically mean a faster system: instruction count, CPI, memory stalls, branch behavior, parallelism, thermal limits and workload characteristics all matter.

Performance comparisons require equivalent workloads, compiler/runtime settings and careful measurement. GHz and core count alone are insufficient.

## 7. Interrupts and exceptions

An interrupt or exception changes normal instruction flow so the processor can handle an event:
- External interrupt: device or timer event.
- Synchronous exception: caused by the current instruction or its execution, such as an invalid operation or page fault.
- System call: controlled transition from user execution to an operating-system service interface.

The CPU saves or exposes sufficient execution state according to the architecture, transfers control to a handler, and eventually resumes or terminates the context. Exact mechanisms differ by ISA.

## 8. Observe the ISA on a real system

Read-only Linux commands:

    uname -m
    lscpu
    cat /proc/cpuinfo

These report architecture and CPU properties exposed by the OS. Output varies by kernel, virtualization, permissions and hardware. A VM or container may expose a virtualized view.

Optional compiler inspection if GCC is installed:

    gcc --version
    gcc -O0 -S sample.c -o sample.s

This compiles a source file to assembly and writes sample.s; it does not execute the program. Use a local source file you created. Output depends on target architecture, compiler version and flags.

## 9. Failure and bottleneck analysis

- **Compute-bound:** execution units are the limiting resource.
- **Memory-bound:** data movement or memory latency dominates.
- **Branch-heavy:** control-flow prediction and misprediction may dominate.
- **Synchronization-bound:** threads wait on locks or coordination.
- **I/O-bound:** storage or network waits dominate.

CPU utilization alone does not identify the bottleneck. Combine workload context, profiling, counters, latency and resource observations.

## 10. Engineering artifact

Create a one-page CPU block diagram containing register file, ALU, control unit, PC and instruction state, memory interface, data and control paths. Annotate which components are ISA-visible and which are implementation details in your chosen architecture.

## Further reading
- RISC-V ISA specifications: https://riscv.org/technical/specifications/
- Intel Software Developer Manuals: https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html
- Arm Architecture Reference Manuals: https://developer.arm.com/documentation
- MIT 6.004: https://ocw.mit.edu/courses/6-004-computation-structures-spring-2017/

# Session 008 — چرخه اجرای دستور: از Fetch تا Write-back

**Stage:** 1 · Computer Architecture & Operating Systems  
**Level:** Intermediate · **Time:** 100–140 minutes  
**Prerequisite:** Session 007 · **Safety:** GREEN

## 1. A program is a sequence of machine instructions

A compiled program is represented as machine instructions and data. The CPU repeatedly performs work according to its ISA and implementation. Fetch–Decode–Execute is a useful conceptual decomposition, but real processors overlap work across multiple instructions using pipelines and may execute internally out of order.

The architecture defines the program-visible result. The microarchitecture determines how the processor achieves it.

## 2. Core state

- **Program Counter (PC):** identifies the instruction address used by the current execution flow.
- **Instruction Register (IR):** a teaching-model register holding the fetched instruction; modern implementations may use queues and decoded micro-operations instead.
- **Register file:** holds architectural operands.
- **ALU / execution units:** perform operations.
- **Memory interface:** fetches instructions and reads/writes data through the memory hierarchy.
- **Control logic:** decodes instruction semantics and selects operations.

Do not assume every CPU has a literal single IR register matching a textbook diagram. It is an explanatory abstraction.

## 3. Basic instruction lifecycle

### Fetch

The processor obtains instruction bytes from the instruction side of the memory hierarchy, using the PC or equivalent fetch state. Instruction cache hits may satisfy fetch without accessing DRAM. If bytes are unavailable in cache, lower levels are consulted.

The next sequential PC may be calculated, but control-flow instructions can redirect it. Some ISAs define variable-length instructions; alignment and fetch boundaries are ISA-specific.

### Decode

The processor determines operation, source and destination registers, immediate fields, addressing mode and required privilege or checks.

A simple processor may decode directly into control signals. A complex implementation may translate instructions into internal micro-operations.

### Execute

The processor performs the required operation: ALU arithmetic/logic, address calculation, branch condition evaluation, floating-point/vector operation, or another specialized operation. Some instructions require several internal steps or cycles.

### Memory access

Load instructions read data through the memory hierarchy. Store instructions update memory according to architecture and memory-ordering rules. A load that misses in cache can wait much longer than an ALU operation. Store buffers and caches complicate timing and visibility.

### Write-back / retirement

A result is made available to the architectural destination state. In a simple in-order CPU, write-back is a clear stage. In an out-of-order CPU, execution may finish before an instruction is committed/retired in program order. Retirement preserves the architectural behavior required by the ISA, including precise exceptions in supported designs.

## 4. Worked conceptual example

Illustrative pseudo-assembly:

    ADD R3, R1, R2

Assume R1=7 and R2=4.

1. Fetch: obtain the encoded ADD instruction.
2. Decode: identify ADD, source registers R1/R2 and destination R3.
3. Read operands: obtain 7 and 4 from the register file.
4. Execute: ALU computes 11.
5. Write-back: update R3 to 11.
6. Continue: execution proceeds according to the next PC.

The result is 11. The example omits flags, exceptions, pipeline overlap, register renaming and architecture-specific behavior.

## 5. Control flow changes the PC

A branch changes which instruction is fetched next. A conditional branch depends on a condition; an unconditional jump redirects control regardless of a comparison.

Modern CPUs predict branch outcomes to keep pipelines busy. A misprediction causes speculative work on the wrong path to be discarded or prevented from committing architecturally. The architectural state must remain correct even if internal speculation occurred.

Branch prediction is a performance mechanism, not permission for a program to observe arbitrary wrong-path architectural results. Microarchitectural side channels are a separate security concern.

## 6. Pipelining

A simple five-stage teaching pipeline is:
1. IF — Instruction Fetch
2. ID — Instruction Decode / Register Read
3. EX — Execute / Address Calculation
4. MEM — Data Memory Access
5. WB — Write Back

Pipeline stages overlap different instructions. This increases throughput; it does not necessarily reduce the latency of one instruction by the same factor.

### Hazards

- **Structural hazard:** two operations compete for the same hardware resource.
- **Data hazard:** an instruction depends on a value not yet available.
- **Control hazard:** the next instruction address depends on a branch or exception.

Common techniques include forwarding/bypassing, pipeline stalls, branch prediction, resource duplication and scheduling/reordering where allowed.

A pipeline diagram must distinguish instruction latency from throughput. A pipeline that completes one instruction per cycle in steady state still has multiple cycles of latency from fetch to completion.

## 7. Out-of-order execution and retirement

Modern high-performance CPUs may execute independent instructions when operands and resources are ready, even if earlier instructions are stalled. Register renaming removes false dependencies caused by reuse of architectural register names. A reorder buffer or equivalent mechanism tracks progress and commits results in the architecturally required order.

This improves utilization but adds dependency tracking, speculative execution, recovery after branch misprediction, precise exception handling, power and area cost. The ISA does not require one particular pipeline design.

## 8. System calls and privilege transition

A user program cannot directly perform every privileged operation. A system-call instruction or ABI-defined mechanism requests a controlled service from the kernel. The CPU changes privilege state according to architecture rules, transfers control to a kernel entry point, and later returns to user execution.

A system call is not merely an ordinary function call: it crosses a protection boundary and follows a defined ABI and kernel entry protocol.

## 9. Observe generated machine-level behavior

On Linux with GCC:

    cat > /tmp/pel-add.c <<'EOF'
    int add(int a, int b) { return a + b; }
    int main(void) { return add(7, 4); }
    EOF
    gcc -O0 -S /tmp/pel-add.c -o /tmp/pel-add.s
    cat /tmp/pel-add.s

This creates temporary source and assembly files. It does not execute the program. Assembly syntax and instructions depend on host target and compiler. Do not assume output matches a particular textbook or CPU.

Cleanup only if you created these exact files for this lab:

    rm /tmp/pel-add.c /tmp/pel-add.s

For deeper inspection, the objdump -d command can disassemble a locally compiled executable. Disassembly interprets bytes under a chosen ISA and mode; it is not source code and may not recover original names or structure.

## 10. Engineering artifact

Create a pipeline timeline for three independent instructions and annotate:
- which stage each instruction occupies per cycle;
- one data dependency;
- how forwarding or a stall handles it;
- one branch hazard and recovery point.

Use a simulator or clearly labeled hand-drawn diagram. State assumptions such as single-issue, in-order and five stages.

## Further reading
- RISC-V ISA specifications: https://riscv.org/technical/specifications/
- MIT 6.004 Computation Structures: https://ocw.mit.edu/courses/6-004-computation-structures-spring-2017/
- Computer Architecture: A Quantitative Approach: https://www.elsevier.com/books/computer-architecture-a-quantitative-approach/hennessy/978-0-12-811905-1/

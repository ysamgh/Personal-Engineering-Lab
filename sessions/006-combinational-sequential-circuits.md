# Session 006 — مدارهای ترکیبی و ترتیبی

**Stage:** 1 · Computer Architecture & Operating Systems  
**Level:** Intermediate · **Time:** 120–160 minutes  
**Prerequisite:** Session 005 · **Safety:** GREEN

## 1. Two fundamental circuit classes

- **Combinational logic:** output depends on current inputs.
- **Sequential logic:** output depends on current inputs and stored state.

A calculator's combinational adder computes a sum from current operands. A counter must remember its previous value; it therefore needs state.

A synchronous sequential circuit can be modeled as next-state logic, storage elements, and a clock reference. The clock does not make every internal signal change simultaneously; it defines when state updates are accepted.

## 2. Combinational building blocks

### Half adder

A half adder adds two one-bit inputs A and B:
- Sum S = A XOR B
- Carry C = A AND B

| A | B | Sum | Carry |
|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

It has no carry-in input, so it cannot alone serve every position in a multi-bit adder.

### Full adder

A full adder accepts A, B and carry-in Cin:
- Sum = A XOR B XOR Cin
- Carry-out = AB + ACin + BCin

A full adder can be built from two half adders and an OR gate. Chaining full adders creates a ripple-carry adder. It is simple, but carry propagation delay can grow with bit width. Faster architectures trade additional logic and wiring for lower carry delay.

### Multiplexer (MUX)

A MUX selects one of several inputs and forwards the selected value. A 2-to-1 MUX has data inputs D0 and D1, select S, output Y:

Y = (¬S · D0) + (S · D1)

| S | Y |
|---:|---|
| 0 | D0 |
| 1 | D1 |

MUXes select operands, results, addresses and control paths in datapaths.

### Decoder and comparator

A 2-to-4 decoder maps two input bits to four output lines; a one-hot implementation asserts exactly one output for each input combination. Decoders appear in address selection and instruction control.

A comparator determines equality, less-than or greater-than. Equality can be built from XNOR per bit followed by AND across all positions. Signed and unsigned comparison differ because the same bit pattern has different numeric interpretations.

## 3. Sequential logic: state and memory

A sequential circuit preserves information across time. A latch or flip-flop is a basic storage element.

### Latch

A level-sensitive latch is transparent during an enable level and holds its previous output when disabled. Timing analysis must account for transparency windows and possible race paths.

### Flip-flop

An edge-triggered flip-flop captures input at a specified clock edge and retains the captured value until a later active edge. A D flip-flop is conceptually:

Q(next) = D sampled at the active clock edge

Between active edges, Q retains its state, ignoring asynchronous control inputs and physical effects.

| Property | Latch | Flip-flop |
|---|---|---|
| Sensitivity | Level | Clock edge |
| Transparency | During enable level | Not transparent as a level |
| Timing model | Enable window | Setup/hold around edge |
| Common use | Specialized storage/timing | Registers and synchronous state |

Neither is universally better; selection depends on timing methodology, power, library and design.

## 4. Clock, setup and hold

A clock is a periodic timing reference. Frequency f and period T are related by T=1/f. A 1 GHz clock has a nominal period of 1 ns.

A synchronous storage element has timing constraints:
- **Setup time:** input must be stable for a minimum interval before the active edge.
- **Hold time:** input must remain stable for a minimum interval after the edge.
- **Clock-to-Q delay:** time from active edge to output change.

Violating setup or hold can capture the wrong value or cause metastability. Synchronizer chains reduce the probability that metastability propagates downstream.

A clock does not automatically synchronize arbitrary external signals. Asynchronous inputs need deliberate crossing strategies: often synchronizer flip-flops for single-bit controls, or handshake/FIFO techniques for multi-bit data.

## 5. Registers and counters

A register is a group of storage elements that holds a multi-bit value. An n-bit register can represent 2^n patterns.

A binary up-counter advances by one on each enabled clock event, wrapping modulo 2^n unless additional logic prevents it. A register with enable updates only when enable is asserted.

Reset initializes state. It may be synchronous (sampled with clock) or asynchronous (acts independently of clock). Reset strategy affects timing and safe release from reset.

## 6. Finite State Machines (FSM)

An FSM models behavior using a finite set of states, inputs, a transition function and outputs.

- **Moore machine:** outputs depend on current state.
- **Mealy machine:** outputs depend on current state and current inputs.

FSMs model controllers: traffic lights, protocol handlers, sequencing logic and appliances. State diagrams and transition tables make behavior reviewable before implementation.

State encoding may use binary, one-hot or other representations. The choice trades off flip-flop count, decode complexity, switching activity and timing.

## 7. Timing is part of correctness

A Boolean expression can be correct while a synchronous implementation fails timing. A simplified register-to-register setup constraint is:

Tclk ≥ Tcq + Tcomb + Tsetup + margin

Real static timing analysis uses clock definitions, constraints, cell libraries, clock skew and interconnect parasitics.

Other concepts:
- **Propagation delay:** input change to output response.
- **Contamination delay:** earliest time output may begin changing.
- **Glitch:** transient output pulse caused by unequal path delays.
- **Clock skew:** difference in clock arrival time at different elements.
- **Clock domain crossing:** transfer between unrelated clock domains.

## 8. Reproducible lab

Use Logisim Evolution or Nand2Tetris Hardware Simulator:
1. Build a half adder from XOR and AND.
2. Build a full adder from two half adders and OR.
3. Cascade four full adders into a 4-bit ripple-carry adder.
4. Add a 2-to-1 MUX to choose between operand sources.
5. Build a 4-bit register using flip-flops.
6. Add enable and reset.
7. Observe combinational output changes in the model while register state updates at the active clock edge.

Tools: https://github.com/logisim-evolution/logisim-evolution and https://www.nand2tetris.org/

## 9. Failure modes

- Incorrect carry wiring creates errors only for some operand patterns.
- Confusing latch transparency with edge-triggered behavior causes timing bugs.
- Missing reset assumptions create unknown initial state in simulation.
- Asynchronous inputs can violate setup/hold requirements.
- A combinational loop may oscillate or settle unpredictably.
- A slow carry path can violate clock timing even when the sum is logically correct.

## 10. Engineering artifact

Produce a circuit diagram and design note for a 4-bit counter with clock, synchronous reset, enable, 4-bit state output and documented wraparound behavior. State the active clock edge and reset polarity.

## Further reading
- MIT 6.004: https://ocw.mit.edu/courses/6-004-computation-structures-spring-2017/
- Nand2Tetris Part I: https://www.nand2tetris.org/
- Intel FPGA Timing Analysis: https://www.intel.com/content/www/us/en/docs/programmable/683243/current.html

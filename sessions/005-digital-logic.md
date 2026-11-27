# Session 005 — منطق دیجیتال و جبر بولی

**Stage:** 1 · Computer Architecture & Operating Systems  
**Level:** Foundation → Intermediate · **Time:** 100–140 minutes  
**Prerequisite:** Session 004 · **Safety:** GREEN

## 1. Why digital logic exists

A processor must make decisions and transform bit patterns. At the physical level, transistors are arranged to implement circuits whose outputs depend on input signals. Digital abstraction treats signals as logical values, commonly 0 and 1. Logic design gives us a precise way to describe and compose those behaviors.

The abstraction is powerful, but it is not the whole physical truth. Real circuits have voltage thresholds, propagation delay, power consumption, noise margins, fan-out limits and manufacturing variation. Boolean equations describe logical behavior; timing and electrical analysis determine whether a physical implementation works reliably.

## 2. Boolean variables and expressions

A Boolean variable has one of two logical values: 0 or 1. A Boolean function maps one or more input variables to an output value. For two variables A and B, a truth table has four input combinations. For n Boolean inputs, there are 2^n combinations.

Notation:
- NOT A: ¬A or A̅
- A AND B: A · B or AB
- A OR B: A + B
- A XOR B: A ⊕ B

Do not confuse Boolean OR with arithmetic addition. Boolean values are not ordinary integers even though they are often encoded as 0 and 1.

## 3. Fundamental logic gates

| Gate | Expression | Output is 1 when |
|---|---|---|
| NOT | ¬A | A is 0 |
| AND | A·B | both inputs are 1 |
| OR | A+B | at least one input is 1 |
| NAND | ¬(A·B) | not both inputs are 1 |
| NOR | ¬(A+B) | both inputs are 0 |
| XOR | A⊕B | inputs differ |
| XNOR | ¬(A⊕B) | inputs are equal |

| A | B | AND | OR | NAND | NOR | XOR | XNOR |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 0 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 |

A gate is a logical abstraction. In a real chip, a gate is implemented with transistor networks and has nonzero delay and power cost.

## 4. Boolean algebra: laws that simplify circuits

The same Boolean function can have multiple expressions and circuit implementations. Simplification can reduce gate count, depth, power, or delay, although physical optimization depends on the technology library and constraints.

| Law | Identity |
|---|---|
| Identity | A·1=A ; A+0=A |
| Null / domination | A·0=0 ; A+1=1 |
| Idempotent | A·A=A ; A+A=A |
| Complement | A·¬A=0 ; A+¬A=1 |
| Involution | ¬(¬A)=A |
| Commutative | A·B=B·A ; A+B=B+A |
| Associative | (A·B)·C=A·(B·C) |
| Distributive | A·(B+C)=AB+AC |
| Absorption | A+AB=A ; A(A+B)=A |

### De Morgan’s laws

¬(A·B) = ¬A + ¬B  
¬(A+B) = ¬A · ¬B

These laws explain how NAND and NOR structures can be transformed into equivalent combinations of other gates. They are central to logic simplification and active-low circuit design.

### Worked simplification

F = A·B + A·¬B

Factor A: F = A·(B+¬B)  
Complement law: F = A·1 = A

A circuit that appears to need multiple gates is equivalent to a direct connection from A to F. A truth table confirms this equivalence for every input combination.

## 5. NAND and NOR are universal gates

A gate is functionally complete if it can be used to construct every Boolean function. NAND alone is functionally complete; NOR alone is also functionally complete.

Using NAND:
- NOT A = A NAND A
- A AND B = (A NAND B) NAND (A NAND B)
- A OR B = (A NAND A) NAND (B NAND B)

Why this matters:
- A chip library may favor particular standard cells.
- Reusing one gate family can simplify some manufacturing or design constraints.
- Synthesis tools map Boolean logic to available cells.
- Logical equivalence does not guarantee identical delay, power or area.

## 6. From truth table to circuit

A systematic design process:
1. Define inputs and outputs.
2. State the expected output for every input combination.
3. Build a truth table.
4. Derive a Boolean expression.
5. Simplify the expression.
6. Draw or implement the gate network.
7. Verify every row against the original truth table.
8. Consider timing, fan-out, hazards and physical constraints.

Example: a two-input logical condition F=A·B is true only when both conditions are true. This is a logical model; real safety systems require redundancy, diagnostics and a safety case, not merely one AND gate.

## 7. Combinational logic boundary

A combinational circuit has outputs determined by its current inputs. It has no intentional memory of previous input values.

Examples: gate networks, decoders, multiplexers, adders and comparators.

In a real circuit, propagation delay means the output does not change instantaneously. During transitions, intermediate glitches may occur. A truth table describes stable logical behavior, not every transient electrical event.

## 8. Observation and simulation

Educational tools:
- Logisim Evolution: https://github.com/logisim-evolution/logisim-evolution
- Nand2Tetris Hardware Simulator: https://www.nand2tetris.org/
- Digital by hneemann: https://github.com/hneemann/Digital

Reproducible lab:
1. Create two input pins A and B.
2. Add AND, OR, XOR and NAND gates.
3. Connect each output to a labeled output pin.
4. Enumerate all four input combinations.
5. Compare simulator output with the truth table.
6. Save a circuit file or screenshot as an artifact.

A simulator is useful for logic verification. Do not interpret it as a transistor-level timing or power model unless it explicitly provides those analyses.

## 9. Failure modes and engineering cautions

- **Wrong polarity:** An active-low signal is asserted at logical 0. Names such as RESET_N or CS_N often indicate active-low behavior; verify the datasheet.
- **Floating input:** An unconnected physical input may not behave as a stable logical 0. Use defined pull-up/pull-down circuitry where required.
- **Glitch:** Different path delays can briefly produce an unintended output transition.
- **Fan-out:** A gate output cannot drive an unlimited number of loads.
- **Timing violation:** A logically correct circuit may fail if signals do not settle before a sampling edge.
- **Metastability:** An asynchronous signal sampled near a clock edge can place a flip-flop into a metastable state. Synchronizers reduce risk but do not make probability zero.

## 10. Engineering artifact

Create a circuit file or diagram implementing F1=A AND B, F2=A XOR B and F3=NOT(A AND B). Include a truth table and distinguish stable Boolean behavior from real propagation delay.

## 11. Transfer task

A digital input from a sensor is active-low, but a software developer assumes that 1 means “alarm active.” Produce an interface note describing signal polarity, truth table, naming convention and test evidence required before connecting it to an alarm controller.

## Exit evidence

- Correct truth tables for the basic gates.
- A Boolean expression simplified using valid identities.
- A working simulated circuit or equivalent gate-level diagram.
- A clear distinction between combinational logic and physical timing behavior.

## Further reading
- Nand2Tetris Part I: https://www.nand2tetris.org/
- MIT 6.004 Computation Structures: https://ocw.mit.edu/courses/6-004-computation-structures-spring-2017/

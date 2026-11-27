# Problem-Solving Playbook

## The loop

**Observe → Define → Decompose → Trace → Hypothesize → Test → Verify → Record**

### 1. Observe
Capture exact error, timestamp, context, recent changes, expected vs actual. Avoid interpretation at first.

### 2. Define
Write one sentence: “When [condition], [system] does [actual] instead of [expected].”

### 3. Decompose
Break the system into layers/components. Draw the data path and dependencies.

### 4. Trace
Follow a request, event, process, packet or data record through its lifecycle. Identify the last known good stage and first failed stage.

### 5. Hypothesize
Create ranked candidate causes. Each hypothesis must predict observable evidence.

### 6. Test
Change or measure one variable at a time. Prefer read-only observations before modifications.

### 7. Verify
Repeat the original test. Check side effects and regression. Define proof of success.

### 8. Record
Write root cause (if established), evidence, fix, verification, and prevention.

## Debugging template

- Expected:
- Actual:
- Reproduction:
- Scope:
- Last known good:
- Recent changes:
- System map:
- Hypothesis A / evidence / test:
- Hypothesis B / evidence / test:
- Root cause (confirmed or still unknown):
- Fix:
- Verification:
- Prevention:
- Follow-up:

## Engineering trade-off template

When selecting an approach, compare:
- Functional fit
- Complexity
- Cost
- Time to MVP
- Security
- Reliability
- Reversibility
- Maintainability
- Scale assumptions
- Vendor lock-in
- Unknowns

Do not optimize for hypothetical scale before evidence. Start with the smallest design that meets real requirements and has a safe path to evolve.

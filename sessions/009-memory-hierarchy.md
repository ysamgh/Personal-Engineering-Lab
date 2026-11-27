# Session 009 — سلسله‌مراتب حافظه: Registers تا RAM و Virtual Memory

**Stage:** 1 · Computer Architecture & Operating Systems  
**Level:** Intermediate · **Time:** 140–180 minutes  
**Prerequisites:** Sessions 004–008 · **Safety:** GREEN

## 1. The memory problem

Processors can execute many operations faster than large storage systems can deliver arbitrary data. No single memory technology simultaneously maximizes capacity, speed, cost per bit and energy efficiency. Computer systems therefore use a hierarchy.

| Level | Typical role | Relative properties |
|---|---|---|
| CPU registers | Active operands and architectural state | Very small, extremely fast |
| L1 cache | Recently/nearby used instructions and data | Small, very fast |
| L2 cache | Larger nearby working set | Larger, slower than L1 |
| Last-level cache (often L3) | Shared or larger cache capacity | Larger, slower than upper levels |
| DRAM | Main memory | Large, volatile, slower than cache |
| SSD/HDD | Persistent storage | Much larger, non-volatile, slower than DRAM |

This is a conceptual order, not a universal physical layout. Cache sizes, sharing, inclusion policies, technologies and access costs differ across processors.

## 2. Why hierarchy works: locality

Caches exploit locality in program behavior.

- **Temporal locality:** data or instructions used recently are likely to be used again.
- **Spatial locality:** addresses near a recently used address are likely to be used soon.
- **Sequential locality:** a common spatial pattern in instruction fetch and array traversal.

A cache transfers data in blocks called cache lines. Fetching a line brings neighboring bytes, not only the exact requested byte. Line size and policy are implementation-dependent.

Locality is a statistical tendency, not a guarantee. Random pointer chasing across a large working set may have poor cache behavior.

## 3. Cache hit and miss

- **Hit:** requested data is found at the checked cache level.
- **Miss:** data is not found there and must be obtained from a lower level.
- **Miss penalty:** additional time/cost to fetch and make data available.
- **Hit rate:** fraction of accesses that hit at a given level.
- **Miss rate:** fraction that miss; for the same access set, miss rate = 1 − hit rate.

An L1 miss may still be an L2 hit, so “cache miss” must specify the level.

A common simplified average memory access time model for one cache level is:

AMAT = Hit time + Miss rate × Miss penalty

For multiple levels, the expression must account for conditional miss rates and penalties at each level. Do not combine percentages or latency values without defining their denominators.

## 4. Cache organization

- **Direct-mapped:** each memory block maps to exactly one cache line. Simple, but conflicts can be frequent.
- **Fully associative:** a block can occupy any line. Flexible, but lookup hardware is more complex.
- **Set-associative:** a block maps to one set and can occupy any way in that set. A common compromise.

When a set is full, a replacement policy selects a line to evict. Examples include approximate LRU, pseudo-LRU and random policies. Real processors use implementation-specific policies.

Cache coherence matters when multiple cores have cached copies of shared memory. Coherence protocols coordinate visibility and ownership of cache lines. Coherence is distinct from consistency: coherence concerns copies/order for a memory location; consistency defines ordering guarantees across operations.

## 5. SRAM vs DRAM

- **SRAM:** commonly used for CPU caches; does not require periodic refresh while powered, but uses more transistors per bit and is relatively expensive.
- **DRAM:** commonly used for main memory; stores bits using capacitor-based cells and requires refresh. It is denser and cheaper per bit, but has different latency and control requirements.

Both are volatile. SSDs and HDDs are non-volatile storage, not RAM.

## 6. Virtual memory: address spaces and translation

A process typically uses a virtual address space. The CPU's Memory Management Unit (MMU), together with OS-managed page tables and translation structures, maps virtual addresses to physical memory.

- **Virtual address:** address used by an executing process.
- **Physical address:** address in the machine's physical memory address space.
- **Page:** fixed-size unit of virtual memory management; common sizes depend on architecture and OS.
- **Page table:** data structure describing mappings and permissions.
- **TLB:** Translation Lookaside Buffer; cache of recent address translations.
- **Page fault:** exception raised when a translation or access requires OS handling; not every page fault means disk I/O.

A page fault may be minor (for example, mapping already-resident memory) or major (requiring storage I/O). Exact categories vary by OS.

Virtual memory provides isolation, flexible mapping, permissions and demand paging. It does not mean every process receives a dedicated physical RAM module.

## 7. Memory protection and permissions

Page-table entries commonly encode permissions such as read, write and execute, plus user/supervisor access. A violation can raise a protection fault. The OS may terminate the process, signal it, or handle the condition according to fault and policy.

W^X (write xor execute) is a security principle that aims to prevent a memory region from being writable and executable at the same time. Real systems have exceptions and complex policies; inspect the actual platform.

## 8. RAM pressure, swapping and performance

When active working sets exceed available physical memory, the OS may reclaim caches, compress memory, or move less-active pages to swap/pagefile. Heavy paging can cause severe latency and thrashing.

Do not infer a memory leak from high RAM use alone. The OS may use otherwise-free memory for cache. Diagnose using trends, process resident memory, allocation behavior, swap activity and workload context.

Read-only Linux observations:

    free -h
    cat /proc/meminfo
    vmstat 1 5
    ps -eo pid,comm,rss,vsz --sort=-rss | head

- free -h: memory summary; distinguish available from free.
- /proc/meminfo: kernel memory counters.
- vmstat 1 5: five interval samples; first line may summarize since boot depending on implementation.
- rss: resident set size in KiB on common procps systems; vsz: virtual size. Neither alone tells the whole allocation story.

Output may reveal usernames, process names or workload details; redact before sharing publicly.

## 9. Safe locality experiment

A small program can compare sequential and pseudo-random access patterns, but timings are affected by compiler optimization, CPU frequency, cache state, OS scheduling and virtualization. A benchmark must be repeated and designed carefully.

The conceptual expectation: sequential array traversal often benefits from spatial locality and hardware prefetching; random access across a large array may cause more cache and translation misses. This is a hypothesis to measure, not a guaranteed fixed ratio.

Use a local development environment and avoid stress tests on production or shared systems.

## 10. Common misconceptions

- “RAM is the same as storage.” False: RAM is volatile working memory; SSD/HDD are persistent storage.
- “A page fault always means data was read from disk.” False: many faults are resolved without storage I/O.
- “More cache always makes every program faster.” False: workload, latency, bandwidth, associativity, coherence and power matter.
- “Free memory is the only useful memory metric.” False: reclaimable cache and available memory matter.
- “Virtual memory is fake RAM.” False: it is an address translation and protection abstraction backed by physical memory and, in some cases, storage.

## 11. Engineering artifact

Create a memory-path diagram for a load operation:
CPU request → L1 → L2 → last-level cache → DRAM → return path.

Annotate hit/miss decisions, where a cache line is transferred, where virtual-to-physical translation occurs, where a page fault may enter the OS, and which latencies are illustrative rather than universal.

## Further reading
- Linux kernel memory management: https://docs.kernel.org/admin-guide/mm/
- Intel 64 and IA-32 Software Developer Manuals: https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html
- Arm Architecture Reference Manuals: https://developer.arm.com/documentation
- MIT 6.004: https://ocw.mit.edu/courses/6-004-computation-structures-spring-2017/

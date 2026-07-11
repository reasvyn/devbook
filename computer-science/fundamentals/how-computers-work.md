# How Computers Work

## Description

From transistors to the text editor you open every day — this document maps the full stack of a modern computer. You will learn how binary encodes everything, how the CPU executes instructions, how memory is organized, how the operating system orchestrates hardware, and what actually happens between double-click and a rendered window.

## Prerequisites

- [What Is Computer Science?](../intro/what-is-computer-science.md) — what this field is and why it matters

## Table of Contents

- [The Big Picture: What Is a Computer?](#the-big-picture-what-is-a-computer)
- [Binary: The Language of Computers](#binary-the-language-of-computers)
- [The CPU and the Fetch-Execute Cycle](#the-cpu-and-the-fetch-execute-cycle)
- [Memory Hierarchy](#memory-hierarchy)
- [Storage: How Data Persists](#storage-how-data-persists)
- [Input / Output](#input--output)
- [The Operating System as Hardware Manager](#the-operating-system-as-hardware-manager)
- [The Boot Process](#the-boot-process)
- [Putting It All Together: Opening a Text Editor](#putting-it-all-together-opening-a-text-editor)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## The Big Picture: What Is a Computer?

A computer is a machine that accepts data (input), transforms it according to a stored program (process), and produces a result (output). Every computer, from a microcontroller in a microwave to the server streaming this page, shares the same four fundamental components:

```
┌─────────────────────────────────────────────────────┐
│                    COMPUTER                          │
│                                                      │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐      │
│  │  INPUT   │───▶│  CPU     │───▶│ OUTPUT  │      │
│  │  DEVICES │    │ (brain)  │    │ DEVICES  │      │
│  └──────────┘    └────┬─────┘    └──────────┘      │
│                       │                             │
│                       ▼                             │
│                ┌──────────────┐                     │
│                │    MEMORY    │                     │
│                │  (RAM +      │                     │
│                │   Storage)   │                     │
│                └──────────────┘                     │
└─────────────────────────────────────────────────────┘
```

| Component | Role | Examples |
|-----------|------|----------|
| **CPU** (Central Processing Unit) | Executes instructions — the "brain" | Intel Core i9, Apple M3, AMD Ryzen |
| **RAM** (Random Access Memory) | Fast temporary storage for active data | DDR5 32 GB |
| **Storage** | Persistent long-term data | SSD, HDD |
| **Input devices** | Feed data into the system | Keyboard, mouse, microphone, camera |
| **Output devices** | Present results to the user | Monitor, speakers, printer |

The key insight: a computer is a **stored-program machine**. Unlike a calculator where the logic is hard-wired, a general-purpose computer reads instructions from memory and executes them. Change the instructions, and the same hardware becomes a word processor, a browser, or a game engine.

### Von Neumann Architecture

Almost every modern computer follows the von Neumann architecture, named after mathematician John von Neumann. Its defining characteristic is a **single shared memory space** for both instructions and data:

```
       ┌──────────────────────────────────────┐
       │              MEMORY                   │
       │  ┌─────────┬─────────┬─────────┐     │
       │  │ Instr 1 │ Instr 2 │ Data 1  │ ...  │
       │  └─────────┴─────────┴─────────┘     │
       └──────────────┬───────────────────────┘
                      │ bus
       ┌──────────────▼───────────────────────┐
       │              CPU                      │
       │  ┌─────────┐  ┌───────────────────┐  │
       │  │ Control │  │   ALU (Arithmetic │  │
       │  │  Unit   │  │   Logic Unit)     │  │
       │  └─────────┘  └───────────────────┘  │
       │  ┌────────────────────────────────┐   │
       │  │        Registers              │   │
       │  └────────────────────────────────┘   │
       └──────────────────────────────────────┘
```

The bus is the communication highway that carries data, addresses, and control signals between components.

## Binary: The Language of Computers

### Why Binary?

A computer's most basic building block is the **transistor** — an electronic switch that can be ON (conducting current) or OFF (blocking current). Two states map naturally to the digits 1 and 0. This is why computers use **binary** (base 2) instead of decimal (base 10).

A single binary digit is a **bit** (short for **b**inary dig**it**). Eight bits grouped together form a **byte**. One byte can represent 256 distinct values (2⁸ = 256).

### Counting in Binary

| Decimal | Binary | Notes |
|---------|--------|-------|
| 0 | 0000 | All bits off |
| 1 | 0001 | Rightmost bit on |
| 2 | 0010 | Second bit on |
| 3 | 0011 | 2 + 1 |
| 4 | 0100 | Third bit on |
| 5 | 0101 | 4 + 1 |
| 6 | 0110 | 4 + 2 |
| 7 | 0111 | 4 + 2 + 1 |
| 8 | 1000 | Fourth bit on |
| 15 | 1111 | All four bits on (max for 4 bits) |

Each position represents a power of 2: from right to left, 2⁰ = 1, 2¹ = 2, 2² = 4, 2³ = 8, and so on.

```
Binary:   1   0   1   1
Power:    2³  2²  2¹  2⁰
Value:    8 + 0 + 2 + 1 = 11
```

### How Data Is Represented in Binary

#### Numbers

Integers are stored directly in binary. Negative numbers typically use **two's complement**, where the leftmost bit indicates the sign (0 = positive, 1 = negative). For floating-point numbers (decimals), most computers use the **IEEE 754 standard**, which splits bits into sign, exponent, and mantissa.

#### Text

Text is encoded by mapping each character to a number, then storing that number in binary. The most common encoding today is **UTF-8**, which is backwards-compatible with ASCII:

| Character | ASCII (decimal) | Binary (8-bit) |
|-----------|-----------------|-----------------|
| A | 65 | 01000001 |
| B | 66 | 01000010 |
| a | 97 | 01100001 |
| 0 | 48 | 00110000 |
| (space) | 32 | 00100000 |

A "Hello" string in memory: `01001000 01100101 01101100 01101100 01101111`

#### Colors

A pixel on your screen is typically represented by three bytes: one for red, one for green, one for blue (RGB). A bright red pixel would be `11111111 00000000 00000000` — full red, zero green, zero blue.

#### Instructions

Program instructions are binary too. A CPU's instruction set assigns a numeric **opcode** to each operation. For example, in a simplified instruction set:

```
Opcode 0001 → LOAD (load a value from memory into a register)
Opcode 0010 → ADD  (add two registers)
Opcode 0011 → STORE (write a register value to memory)
```

A program is just a sequence of these binary instructions stored in memory.

### Units of Data

| Unit | Size | Approximate Capacity |
|------|------|---------------------|
| Bit | 1 binary digit | Single ON/OFF |
| Byte | 8 bits | One character |
| Kilobyte (KB) | 1024 bytes | A short email |
| Megabyte (MB) | 1024 KB | A 3-minute MP3 song |
| Gigabyte (GB) | 1024 MB | An HD movie (~1.5 GB) |
| Terabyte (TB) | 1024 GB | ~250,000 photos |

### Hexadecimal (Base 16)

Because binary strings are long and error-prone for humans, programmers use **hexadecimal** as a shorthand. Each hex digit represents 4 bits:

| Binary | Hex | Decimal |
|--------|-----|---------|
| 0000 | 0 | 0 |
| 0001 | 1 | 1 |
| ... | ... | ... |
| 1001 | 9 | 9 |
| 1010 | A | 10 |
| 1011 | B | 11 |
| 1100 | C | 12 |
| 1101 | D | 13 |
| 1110 | E | 14 |
| 1111 | F | 15 |

A byte like `10101100` is written as `AC` in hex. Memory addresses, color codes (`#FF5733`), and debug output are almost always shown in hex.
## The CPU and the Fetch-Execute Cycle

### CPU Anatomy

A CPU contains several key components:

```
┌──────────────────────────────────────────┐
│                  CPU                      │
│                                          │
│  ┌──────────┐  ┌──────────────────────┐  │
│  │Control   │  │   ALU                │  │
│  │Unit      │  │  (Arithmetic Logic   │  │
│  │(CU)      │  │   Unit)              │  │
│  │ ── reads │  │  ── ADD, SUB, AND,   │  │
│  │   instrs │  │     OR, XOR, SHIFT   │  │
│  │ ── decodes│  │                     │  │
│  │ ── orchestrates│                  │  │
│  └──────────┘  └──────────────────────┘  │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │         REGISTERS                  │  │
│  │  ┌──────┐ ┌──────┐ ┌──────┐       │  │
│  │  │ PC   │ │ IR   │ │  R0  │  ...  │  │
│  │  └──────┘ └──────┘ └──────┘       │  │
│  └────────────────────────────────────┘  │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │         CACHE                      │  │
│  │  ┌──────┐ ┌──────┐ ┌──────┐       │  │
│  │  │ L1   │ │ L2   │ │ L3   │       │  │
│  │  └──────┘ └──────┘ └──────┘       │  │
│  └────────────────────────────────────┘  │
└──────────────────────────────────────────┘
```

- **Control Unit (CU)** — decodes instructions and directs the flow of data
- **ALU (Arithmetic Logic Unit)** — performs math and logical operations
- **Registers** — ultra-fast storage locations inside the CPU (usually 16–64 bytes each, measured in nanoseconds to access)
- **Cache** — small but very fast memory inside the CPU package

Important registers:

| Register | Name | Purpose |
|----------|------|---------|
| PC | Program Counter | Address of the next instruction to execute |
| IR | Instruction Register | Holds the currently executing instruction |
| SP | Stack Pointer | Tracks the top of the call stack |
| Flags / Status Register | — | Stores condition results (zero, carry, overflow) |
| R0–Rn | General-purpose registers | Scratch space for calculations |

### The Fetch-Execute Cycle

The CPU operates in an endless loop called the **fetch-execute cycle** (also called the instruction cycle):

```
      ┌──────────────┐
      │   START      │
      └──────┬───────┘
             ▼
      ┌──────────────┐
      │  FETCH: read │
      │  instruction │
      │  from memory │◄───────── Program Counter (PC)
      │  at address  │           points to next instr
      │  in PC       │
      └──────┬───────┘
             ▼
      ┌──────────────┐
      │  DECODE:     │
      │  interpret   │
      │  the opcode  │
      │  and operands│
      └──────┬───────┘
             ▼
      ┌──────────────┐
      │  EXECUTE:    │
      │  perform the │
      │  operation   │
      │  (ALU, move, │
      │  branch...)  │
      └──────┬───────┘
             ▼
      ┌──────────────┐
      │  UPDATE PC   │
      │  PC = PC + 1 │──────► back to FETCH
      └──────────────┘
```

Let's trace a concrete example. Suppose the CPU executes `ADD R0, R1` — add the value in register R1 to register R0 and store the result in R0.

```
Step 1 (FETCH):   PC holds address 0x100. CPU sends address 0x100
                  on the memory bus. Memory returns the 32-bit
                  instruction 0xE0810001.

Step 2 (DECODE):  Control Unit decodes: opcode = ADD (0xE0),
                  destination = R0, source = R1.

Step 3 (EXECUTE): ALU reads R0 (currently 5) and R1 (currently 7),
                  computes 5 + 7 = 12, writes 12 back to R0.

Step 4 (UPDATE):  PC = PC + 4 (32-bit CPU), now points to 0x104.
                  Cycle repeats.
```

A modern CPU executes billions of these cycles per second (GHz). A 3 GHz CPU completes roughly one cycle every 0.33 nanoseconds.

### Pipelining and Modern CPU Features

Simple fetch-execute is too slow for modern demands. CPUs now use:

- **Pipelining** — while one instruction executes, the next is being decoded, and the one after that is being fetched. This keeps all parts of the CPU busy.
- **Superscalar execution** — multiple execution units let the CPU run several instructions simultaneously.
- **Out-of-order execution** — the CPU reorders instructions to keep the pipeline full, as long as the final result is correct.
- **Branch prediction** — the CPU guesses which way a conditional jump will go and speculatively executes ahead. If wrong, it discards the work.

```
Simple pipeline (5 stages):

Clock:  1    2    3    4    5    6    7    8
Instr1: F    D    E    M    W
Instr2:      F    D    E    M    W
Instr3:           F    D    E    M    W
Instr4:                F    D    E    M    W

F = Fetch, D = Decode, E = Execute, M = Memory, W = Write-back
```

### Clock Speed vs. Instructions Per Cycle

A CPU's performance depends on three factors:

```
Performance = Clock Speed × Instructions Per Cycle (IPC)
```

A 4 GHz CPU with low IPC may be slower than a 3 GHz CPU with high IPC. This is why raw GHz comparisons between different architectures (e.g., Intel vs. Apple Silicon) are misleading.

## Memory Hierarchy

Not all memory is created equal. The faster the memory, the more expensive it is per byte. Computers use a **hierarchy** to balance speed and cost:

```
           REGISTERS          ─ ─ ─ ◄── 1 ns, ~1 KB
         ┌──────────┐              ▲
         │   L1     │              │    Faster
         │  CACHE   │  ─ ─ ─ ◄── 2 ns, ~128 KB
     ┌───┴──────────┴───┐          │    but
     │      L2 CACHE    │  ─── ◄── 5 ns, ~1 MB
 ┌───┴──────────────────┴───┐      │    more
 │        L3 CACHE          │  ─── ◄── 15 ns, ~16 MB
 │  (shared across cores)   │      │    expensive
 ├──────────────────────────┤      │
 │          RAM             │  ─── ◄── 50-100 ns, ~32 GB
 │      (Main Memory)      │      │
 ├──────────────────────────┤      │
 │       SSD (NVMe)        │  ─── ◄── 10 µs, ~1-4 TB
 ├──────────────────────────┤      │
 │       HDD (Disk)        │  ─── ◄── 10 ms, ~20 TB
 └──────────────────────────┘      ▼
                               Slower
                               but
                               cheaper per GB
```

### The Principle of Locality

The memory hierarchy works because programs exhibit **locality**:

- **Temporal locality** — if you access a memory location, you are likely to access it again soon (think loop counters).
- **Spatial locality** — if you access a memory location, you are likely to access nearby locations (arrays, sequential code).

The cache exploits this by fetching a **cache line** (typically 64 bytes) whenever the CPU requests any address within that line. The next several sequential accesses hit the cache instead of going to RAM.

### Cache Miss and Hit

```
CPU requests address 0x1008:

┌─────────────┐
│  Is 0x1008  │
│  in L1?     │──── YES ──► L1 HIT  ──► data returned in ~1 ns
└──────┬──────┘
       │ NO
       ▼
┌─────────────┐
│  Is 0x1008  │
│  in L2?     │──── YES ──► L2 HIT  ──► data returned in ~5 ns
└──────┬──────┘
       │ NO
       ▼
┌─────────────┐
│  Is 0x1008  │
│  in L3?     │──── YES ──► L3 HIT  ──► data returned in ~15 ns
└──────┬──────┘
       │ NO (cache miss — worst case)
       ▼
  Fetch from RAM ────────────────────────► data returned in ~80 ns
```

A cache miss is expensive — accessing RAM can take 100x longer than accessing L1 cache. This is why optimizing for cache locality is one of the most impactful performance techniques in software.

### Virtual Memory

The operating system abstracts physical RAM into **virtual memory**. Each process gets its own address space (e.g., 2⁶⁴ bytes on a 64-bit system). The CPU's **Memory Management Unit (MMU)** translates virtual addresses to physical addresses using page tables:

```
Process A's view:         Physical RAM:
  0x0000 ─────────┐
                   ├────────► 0x1000  (page frame)
  0x4000 ──────┐  │
                │  └────────► 0x5000
                │
                └───────────► Disk (swapped out)
```

Pages that are not actively used can be **swapped** to disk (paging), letting the system run programs that need more memory than physically available.

## Storage: How Data Persists

RAM is volatile — it forgets everything when power is lost. For permanent storage, computers use:

### Hard Disk Drive (HDD)

A spinning magnetic platter with a read/write head on an arm. Data is stored in concentric tracks divided into sectors.

```
       ┌────────────────────────┐
       │     Spinning platter   │
       │   ┌──┐                 │
       │   │  │  Track          │
       │   │  ├─────┐           │
       │   │  │Sect │           │
       │   │  └─────┘           │
       │   └──┘                 │
       │    ▲                   │
       │  Read/write head       │
       └────────────────────────┘
```

- **Speed**: ~10 ms seek time (moving the head mechanically is slow)
- **Cost**: ~$15/TB, very cheap
- **Durability**: Fragile (moving parts), prone to failure if dropped
- **When to use**: Bulk storage, backups, media archives

### Solid State Drive (SSD)

Uses NAND flash memory — no moving parts. Data is stored in floating-gate transistors that trap electrons.

- **Speed**: ~50 µs read, ~100 µs write — about 100x faster than HDD
- **Cost**: ~$80/TB, more expensive than HDD but much faster
- **Durability**: No moving parts, resistant to shock
- **Wear**: Flash cells wear out after ~1000–100,000 write cycles (managed by the SSD controller)
- **When to use**: Operating system, applications, active projects

### How Files Are Organized

Storage devices are formatted with a **filesystem** (NTFS, ext4, APFS). The filesystem maintains a hierarchical structure of directories and files, tracking:

- **File name and metadata** (size, timestamps, permissions)
- **Which disk blocks belong to which file**
- **Free space** available

When you save a file, the OS does not necessarily write it to contiguous blocks. Fragmented files are slower to read, which is why defragmentation helps HDDs (SSDs handle fragmentation transparently).

## Input / Output

The CPU communicates with peripherals through **I/O controllers** and **interrupts**.

### Memory-Mapped I/O

Most modern systems use memory-mapped I/O. A portion of the address space is reserved for device controllers. Reading or writing those addresses talks to the device instead of RAM:

```
┌──────────────────────────────────┐
│         Address Space            │
│                                  │
│  0x0000_0000  ┌──────────────┐   │
│               │    RAM        │   │
│  0xF000_0000  ├──────────────┤   │
│               │  Keyboard    │   │
│  0xF000_1000  ├──────────────┤   │
│               │  GPU (video) │   │
│  0xF100_0000  ├──────────────┤   │
│               │  Disk Ctrl   │   │
│              ...             │   │
└──────────────────────────────────┘
```

### Interrupts

Instead of the CPU constantly checking whether a key was pressed (polling), devices send **interrupts**. When the keyboard sends a byte, it raises an interrupt line. The CPU:

1. Completes the current instruction
2. Saves its state (registers, PC)
3. Jumps to the **interrupt handler** (a small OS routine)
4. Reads the key from the keyboard controller
5. Restores state and resumes the interrupted program

```
   Time ─────────────────────────────────────────▶

   CPU:  [instr] [instr] [instr] [instr]
                          │
   Keyboard:              │ key pressed
                          ▼
                     Interrupt!
                          │
   CPU:                         [save] [handler] [restore] [instr]
```

Interrupts are essential for responsiveness. Without them, the CPU would have to constantly poll every device, wasting billions of cycles.

### Direct Memory Access (DMA)

For large transfers (disk reads, network packets), letting the CPU copy every byte is wasteful. DMA controllers move data directly between a device and RAM without CPU involvement:

```
Without DMA:          With DMA:
CPU reads from disk   CPU tells DMA: "copy 4 KB from disk to 0x1000"
  to register          └─ CPU is free to run other code
CPU writes register      │
  to RAM                 │
CPU reads from disk      │  DMA controller copies data
  to register             │   directly to RAM
CPU writes register       │
  to RAM                  │
... (wastes CPU time)    ◄── DMA interrupts CPU when done
```

## The Operating System as Hardware Manager

The operating system (Linux, Windows, macOS) is the software layer that manages hardware and provides services to applications. Its core responsibilities include:

```
┌─────────────────────────────────────────────────┐
│                 APPLICATIONS                     │
│  Browser  │  Editor  │  Game   │  Terminal      │
├─────────────────────────────────────────────────┤
│            OPERATING SYSTEM                      │
│  ┌────────┐ ┌────────┐ ┌────────┐              │
│  │Process │ │Memory  │ │Device  │   ┌────────┐│
│  │Manager│ │Manager │ │Drivers │   │File    ││
│  │ (sched)│ │ (VM)   │ │(I/O)   │   │System  ││
│  └────────┘ └────────┘ └────────┘   └────────┘│
│  ┌──────────────────────────────────────────┐   │
│  │               Kernel                     │   │
│  └──────────────────────────────────────────┘   │
├─────────────────────────────────────────────────┤
│                HARDWARE                          │
│  CPU  │  RAM  │  Disk  │  GPU  │  Network       │
└─────────────────────────────────────────────────┘
```

### Process Management

A **process** is a running program with its own virtual address space, open files, and execution state. The OS scheduler decides which process runs on the CPU at any given moment.

```
Time slice (typically ~10-100 ms):

Process A:  ████████░░░░░░░░░░░░████████░░░░░░
Process B:  ░░░░░░████████░░░░░░░░░░░░████████
Process C:  ░░░░░░░░░░░░████████░░░░░░░░░░░░░░

           ◄─── Context switch ──►
```

The scheduler uses algorithms like Completely Fair Scheduler (Linux) or Round Robin to give each process its fair share.

#### States of a Process

```
      ┌────────────┐
      │    NEW     │
      └─────┬──────┘
            │ admitted
            ▼
      ┌────────────┐    scheduler    ┌──────────┐
      │   READY    │◄───────────────│ RUNNING  │
      └─────┬──────┘    dispatch     └────┬─────┘
            │                             │
            │ I/O or event    I/O complete│
            │   wait           or event   │
            ▼                             │
      ┌────────────┐                      │
      │  WAITING   │──────────────────────┘
      └────────────┘
            │
            │ exit
            ▼
      ┌────────────┐
      │ TERMINATED │
      └────────────┘
```

### Memory Management

The OS's memory manager:
- Allocates and deallocates RAM for processes
- Translates virtual addresses to physical addresses (via page tables)
- Handles swapping (moving unused pages to disk)
- Enforces protection — one process cannot read another process's memory

### Device Drivers

A **device driver** is a kernel module that knows how to talk to a specific hardware device. It translates generic OS commands (read sector, send packet) into device-specific register writes. This is why you need a driver for your GPU or network card.

### System Calls

Applications cannot access hardware directly — the kernel enforces this via **protection rings** (Ring 0 for kernel, Ring 3 for user programs). When an app needs to read a file, it calls a **system call** (e.g., `read()` in Linux):

```
User app:           │  Kernel:
printf("hello");    │
  └─► write(1, ...) │
      └─► trap      │──► syscall handler
                    │    validate arguments
                    │    switch to kernel stack
                    │    call device driver
                    │    copy data to user buffer
                    │◄── return to user mode
```

This transition is called a **context switch** and is relatively expensive (microseconds), which is why batching system calls (reading a 4 KB block instead of one byte at a time) is important for performance.

## The Boot Process

When you press the power button, the computer is a lifeless collection of circuits. Here is how it springs to life:

```
┌──────────────────────────────────────────────────┐
│  1. POWER ON                                      │
│     Power supply signals motherboard: "power OK" │
├──────────────────────────────────────────────────┤
│  2. RESET VECTOR                                  │
│     CPU resets PC to a fixed address              │
│     (e.g., 0xFFFFFFF0 on x86)                    │
│     This address maps to ROM (BIOS/UEFI)          │
├──────────────────────────────────────────────────┤
│  3. BIOS / UEFI POST                              │
│     Power-On Self-Test: check CPU, RAM, devices   │
│     Initialize the chipset and basic hardware     │
├──────────────────────────────────────────────────┤
│  4. BOOTLOADER SEARCH                             │
│     UEFI scans configured boot devices (SSD, USB) │
│     for a valid GPT partition with EFI System     │
│     Partition. Reads the bootloader (e.g., GRUB). │
│     (Legacy BIOS: reads first 512 bytes = MBR)   │
├──────────────────────────────────────────────────┤
│  5. BOOTLOADER LOADS                              │
│     GRUB reads its config (grub.cfg)              │
│     Presents menu (or auto-selects)               │
│     Loads the kernel and initrd into memory       │
│     Jumps to kernel entry point                   │
├──────────────────────────────────────────────────┤
│  6. KERNEL INITIALIZATION                         │
│     ┌────────────────────────────────────────┐    │
│     │ - Set up page tables, MMU, virtual mem │    │
│     │ - Initialize interrupt controller      │    │
│     │ - Set up scheduler                     │    │
│     │ - Mount root filesystem                │    │
│     │ - Load essential drivers (initrd)      │    │
│     │ - Start init process (PID 1)           │    │
│     └────────────────────────────────────────┘    │
├──────────────────────────────────────────────────┤
│  7. INIT / SYSTEMD                                │
│     Starts system services:                       │
│     - udev (device manager)                       │
│     - NetworkManager                              │
│     - Display server (X11 or Wayland)             │
│     - Login manager                               │
├──────────────────────────────────────────────────┤
│  8. LOGIN SCREEN                                  │
│     User enters credentials                       │
│     Desktop environment starts (GNOME, KDE)       │
└──────────────────────────────────────────────────┘
```

## Putting It All Together: Opening a Text Editor

Let's trace what happens when you double-click a text editor icon — from your finger to the rendered window.

### Phase 1: Input Detection (millisecond 0–10)

```
1. Your mouse button pushes a mechanical switch.
2. The mouse controller sends a USB HID packet:
   ┌────────────┬──────────┬────────┐
   │ Button: 1  │ X-move:0 │ Y-move:0│
   └────────────┴──────────┴────────┘
3. USB host controller receives packet.
4. Interrupt sent to CPU.
5. CPU interrupts the running process.
6. Mouse driver reads the packet.
7. Driver determines which window is under cursor.
8. Desktop environment (DE) receives "double-click"
   event on the editor icon.
```

### Phase 2: Process Creation (millisecond 10–50)

```
9. DE's file manager calls fork() + exec().
10. OS kernel allocates a process descriptor,
    assigns PID 4821, creates virtual address space,
    reads the ELF binary from disk, maps its .text
    section and shared libraries (libc, GTK/Qt)
    into memory, and sets PC to the entry point.
11. Scheduler places the new process on the ready queue.
```

### Phase 3: Loading Libraries and Initialization (millisecond 50–200)

```
12. Dynamic linker resolves shared library symbols.
13. C runtime initializes malloc, TLS, and global ctors.
14. App reads config from ~/.config/editor/config.json:
    kernel translates path → inode → DMA from SSD → buffer.
```

### Phase 4: GUI Initialization (millisecond 200–500)

```
17. App toolkit opens a Unix socket connection to
    the display server (X11/Wayland).
18. Display server allocates a framebuffer in GPU memory.
19. App creates a 1200×800 window with title "Untitled — Editor".
20. GPU scans out the framebuffer 60+ times per second
    to your monitor.
```

### Phase 5: Ready for Input (millisecond 500–1000)

```
23. Editor enters its event loop — wait_for_event() blocks.
24. The OS schedules ~20-50 processes, context-switching
    hundreds of times per second.
25. Each keystroke follows: key → USB → interrupt →
    driver → display server → editor → GPU redraw.
26. Between input, the CPU idles in deep sleep (C-states)
    waiting for the next interrupt.
```

The entire process — from double-click to visible window — takes roughly 500–1000 milliseconds. The vast majority of that time is spent loading the binary and libraries from storage into memory.

## Learning Tips

- **Build a mental model of the stack.** Know which level you are debugging at: is the problem in your code, the OS, the driver, or the hardware?
- **Use system monitoring tools.** `htop`, `perf`, `iostat`, and `lsof` reveal what the OS is actually doing. Run `htop` and watch the process list; run `perf stat ./your_program` to see cache misses and context switches.
- **Write small programs that measure.** Time a loop that accesses memory sequentially vs. randomly — the difference will shock you and make cache locality concrete.
- **Study one layer at a time.** It is easy to be overwhelmed. Start with binary and the CPU, then add memory, then the OS. Each layer builds on the previous one.
- **Read the Linux kernel docs.** `Documentation/admin-guide/` and `Documentation/scheduler/` are surprisingly accessible.
- **Try a digital logic simulator.** Tools like Logisim let you build circuits from individual logic gates, making the transistor→CPU connection tangible.

## Glossary

| Term | Definition |
|------|------------|
| ALU | Arithmetic Logic Unit — the part of the CPU that performs math and logical operations |
| Binary | Base-2 number system using only 0 and 1 |
| Bit | A single binary digit (0 or 1) |
| Bootloader | A small program that loads the operating system kernel into memory during startup |
| Bus | A communication system that transfers data between computer components |
| Byte | 8 bits, the fundamental unit of data storage |
| Cache | A small, fast memory that stores copies of frequently accessed data from slower memory |
| Cache line | The smallest unit of data transferred between cache and memory (typically 64 bytes) |
| Cache miss | A failed lookup in a cache level, requiring access to a slower level |
| Clock cycle | One tick of the CPU's internal clock, during which a basic operation step occurs |
| Context switch | Saving and restoring the state of a process when the CPU switches between processes |
| Control Unit | The CPU component that decodes instructions and coordinates execution |
| DMA | Direct Memory Access — hardware that transfers data between devices and RAM without CPU involvement |
| Driver | A kernel module that communicates with a specific hardware device |
| Fetch-execute cycle | The fundamental loop in which the CPU reads, decodes, and executes instructions |
| Filesystem | The structure and rules for naming, storing, and organizing files on a storage device |
| Hexadecimal | Base-16 number system (0–9, A–F), used as a shorthand for binary |
| HDD | Hard Disk Drive — magnetic spinning-platter storage |
| IEEE 754 | Standard for floating-point arithmetic in binary |
| Interrupt | A signal from hardware to the CPU indicating an event needs attention |
| IPC | Instructions Per Cycle — a measure of CPU efficiency |
| Kernel | The core of the operating system, managing hardware and system resources |
| Locality | The tendency of programs to access the same or nearby memory locations repeatedly |
| MMU | Memory Management Unit — hardware that translates virtual to physical addresses |
| Opcode | The part of a machine instruction that specifies the operation to perform |
| Page | A fixed-size block of virtual memory (typically 4 KB on most systems) |
| Paging | Moving pages between RAM and disk to make virtual memory available beyond physical RAM |
| PC | Program Counter — register holding the address of the next instruction |
| Pipeline | A CPU design technique where multiple instruction stages overlap |
| POST | Power-On Self-Test — the initial diagnostic check performed by firmware during boot |
| Process | A running program with its own address space, state, and resources |
| RAM | Random Access Memory — volatile main memory for active data |
| Register | An ultra-fast storage location inside the CPU |
| Scheduler | The OS component that decides which process runs when and for how long |
| SSD | Solid State Drive — flash-based storage with no moving parts |
| System call | A request from user space to the kernel for a privileged operation |
| Transistor | An electronic switch that can be ON or OFF — the fundamental building block of all digital circuits |
| Two's complement | The standard way negative integers are represented in binary |
| UEFI | Unified Extensible Firmware Interface — modern replacement for BIOS |
| UTF-8 | A variable-width character encoding for Unicode, backward-compatible with ASCII |
| Virtual memory | An abstraction that gives each process its own address space, mapped to physical memory by the MMU |

## Quick References

- [Computer Science from the Bottom Up](https://www.bottomupcs.com/) — a free online book covering computer architecture and operating systems from the hardware up
- [Ben Eater's 8-bit Computer](https://eater.net/8bit) — build a CPU from breadboard logic gates (YouTube series)
- [The Linux Kernel Documentation](https://www.kernel.org/doc/html/latest/) — official docs for the world's most studied kernel
- [Godbolt Compiler Explorer](https://godbolt.org/) — see how C/C++ code compiles to assembly, making the fetch-execute cycle observable
- [Latency Numbers Every Programmer Should Know](https://gist.github.com/hellerbarde/2843375) — comparable latency table for CPU, RAM, disk, network
- [OSDev Wiki](https://wiki.osdev.org/) — practical reference for boot process, memory management, and system programming

## Next Steps

- [Command Line Basics](command-line-basics.md) — navigate the filesystem, run programs, and control your computer through the terminal
- Back to [Computer Science Introduction](../intro/index.md)

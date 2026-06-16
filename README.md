# Reverse Engineering Study Notebook

> Curated study notebook focused on Reverse Engineering, Computer Architecture, C Programming, and x86/x64 Assembly.

## Overview

This project was created as part of a study challenge focused on understanding how software works internally, from high-level C code down to machine instructions executed by the processor.

The notebook centralizes references, study notes, reverse engineering workflows, troubleshooting experiences, and reusable prompts for practical analysis using industry-standard tools.

---

## Study Objectives

- Understand how C code is translated into Assembly.
- Analyze binary execution flow and program internals.
- Learn processor architecture concepts such as:
  - Registers
  - Stack and Heap
  - Calling Conventions
  - Syscalls
- Reverse engineer compiled software.
- Identify compiler optimizations and their impact on binary analysis.
- Develop practical skills with professional reverse engineering tools.

---

## Technologies & Tools

| Category | Tools |
|-----------|---------|
| Reverse Engineering | Ghidra, IDA Free, Cutter |
| Debugging | GDB |
| Programming Languages | C, Assembly (x86/x64) |
| Operating System | Linux |
| Documentation | NotebookLM |

---

## Knowledge Sources

### Computer Systems: A Programmer's Perspective (CS:APP)

Author: Randal E. Bryant & David R. O'Hallaron

Provides a deep understanding of the interaction between hardware and software, memory organization, machine-level programming, and computer systems.

### The Art of Assembly Language

Author: Randall Hyde

Comprehensive guide for understanding processor architecture and Assembly programming concepts.

### Linux System Programming

Author: Robert Love

Focuses on Linux internals, system calls, kernel interaction, process management, and low-level programming.

---

## Reverse Engineering Analysis Framework

### Master Prompt

```text
Explain the internal behavior of the program step by step, identifying execution flow, function calls, data structures, memory usage, registers, calling conventions, algorithms, and possible compiler optimizations.

Provide practical examples in both C and x86/x64 Assembly whenever applicable.

Include ASCII diagrams when useful and explain how to reproduce the analysis using Ghidra, IDA Free, Cutter, and GDB.

When encountering unknown functions, formulate evidence-based hypotheses and explain the reasoning process used to reach conclusions.

The objective is deep reverse engineering understanding and internal software analysis.
```

---

## Troubleshooting & Lessons Learned

### Understanding `_start` vs `main`

#### Challenge

Distinguishing the role of the binary entry point (`_start`) from the application's `main()` function.

#### Key Learning

`_start` is the true entry point of an executable.

Before `main()` is executed, the runtime environment is initialized by the C Runtime Startup (CRT), which prepares memory structures and program state.

#### Insight

Monitoring register usage—especially `RAX` during syscalls—often reveals functionality hidden by compiler optimizations.

---

## Core Concepts

### Compilation

C source code is translated into Assembly instructions before becoming machine code.

Variables may become:

- CPU registers
- Stack locations
- Static memory addresses

### System Calls (Syscalls)

Direct communication channel between user-space applications and the operating system kernel.

Example registers on Linux x64:

| Register | Purpose |
|-----------|----------|
| RAX | Syscall number |
| RDI | Argument 1 |
| RSI | Argument 2 |
| RDX | Argument 3 |

### Compiler Optimization

Optimization levels such as:

```bash
-O0
-O1
-O2
-O3
```

can significantly alter generated Assembly code, often removing redundant operations and restructuring control flow.

---

## Glossary

### Syscall

Interface between a user-space application and the operating system kernel.

### Registers

Ultra-fast storage locations inside the CPU.

Examples:

- RAX
- RBX
- RCX
- RDX
- RDI
- RSI

### Calling Convention

Rules that define how functions receive parameters and return values.

Linux x64 argument order:

```text
RDI → Argument 1
RSI → Argument 2
RDX → Argument 3
RCX → Argument 4
R8  → Argument 5
R9  → Argument 6
```

### `.data` Section

Binary section dedicated to initialized global and static variables.

---

## Reusable Reverse Engineering Prompts

### Function Reconstruction

```text
Analyze the Assembly code below and reconstruct the equivalent C function signature. Identify parameter passing mechanisms and return values.
```

### Syscall Analysis

```text
Explain the difference between a function call (call) and a system call (syscall) in the following code snippet.
```

### Binary Protection Detection

```text
Identify indicators of software protection mechanisms or obfuscation techniques, including anti-debugging patterns.
```

---

## Reproducing Analyses

### GDB

Start analysis from the program entry point:

```bash
break _start
run
stepi
```

Useful commands:

```bash
info registers
x/20i $rip
layout asm
```

### Ghidra

1. Import the binary.
2. Run Auto Analysis.
3. Inspect the Control Flow Graph.
4. Analyze functions and decompiled output.
5. Correlate Assembly with pseudocode.

---

## Study Recommendation

Compile the same C program using different optimization levels:

```bash
gcc program.c -O0
gcc program.c -O2
gcc program.c -O3
```

Compare the generated Assembly and observe how compiler optimizations affect:

- Control flow
- Register usage
- Function inlining
- Memory access patterns

This exercise provides one of the fastest paths toward mastering reverse engineering fundamentals.

---

## Project Goal

Build a solid foundation in Reverse Engineering by understanding software behavior from source code to machine instructions, enabling deeper analysis of binaries, operating systems, and security-related technologies.

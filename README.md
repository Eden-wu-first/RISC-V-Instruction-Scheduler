# RISC-V Instruction List Scheduler (CSC3060 - Spring 2026)

## 📖 Project Overview
This is a course project for **CSC3060: Introduction to Computer Systems** at **CUHK-Shenzhen**. Please do not submit it as your own assignment.

This project implements a **List Scheduling** algorithm for RISC-V assembly code. The goal is to reorder instructions to minimize pipeline stalls by understanding data dependencies (RAW, WAR, WAW) and instruction latencies. It is built upon a minimal C++ framework provided by the course.

For a full description of the project, see the project handout at [csc3060_spring2026_project3_cs.pdf](./csc3060_spring2026_project3_cs.pdf)

## 🚀 Key Features

*   **Instruction Abstraction**: Models RISC-V instructions with functions to check register usage (`usesReg`, `definesReg`).
*   **Static Latency Model**: Accurately assigns execution latencies to instructions based on a lookup table (Load: 5 cycles, Division: 10 cycles, etc.).
*   **Dependency Graph (DAG) Builder**: Constructs a Directed Acyclic Graph by identifying **RAW** (Read-After-Write), **WAR** (Write-After-Read), and **WAW** (Write-After-Write) dependencies.
*   **List Scheduler**: Implements the core algorithm using priority queues. It schedules instructions cycle-by-cycle, distinguishing between data-ready nodes (RAW) and order-only constraints (WAR/WAW).
*   **Critical Path Priority**: Assigns priority to nodes based on their **Critical Path Length** (CPL), ensuring long-latency operations are started early.
*   **Tie-Breaking Policies**: Supports `SMALLER_INDEX` (default), `MOST_CHILD` (unblocks more instructions), and `LPT` (Longest Processing Time) policies.
*   **Bonus Optimization (`scheduleFast`)**: An optimized version using `std::priority_queue` that significantly outperforms the naive O(n²) approach by achieving **O(n log n)** complexity.

## 🛠️ Setup

### Environment

- CMake 3.14 or higher version
- Compiler should support C++17 (GCC 7+, Clang 5+, MSVC 2017+)

```sh
mkdir build
cd build
cmake ..
make -j4
```

### Run unit tests

```sh
cd build
ctest                        # run all test
ctest -R Latency             # pattern matching "Latency"
ctest -V                     # verbose output
ctest -j4                    # parallel execution
```

## 📂 Project Structure
- `src/`: Contains all C++ source files (Instruction.cpp, DAGBuilder.cpp, Scheduler.cpp, etc.).
- `tests/`: Unit tests for the instruction, DAG builder, and scheduler components.
- `CMakeLists.txt`: The build configuration file.
- `report.pdf`: My detailed analysis and results of the scheduling process (comparing original vs. scheduled orders).
- `csc3060_spring2026_project3_cs.pdf`: Official assignment requirements and specifications.

## 📊 Results Analysis (from Report)
The scheduler demonstrates significant performance improvements in scenarios with load-use hazards and long-latency instructions:
- Example 3 (Load-Use Hazard Hidden): Performance improved by 17.6% by grouping lw instructions together.
- Example 4 (Early Start Critical Operation): Performance improved by 23.8% by prioritizing the long-latency div instruction.

## 📝 Academic Integrity Declaration
As required by the course policy (6.3), I utilized AI tools (like Copilot and LLMs) for:
- Identifying and fixing code logic errors (e.g., infinite loops, incorrect condition checks).
- Iterative priority computation logic in `scheduleFast()`.
- Translating and polishing the written report.

Links to LLM conversations are included in the final report submission.

## 📄 License
This project is open-sourced under the MIT License — you can freely use, modify and distribute it with copyright notice.

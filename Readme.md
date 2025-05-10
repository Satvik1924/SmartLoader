# 🧠 SimpleSmartLoader

**SimpleSmartLoader** is a lightweight, educational implementation of a *lazy-loading executable loader* for 32-bit ELF binaries. Unlike traditional loaders that load all segments upfront, SimpleSmartLoader mimics demand paging by loading segments into memory only when accessed during execution — similar to how modern operating systems like Linux handle memory via page faults.

> ⚠️ **Note:** This loader is built purely for learning purposes. It is intended to work on Linux with statically linked 32-bit ELF executables and does not support dynamic linking (no glibc references).

---

## 📌 Key Features

* **Lazy Segment Loading:** Segments are loaded on demand, triggered by segmentation faults.
* **Signal Handling:** Captures `SIGSEGV` and resolves it as a simulated page fault.
* **Memory Mapping:** Uses `mmap` to allocate memory dynamically for segments at runtime.
* **Performance Reporting:** At the end of execution, reports:

  * Total number of page faults
  * Total pages allocated
  * Total internal fragmentation (in KB)

---

## 🛠️ How It Works

1. Loader attempts to jump to the `_start` of the target binary.
2. Since the segment is not loaded yet, a segmentation fault (`SIGSEGV`) occurs.
3. The signal handler catches this and:

   * Identifies the faulting address
   * Locates the corresponding segment from ELF headers
   * Allocates memory using `mmap` (aligned to 4KB pages)
   * Loads content into memory using `lseek` and `read`
4. Execution resumes automatically.
5. At the end of execution, the loader prints statistics for:

   * Total page faults (1 per new segment access)
   * Pages allocated (number of pages mapped)
   * Internal fragmentation (unused bytes in the last page)

---

## 📂 Project Structure

```
📁 SmartLoader/
├── include/
│   └── simple-multithreader.h     # Header file for multithreading support
├── src/
│   ├── loader.cpp                 # Contains SimpleSmartLoader logic
│   └── multithreader.cpp          # Threading utilities (vector/matrix ops)
├── test/
│   ├── vector-test.cpp            # Parallel vector addition test
│   └── matrix-test.cpp            # Parallel matrix multiplication test
├── Makefile
└── README.md                      # This file
```

---

## 🧪 Example Use Cases

### 🧮 Matrix Multiplication

```bash
./matrix-test <num_threads> <matrix_size>
```

Example:

```bash
./matrix-test 4 1024
```

### ➕ Vector Addition

```bash
./vector-test <num_threads> <vector_size>
```

Example:

```bash
./vector-test 8 48000000
```

---

## 🧑‍💻 Contributors

* **Satvik Arya (2023493)**

  * Implemented segmentation fault handler
  * Typecasting and execution flow design
  * Logic to identify faulting address and segment

* **Sanjeev (2023483)**

  * Memory allocation using `mmap`
  * Loading segment content via `lseek` and `read`
  * Metrics tracking: page faults, pages allocated, and fragmentation

---

## 🚀 Getting Started

### Requirements

* Linux system (x86 32-bit support)
* GCC or Clang with pthread support

### Build

```bash
make
```

### Run Tests

```bash
./matrix-test 4 1024
./vector-test 8 48000000
```

---

## 📊 Output Example

```
Execution time of Thread 0 : 0.012345 seconds
Execution time of Thread 1 : 0.011876 seconds
...
Execution time: 0.023899 seconds
Test Success.
```

---

## 📘 License

This project is for academic and personal use only. No warranty or guarantees. Use at your own discretion.

---

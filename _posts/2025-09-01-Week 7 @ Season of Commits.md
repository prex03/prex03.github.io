---
title: Optimizing PyDataStructs - LLVM Optimizations for Bubble Sort
date: 2025-09-01 22:04:55 +0530
categories: [Season of Commits]
tags: [Week 7]
---

This week’s focus was on pushing the LLVM backend for Bubble Sort further by adding optimization passes, generating highly efficient machine code via llvmlite, and benchmarking the results against Python and C++ implementations.

1. Optimized LLVM Bubble Sort with llvmlite

After successfully integrating Bubble Sort into the LLVM backend, the next step was optimization. Using llvmlite’s pass manager, I enabled:

Loop vectorization - improving performance by applying SIMD instructions where possible

SLP vectorization - targeting independent operations within basic blocks

High-level optimizations (O3) - including aggressive inlining and branch prediction improvements

The result is a Bubble Sort implementation that runs hundreds of times faster than the interpreted Python version, and significantly outperforms even the C++ backend.

2. Benchmarking Results

I benchmarked the optimized LLVM Bubble Sort against Python and C++ backends across increasing array sizes. The results are summarized below:

|    |   Array_Size |   Python_Time_s |   CPP_Time_s |   LLVM_Time_s |   CPP_Speedup |   LLVM_Speedup |
|----|--------------|-----------------|--------------|---------------|---------------|----------------|
|  0 |          100 |        0.001673 |     0.001551 |      2.5e-05  |          1.08 |          66.05 |
|  1 |          500 |        0.043857 |     0.042363 |      0.000153 |          1.04 |         286.34 |
|  2 |         1000 |        0.176141 |     0.173289 |      0.000428 |          1.02 |         411.99 |
|  3 |         2000 |        0.723305 |     0.700717 |      0.00132  |          1.03 |         547.87 |
|  4 |         3000 |        1.62648  |     1.57613  |      0.002563 |          1.03 |         634.69 |
|  5 |         4000 |        2.92309  |     2.8723   |      0.004335 |          1.02 |         674.22 |
|  6 |         5000 |        4.5532   |     4.45938  |      0.006694 |          1.02 |         680.2  |

Even at modest input sizes, LLVM achieves >600x speedup compared to Python. The gains increase with larger arrays, confirming that LLVM optimizations scale well.

3. Why LLVM Optimizations Matter

While Bubble Sort is inherently quadratic, LLVM optimizations allow it to:

Exploit modern CPU instruction sets like SSE, AVX, and AVX2

Minimize branch mispredictions via unrolling and speculative execution

Improve cache utilization through memory layout optimizations

This demonstrates that even for a simple algorithm like Bubble Sort, LLVM’s backend can extract significant real-world performance.

**What’s Next?**

With Bubble Sort LLVM-optimized and benchmarked, the next goals are:

a. Extending LLVM optimizations to Quick Sort and Merge Sort

b. Experimenting with adaptive compilation between Python, C++, and LLVM backends

c. Adding target-specific optimizations (e.g., AVX-512 for supported CPUs)

d. Automating benchmark pipelines to continuously compare backends

The gist for the benchmark code and results can be found [here](https://gist.github.com/prex03/c92ebcc8a08806e95cb2f6dcec215681)

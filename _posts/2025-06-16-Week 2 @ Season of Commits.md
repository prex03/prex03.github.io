---
title: Optimizing PyDataStructs - BFS in C++, Benchmarking C++ Graphs, and Smarter Node Storage
date: 2025-06-16 20:48:50 +0530
catrgories: [Season of Commits]
tags: [Week 2]
---

This week was all about smarter algorithms and faster data structures. Continuing my journey with PyDataStructs for Season of Commits with FOSS United, I made key improvements to the graph module by introducing BFS algorithms in C++, extending the benchmarking tool to cover these new backends, and optimizing node storage for better performance.

**What I Worked On -**

1. BFS Algorithm in C++

I implemented the Breadth-First Search (BFS) algorithm for both the Adjacency List and Adjacency Matrix graph backends in C++. This involved writing traversal logic that calls back into Python via function pointers for each visited node. Special care was taken to keep the interface consistent with the Python version, so users don’t need to change their code to benefit from faster traversal.

2. Benchmarking C++ Graphs

The benchmarking framework I built last week now supports the new C++ backends. I modified the benchmarks to include:

Comparisons between C++ and Python implementations of both Adjacency List and Adjacency Matrix graphs
A clear view of where C++ gives us the biggest performance gains
Preliminary results already show significant speedups with C++ backends compared to the Python ones although work is yet to be done to catchup with NewtorkX.

3. Using std::variant for Node Data

Previously, each graph node stored its data as a PyObject*, which required constant interaction with Python’s reference counting system and had performance drawbacks. This week, I transitioned the internal data storage to use std::variant<std::monostate, int, double, std::string> for common data types. This reduces the overhead of working with Python objects while still supporting basic use cases natively in C++.

By avoiding PyObject manipulation unless absolutely necessary, node operations are now faster and more memory efficient.

**What’s Next?**

a. Add DFS implementations in C++

b. Add implementation of minimal spanning algorithms in C++

c. Benchmark the newly implemented algorithms.

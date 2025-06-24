---
title: Optimizing PyDataStructs - Prims in C++, Benchmarking algorithms, and Smarter Node & Edge Storage
date: 2025-06-24 22:37:20 +0530
categories: [Season of Commits]
tags: [Week 3]
---



This week in my Season of Commits journey with FOSS United, I continued refining the performance and extensibility of PyDataStructs' graph module. Building on last week's C++ BFS and benchmarking foundation, I introduced support for richer edge data, implemented Prim's Minimum Spanning Tree algorithm in C++, and pushed the design further with smarter edge storage using std::variant.

What I Worked On –

1. Graph Edge Backend with std::variant

Just like nodes, edges in the C++ backend previously relied on PyObject* to store values. This week, I overhauled that design to use std::variant<std::monostate, int, double, std::string>, allowing edges to natively support common data types without falling back to Python overhead. This simplifies memory management, improves type safety, and boosts performance for most use cases.

The variant system also opens doors for further optimizations and advanced algorithms that depend on numerical edge weights.

2. C++ Implementation of Prim’s Algorithm

I implemented Prim’s algorithm for computing the Minimum Spanning Tree (MST) directly in C++. It operates on the adjacency list backend and benefits from the variant-based edge representation. This required careful handling of edge comparisons, priority queue operations, and efficient graph traversal logic, all in a way that remains interoperable with Python code.


What’s Next?

a. Benchmark all implemented algorithms (BFS, Prim’s) across Python and C++ backends

b. Implement the backend for Kruskal’s Minimum Spanning Tree algorithm in C++

c. Continue refining edge and node interfaces for broader data type support


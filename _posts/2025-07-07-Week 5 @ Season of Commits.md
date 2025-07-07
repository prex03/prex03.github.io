---
title: Optimizing PyDataStructs - Dijkstra’s Algorithm and Graph Performance Fixes
date: 2025-07-07 22:36:20 +0530
categories: [Season of Commits]
tags: [Week 5]
---

This week’s progress on PyDataStructs centered around two major efforts: implementing Dijkstra’s shortest path algorithm in the C++ backend and benchmarking it against NetworkX, followed by an investigation into performance bottlenecks with a focus on node representation in graphs.

**1. C++ Backend Implementation of Dijkstra’s Algorithm**

Following our successful integration of Prim’s algorithm in the C++ backend, this week I added support for Dijkstra’s shortest path algorithm using the adjacency list representation. The goal was to achieve comparable or better performance than NetworkX while maintaining flexibility for arbitrary data in nodes.

The C++ implementation adheres to the structure of our existing Prim’s backend. It leverages a priority queue and efficient C++ data structures, while still interoperating seamlessly with Python through the Python C API. This makes the function accessible from Python while harnessing native-level performance.

**2. Benchmarking Dijkstra’s Algorithm and Performance Analysis**

After implementation, I benchmarked our cpp_adjacency_list version of Dijkstra’s algorithm against NetworkX using randomly generated connected graphs of increasing sizes and densities.

While the C++ backend showed promising results on smaller graphs, we observed that for larger graphs (>1K nodes), the performance didn’t scale as expected. In some cases, our implementation lagged behind NetworkX, something that was not the case with Prim’s.

This prompted a deeper look, and after discussions with my mentor, we deduced a key bottleneck: the use of std::string to represent node identifiers. String comparisons and hash lookups in C++ can become significantly expensive at scale, especially compared to integer-based indexing which libraries like NetworkX internally optimize for.

**What’s Next?**
Next week, I’ll focus on:

a. Refactoring the graph representation to use integer-based node identifiers internally for faster access and comparisons.

b. Re-benchmarking the Dijkstra implementation after this optimization.

c. Generalizing the optimization to apply to other algorithms like BFS and Kruskal that will be implemented soon.

The PR for Dijkstra’s implementation can be found [here](https://github.com/codezonediitj/pydatastructs/pull/686) and the results and code for the benchmark tests can be checked [here](https://gist.github.com/prex03/b8c6d0ec260e87feecf0aa88831f96a8)

---
title: Expanding the C++ Graph Backend - DFS and Bellman-Ford  
date: 2025-10-22 23:25:31 +0530  
categories: [Season of Commits]  
tags: [Week 10]  
---

This week’s progress focused on extending the C++ graph backend in PyDataStructs with two major algorithmic implementations, Depth-First Search (DFS) and the Bellman-Ford shortest path algorithm. The [PR](https://github.com/codezonediitj/pydatastructs/pull/707) builds upon the existing backend architecture established in earlier commits, further accelerating computation on large graphs while maintaining full compatibility with the Python interface.

---

### 1. Adding DFS and Bellman-Ford to the C++ Backend

The primary goal of this update was to move more core graph algorithms from Python to C++ for improved performance and scalability. With this PR, both **DFS** and **Bellman-Ford** are now available in the C++ backend, enabling faster traversal and shortest-path computations, especially on large or dense graphs.

---

### 2. Depth-First Search (DFS) — Iterative, Flexible, and Fast

The C++ DFS implementation mirrors the behavior of the existing Python version while significantly improving runtime performance.  
Key highlights include:

- **Iterative stack-based design** — avoids recursion depth limits  
- **Support for both graph types** — adjacency list and adjacency matrix  
- **Custom callback operations** — users can attach operations (like node labeling or early stopping) via function arguments  
- **Full compatibility** — results and traversal order match the Python backend for consistent testing

This design not only improves efficiency but also provides a framework for extending graph traversal algorithms with user-defined logic, something that will integrate well with future backend expansion (e.g., parallel traversal in LLVM).

---

### 3. Bellman-Ford Algorithm — Optimized with SPFA

The Bellman-Ford shortest path algorithm, implemented as `shortest_paths_bellman_ford_adjacency_list`, was enhanced with the **Shortest Path Faster Algorithm (SPFA)** optimization.  
This approach drastically reduces redundant relaxations on sparse graphs while preserving correctness on graphs with negative weights.

Key improvements include:

- **SPFA optimization** for improved average-case performance  
- **Negative weight and cycle detection**  
- **Consistent output format** — returns distances and predecessors identical to Python’s implementation  
- **Optional target node parameter** for partial shortest-path computation  

These features make the C++ backend robust for a wide range of applications  from general-purpose graph analytics to low-level algorithm benchmarking.

---

### 4. Testing and Validation

This update includes extensive test coverage to ensure correctness and cross-backend consistency.  
Highlights:

- Added dedicated C++ backend tests for DFS and Bellman-Ford  
- Verified behavior for graphs with both positive and negative weights  
- Included cycle detection validation in Bellman-Ford  
- Confirmed callback handling in DFS for custom operations  

All tests across Python and C++ backends pass successfully, ensuring stable integration with the existing codebase.

---

### 5. The Road Ahead

With DFS and Bellman-Ford now integrated, the focus shifts toward:

- **Adding more C++ backend algorithms** — including SCC’s and max flow algorithms  
- **Backend benchmarking suite** — to quantify speedups across graph sizes and structures  

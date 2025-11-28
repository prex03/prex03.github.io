---
title: Completing the C++ Graph Backend 
date: 2025-11-29 22:34:17 +0530  
categories: [Season of Commits]  
tags: [Week 11+12]  
---


This week’s release brings a major milestone for PyDataStructs, the integration of a full-fledged C++ backend for a broad set of graph algorithms. With this, users gain access to high-performance implementations of classic algorithms while retaining the same Python-facing API.

**1. Full C++ Graph Algorithm Backend**

PyDataStructs now supports native C++ implementations of key graph algorithms, including:

a. Kosaraju’s algorithm for strongly connected components (Both Adjacency List and Matrix Versions)

b. Tarjan’s algorithm for strongly connected components (Both Adjacency List and Matrix Versions)

c. Floyd–Warshall algorithm for all-pairs shortest paths (Both Adjacency List and Matrix Versions)

d. Kahn’s algorithm for topological sorting (on DAGs) (Both Adjacency List and Matrix Versions)

e. Edmonds–Karp algorithm and Dinic’s algorithm for maximum flow computations (Adjacency List Version)

Crucially, equivalent unit tests have been added for both adjacency-list and adjacency-matrix graph representations, ensuring correctness across internal graph models. 

**2. Why This Matters**

Performance: Algorithms implemented in C++ immediately benefit from lower-level optimizations, memory control, and elimination of Python overhead  making them suitable for large-scale graphs or performance-sensitive workloads.

Unified API: Despite the backend shift, users need not change their code: the public Python API remains the same. This means a smooth upgrade path while gaining speed.

Backend Foundation for Future Enhancements: With the core C++ backend in place, this opens up PydataStructs to a wide variety of futture optimization strategies, including but not limited to llvm and compiler based speedups.

**3. What Changed in the Codebase**

Pull Request #708 introduces a complete suite of C++ implementations for several classical graph algorithms. Instead of a high-level summary, here is a breakdown of the concrete changes and internal mechanisms added to the backend:

a. Floyd–Warshall for Both Graph Representations

  Two full implementations were added:
  
    all_pair_shortest_paths_floyd_warshall_adjacency_list
    
    all_pair_shortest_paths_floyd_warshall_adjacency_matrix

These functions:

  - Parse Python graph objects through PyArg_ParseTupleAndKeywords
  
  - Construct dist and next dictionaries entirely in C++
  
  - Populate edge weights by examining:
  
        graph->edges (adjacency list backend)
        
        graph->matrix and graph->edge_weights (adjacency matrix backend)
  
  - Run the Floyd–Warshall triple nested loop with careful handling of:
  
  - Missing dictionary entries (PyDict_GetItemString)
  
  - INFINITY for unreachable paths
  
  - Next-hop pointer reconstruction
  
  - Output a Python tuple (dist_dict, next_dict)
  
This implementation mirrors Python semantics exactly while executing the tight inner loops in optimized C++.

b. Kahn’s Topological Sort (List + Matrix)

  Two implementations were added:
  
    topological_sort_kahn_adjacency_list
    
    topological_sort_kahn_adjacency_matrix
  
  Key features include:
  
  - Building in-degree maps directly from the backend node structures
    
  - Using std::deque to hold nodes with zero in-degree
    
  - Mutating the adjacency list or adjacency matrix during processing
    
  - Safely decrementing in-degrees and appending nodes to the result list
    
  - Detecting cycles and raising a Python ValueError if the graph is not a DAG
    
  - Both versions preserve semantics while removing C++ graph edges and cleaning up GraphEdge objects using Py_DECREF.

c. Maximum Flow (Edmonds–Karp and Dinic)

The PR adds both classic algorithms with full Python interop:

  **Edmonds–Karp (Adjacency List)**
  
  BFS is implemented in C++ in breadth_first_search_max_flow_adjacency_list
  
  Tracks:
  
  - parent maps
  
  - Residual capacities using a Python dictionary (flow_passed)
  
  - max_flow_edmonds_karp_adjacency_list repeatedly:
  
  - Calls the BFS routine
  
  - Augments along the discovered path
  
  - Updates residual capacities in Python dictionaries
  
  - Produces the maximum flow as a Python float

  **Dinic’s Algorithm (Adjacency List)**

  Uses the same BFS routine in level graph building mode
  
  A dedicated recursive DFS:
  
  - depth_first_search_max_flow_dinic_adjacency_list
  
  - Computes blocking flows
  
  - Pushes residual updates back into Python dicts
  
  - max_flow_dinic_adjacency_list iterates BFS → DFS phases until exhaustion

This is the first time PyDataStructs has true residual network logic implemented entirely in C++, integrated tightly with Python-accessible state.

d. Extensive Test Additions

The test suite was expanded to verify:

  - Algorithm correctness on both adjacency list and adjacency matrix graphs
  
  - Equality between Python and C++ outputs
  
  - Correct handling of edge cases (cycles, unreachable nodes, zero-capacity edges)
  
  - These tests ensure backend uniformity and prevent future regressions.

**5. What’s Next**

Now that the C++ backend supports a robust collection of algorithms, the upcoming goals are:

a. Benchmarking:
Compare performance against:

  - pure Python implementations
    
  - earlier C++ extensions
    
  - NetworkX equivalents
    
b. Backend diversification:

Building on earlier LLVM work, this backend can now be:

  - JIT-compiled using LLVM
  
  - Potentially parallelized
  
  - Extended to GPU offloading in future releases

c. More algorithm coverage:
With the infrastructure in place, additions like:

  - push-relabel max flow
  
  - Johnson’s APSP
  
  - minimum-cut variants
  
become much easier.

**6. Conclusion**

This [PR](https://github.com/codezonediitj/pydatastructs/pull/708) represents one of a very substantial backend upgrade in PyDataStructs. By implementing SCC algorithms, APSP, topological sort, and two major max-flow algorithms entirely in C++, this PR transforms PyDataStructs into a genuinely high-performance graph processing library.
Users now benefit from:

  - C++-level speed
    
  - A fully preserved Python API
    
  - Identical semantics between adjacency list and matrix backends
    
  - A solid foundation for further optimizations (LLVM, parallelism, GPU)

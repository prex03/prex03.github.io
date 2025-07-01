---
title: Optimizing PyDataStructs - Faster Prim's Algorithm and Smarter Graph Nodes
date: 2025-07-01 18:36:20 +0530
categories: [Season of Commits]
tags: [Week 4]
---

This week’s progress on PyDataStructs centered around two major improvements: enabling arbitrary data storage in graph nodes using Python objects and benchmarking our Prim's algorithm implementation against NetworkX.

**1.  Flexible Graph Nodes with Arbitrary Data**

The graph node structure in the C++ backend was previously limited to storing basic types like integers, doubles, and strings using a std::variant. This week, I expanded this capability to support arbitrary Python objects, making the nodes more flexible for real-world applications that require storing complex metadata or user-defined classes.

This was done by introducing a new DataType::PyObject enum value and adapting the variant and memory management accordingly. It allows users to store any Python object (NumPy arrays, dictionaries, or even custom classes) directly inside the C++ graph node structure, while maintaining compatibility with Python’s reference counting.

**2. Benchmarking Prim’s Algorithm and Beating NetworkX**

I benchmarked our C++-based Prim’s algorithm (cpp_adjacency_list implementation) against NetworkX’s version on randomly generated connected graphs. The performance comparison was conducted across a range of sparse and dense graphs up to 14,000 nodes.

We outperformed NetworkX consistently for graphs up to 10,000 nodes in both sparse and dense configurations with speedups reaching 2.3× faster at certain points. Here's the results for a graph with 0.1 edge probability (~5x10^6 edges in a graph with 10,000 nodes)

|    |   Nodes |   NetworkX_Time_s |   PyDataStructs_cpp_adjacency_list_Time_s |    ratio |
|----|---------|-------------------|-------------------------------------------|----------|
|  0 |     500 |        0.00802446 |                                0.00865229 | 0.927437 |
|  1 |    1000 |        0.0551664  |                                0.0426897  | 1.29227  |
|  2 |    1500 |        0.0973237  |                                0.08409    | 1.15737  |
|  3 |    2000 |        0.205225   |                                0.146714   | 1.39882  |
|  4 |    2500 |        0.388858   |                                0.298837   | 1.30124  |
|  5 |    3000 |        0.705743   |                                0.400346   | 1.76283  |
|  6 |    3500 |        0.872945   |                                0.518429   | 1.68383  |
|  7 |    4000 |        1.1357     |                                0.71299    | 1.59287  |
|  8 |    4500 |        1.5433     |                                0.848275   | 1.81934  |
|  9 |    5000 |        2.12281    |                                1.16875    | 1.81631  |
| 10 |    5500 |        2.30357    |                                1.44926    | 1.58948  |
| 11 |    6000 |        2.70319    |                                1.7022     | 1.58806  |
| 12 |    6500 |        3.39261    |                                2.06868    | 1.63998  |
| 13 |    7000 |        4.1325     |                                2.60368    | 1.58718  |
| 14 |    7500 |        6.9018     |                                2.98203    | 2.31447  |
| 15 |    8000 |        5.29952    |                                3.64075    | 1.45561  |
| 16 |    8500 |        6.40807    |                                4.69019    | 1.36627  |
| 17 |    9000 |        6.35269    |                                5.23698    | 1.21304  |
| 18 |    9500 |        9.18198    |                                6.05257    | 1.51704  |
| 19 |   10000 |       10.0267     |                                8.04014    | 1.24708  |
| 20 |   10500 |        9.33773    |                               15.0429     | 0.620741 |
| 21 |   11000 |       11.0163     |                               17.4937     | 0.629731 |
| 22 |   11500 |       14.0998     |                               44.3853     | 0.317668 |
| 23 |   12000 |       16.58       |                               55.8765     | 0.296726 |
| 24 |   12500 |       13.7712     |                              163.023      | 0.084474 |
| 25 |   13000 |       41.7863     |                              147.44       | 0.283412 |
| 26 |   13500 |       35.6498     |                              275.782      | 0.129268 |
| 27 |   14000 |       53.9506     |                              254.128      | 0.212297 |

The PR which has the changes regarding Prim's implementations can be checked out here (https://github.com/codezonediitj/pydatastructs/pull/685) and the code used to benchmark is availiable here (https://gist.github.com/prex03/7cc12c2f96a71e8077fc25c443bcd6ca)

**What’s Next?**

Next week, I’ll focus on:

a. Investigating performance bottlenecks for large graphs (>10K nodes)

b. Implementing and benchmarking other algorithms in the C++ backend

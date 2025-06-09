---
title: Boosting PyDataStructs Graphs with C++ Backends and Benchmarking Tools 
date: 2025-06-09 20:48:50 +0530
categories: [Season of Commits]
tags: [Week 1]
---

This week has been all about speed and efficiency. As part of my work for Season of Commits with FOSS United, I focused on enhancing the performance of the PyDataStructs library by implementing C++ backends for graph structures and building a benchmarking framework to evaluate performance against networkx.

The goal? To make graph creation and operations faster and more memory efficient without losing the simplicity of Python.

What I Worked On - 

**1. C++ Adjacency List Graph**

I started by implementing a C++ version of the adjacency list graph. Each node maintains a name, associated Python data, and a map of adjacent nodes. I exposed this structure to Python using the Python C API, making sure it behaves just like the existing Python version, but with a significant boost in speed.


**2. C++ Adjacency Matrix Graph**

Next, I tackled the adjacency matrix representation. It's more suited for dense graphs and supports constant-time edge lookup. I used a 2D std::vector matrix and built logic to manage node indices and edge weights efficiently.


**3. Memory Management & Debugging**

This was the tricky part. I had to manually handle reference counting and memory deallocation to avoid leaks and crashes. After a few segmentation faults and a few hours of fprintf(stderr, ...) debugging, everything came together smoothly.


**4. Benchmarking NetworkX vs PyDataStructs**

To showcase the performance gains, I built a benchmarking framework to compare:

a. Graph creation time

b. Memory usage

c. Performance on standard models like Erdős-Rényi, Barabási-Albert, and Watts-Strogatz

Early results are indicative of the work that needs to be done: PyDataStructs with pure python implementation is outperformed by networkx in almost all graph types. 


**What’s Next?**

a. Implement DFS and BFS with C++ backends to take advantage of the backend speed
b. Polish the benchmarking framework to compare results of the newly added C++ backends against NetworkX.

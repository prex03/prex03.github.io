---
title: Optimizing PyDataStructs - Shift to Meson Build
date: 2025-10-12 23:05:17 +0530
categories: [Season of Commits]
tags: [Week 9]
---

This week’s update focused on adding the Meson-based build process in PyDataStructs and improving type handling within the graph backend. The [PR](https://github.com/codezonediitj/pydatastructs/pull/696) builds upon PR #689, which introduced the Meson build system and laid the groundwork for the C++ graph backend.

1. Python 3.9+ Becomes the New Baseline

As part of ongoing modernization, the minimum supported Python version for PyDataStructs was raised to 3.9.
Python 3.8’s handling of collections.abc imports caused compatibility issues with several external dependencies when using Meson. By moving to 3.9+, the library benefits from a more stable standard library, better typing features (like typing.Protocol and enhanced generics), and broader third-party package support, all essential for future backend extensions like LLVM integration.

2. Why Meson Over Setuptools

The migration from setuptools to Meson was a deliberate move aimed at improving build performance, modularity, and C++ integration. Unlike setuptools, which primarily targets Python packaging, Meson is designed as a cross-language build system with first-class support for C and C++ projects.

For PyDataStructs, which now includes extensive C++ backend code, Meson offers:

a. Faster incremental builds and better dependency tracking

b. Cleaner integration with compiled extensions (C++, LLVM, and future GPU modules)

c. Unified configuration across Python and native components, reducing setup complexity

d. Cross-platform consistency, especially for developers working on macOS, Linux, and Windows

This shift ensures that PyDataStructs’ build pipeline is robust enough to handle increasingly complex backends while remaining developer-friendly.

3. Fixing TypeErrors in Graph Node Tests

This PR also addressed a TypeError encountered when testing C++ graph nodes under Meson. The issue stemmed from inconsistencies in how Python and C++ objects were mapped within the compiled module. The fix ensures stable interoperation between layers, paving the way for seamless backend extensibility — particularly as the LLVM graph backend matures.

4. The Road Ahead

With Meson now stabilized and Python 3.9 as the new baseline, the next development steps include:

a. Adding functions to the graph C++ backend

b. Benchmark them and look for areas which could benifit from being delegated to the LLVM backend

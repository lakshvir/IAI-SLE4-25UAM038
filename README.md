# IAI-SLE4-25UAM038
README.md
BFS vs DFS Graph Search — Architecture Decision Record (ADR)
Author: Lakshvir
Roll Number: 25UAM038
Course: Introduction to Artificial Intelligence (IAI)
Project: SLE-4 — Architecture Decision Record

📌 Overview
This repository contains the Architecture Decision Record (ADR) for selecting a primary graph search algorithm — Breadth-First Search (BFS) vs Depth-First Search (DFS) — based on empirical profiling performed in SLE-2, with system design from SLE-3.

The decision documented here recommends BFS as the preferred search algorithm for the tested scenario: a complete binary tree with 65,535 nodes (depth 15) and a fixed target node 7000.

🎯 Decision Summary
Selected Algorithm: Breadth-First Search (BFS)

Why?

BFS expanded 7,000 nodes vs DFS's 46,471 nodes (~6.6× fewer).

BFS averaged 2.456848 ms vs DFS's 16.960892 ms (~6.9× faster).

Both algorithms successfully located target node 7000.

BFS's level-order traversal reaches the shallow target efficiently.

⚠️ This conclusion is specific to the tested tree, target node, implementation, and environment. It does not claim BFS is universally superior to DFS.

📊 Profiling Results (SLE-2)
Metric	BFS	DFS
Nodes Expanded	7,000	46,471
Avg. Execution Time (100 runs)	2.456848 ms	16.960892 ms
Target Found	✅ 7000	✅ 7000
Tree Size	65,535 nodes	65,535 nodes
Tree Depth	15	15
Node efficiency: BFS ≈ 6.6× better
Time efficiency: BFS ≈ 6.9× better

🏗️ System Architecture (SLE-3 — C4 Model)
The C4 architecture separates concerns into five containers:

text
┌─────────────────┐
│  Input Module   │  → parses user configuration
└────────┬────────┘
         │
┌────────▼────────┐
│   Tree Model    │  → generates 65,535-node binary tree
└────────┬────────┘
         │
┌────────▼────────┐
│  Search Engine  │  → Algorithm Selector → BFS / DFS Traverser
└────────┬────────┘
         │
┌────────▼────────────┐
│ Profiling Controller│  → measures time & node counts
└────────┬────────────┘
         │
┌────────▼────────┐
│  Output Module  │
└─────────────────┘
The modular design allows clean swapping between BFS and DFS, enabling fair, identical-condition profiling.

✅ Consequences
Positive
Lower average execution time (2.46 ms vs 16.96 ms).

Fewer nodes expanded (7,000 vs 46,471).

Guarantees shortest path in unweighted graphs.

Negative / Trade-offs
BFS queue memory can grow large at deep levels.

Performance depends on target location — DFS may win for deep targets.

More experiments needed across tree structures and target positions.

🔗 Related Repositories
SLE	Repository
SLE-1	IAI-SLE1-25UAM038
SLE-2	SLE-2-BFS-vs-DFS
SLE-3	IAI-SLE3-25UAM038
🧭 SLE Journey Summary
SLE-1: Built a PEAS-based AI agent for a Modern Autonomous Delivery Drone; implemented logic in Python.

SLE-2: Profiled BFS and DFS on a 65,535-node complete binary tree with target 7000 over 100 runs.

SLE-3: Designed the C4 architecture (Context, Container, Component, Code) for the BFS vs DFS system.

SLE-4: Produced this ADR selecting BFS for the tested scenario, grounded in empirical data.

🎓 Viva Quick Notes
Why BFS over DFS? Fewer nodes expanded (6.6×), faster (6.9×).

Alternatives? DFS rejected; A* not tested.

Profiling? BFS: 7,000 nodes / 2.456848 ms; DFS: 46,471 nodes / 16.960892 ms.

C4 Containers? Input, Tree Model, Search Engine, Profiling Controller, Output.

Larger problems? BFS memory may bottleneck; DFS may be preferred under memory constraints.

AI role? Used for ADR structuring and concept explanation; all numbers and code verified by me.





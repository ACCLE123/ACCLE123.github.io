---
title: difference constraints
date: 2024-04-22 16:01:00
tags:
---

### Difference constraints

Difference constraints are expressed as inequalities of the form $x_j - x_i \leq b$

Each variable $x_i$ can be represented as a node in the graph.

$x_j \leq b + x_i$ can be represented as directed edge from node i to node j with an edge weight of $b$ in the shortest path problem.

We can build a graph by following those rules. Each edge represents an inequality.

If $x_1 <= 10$ and $x_1 <= 20$, the maximum of $x_1$ is $min(10, 20) = 10$.

To find maximum value of each node, we use a shortest path algorithm; conversely, to find the minimum value, we use a different approach.

We need to define some nodes with value of 0, because we require relative answers for certain nodes. If we don't do that, the number of solution of inequalities must be infinite.

The shortest path algorithms, such as dijkstra and spfa, are used to solve this problem.



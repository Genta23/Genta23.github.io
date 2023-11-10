---
title: Graph Pruning
layout: default
parent: Research
---
<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# Multi-agent Pathfinding on Large Maps Using Graph Pruning : This Way or That Way?
Click here for the reference : <a href="http://svancara.net/files/ICAART_2023_SP_study.pdf" target="_blank">Multi-agent Pathfinding on Large Maps Using Graph Pruning : This Way or That Way?</a>

This article was created on November 7, 2023.

## Optimal MAPF solvers can generally be split into two categories

### The search-based
-  easily able to find solutions on large sparsely populated maps 
-  having trouble dealing with small densely populated maps.

### The reduction-based 
- able to deal with the small densely populated maps
- unable to find a solution for large maps even with a small number of agents.

The reduction-based method has problems when it is applyed for large instances.
So, in recent research, only vertices around the selected path are considered and other vertices are removed.

The reduction-based method is described in this paper.

First, this paper describes the four pruning strategies from original study, and then introduce four new ones to select paths for each agent.

## makespan optimal solutions
This paper is interested in makespan optimal solutions.

There are two common techniques to speed up computation.

### Using lower bound for the makespan
- The lower bound for \\(H\\) is the longest shortest path.

### Note that there are locations agent is definitely not present at certain times.
- for agent \\(a_i \in A\\), if vertex \\(v\\) is distance \\(d\\) away from start location \\(s_i\\), the agents \\(a_i\\) cannot be at vertex \\(v\\) at times \\(0,...,(d-1)\\).
- similarly, if vertex \\(v\\) is distance \\(d\\) away from goal location \\(g_i\\), the agents \\(a_i\\) cannot be at vertex \\(v\\) at times \\(H-d+1,...,H\\).

## The Sub-Graph Method
There are too many possibilities for an agent's location remain, which may overwhelm the underlying solver.

So, the idea is to pick just one of the shortest paths and to treat the other vertices as impassable enter the solver.

### Solving Strategies

- Baseline Strategy \\((k_{max}, const), (m = 0, m++)\\)
- Makespan-add Strategy \\((k = 1, const), (m = 0, m++)\\)
- Prune-and-cut Strategy \\((k = 0, k++), (m = 0, m++)\\)
- Combined Strategy \\((k = 0, k++), (m = 0, m++), same time\\)
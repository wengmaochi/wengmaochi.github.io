---
title: ILP-Based Approach for High-level CGRA Kernel Loops Mapping  
date: 2023-11-01
math: true
image:
  placement: 4
  caption: ''
summary: "This is the side project during internship at the Industrial Technology Research Institute."
---
This is the side project during internship at the Industrial Technology Research Institute. When researching on CGRA mapping problem, I happened to read PANORAMA[1] and found some improvement could be made in the integer-linear programming (ILP) problem defined in the paper.  

# Introduction
Coarse-grained reconfigurable array (CGRA) is promising to be hardware accelerator for machine learning workloads due to its power efficiency and reconfigurability. However, its unmature software stacks and compiler for map complex loop kernels onto the architectures limits its capability. 

Application loop kernels are represented as Dataflow Graph (DFG), where the nodes represent operations, and the edges represent the dependencies between operations. The compiler need to map DFG nodes onto CGRA in spatio-temporal way. One common way to represent CGRA is to use individual nodes to represent every hardware resource inside PEs and edges to represent wire information. The nodes and edges will duplicate in temporal coordinate to represent the same hardware resource in different cycles. In this way, the mapping problem is transformed from mapping applications onto CGRA to map a graph to the other graph. 

# PANORAMA[1]
The mapping problem is proven to be NP-complete. Researchers have strived to come up heuristic algorithm like Simulated Annealing[2] to solve this problem. In PANORAMA[1], they propose a high-level mapping approach to reduce the search space and derive a better starting point for original mapping.
### Overview of PANORAMA[1]
Unlike previous methods, PANORAMA[1] first use spectal clustering to cluster DFG nodes into cluster dependancy graph, where nodes represent clusters of DFG nodes and edge weights represent the number of DFG edges between two clusters. Then the CDGs will be map onto CGRA cluster by the proposed algorithm. This result will be the constraint to find the starting point for later mapping. 

### Cluster mapping algorithm proposed in PANORAMA[1]

#### Column-wise Scattering 
**Boolean Decision Variable:** $v_{irc}$ is 1 if $i$-th CDG node $v_i \in V$ is not pushed onto the CGRA cluster $P_{(r+1)c}$ from $p_{rc}$, 0 otherwise. $r$ and $c$ are CGRA cluster row and column ids. 

**Objective Function:** Minimize $\sum_{v_i \in V}v_{ir1}*|v_i|-(|V_D / R|)$, where $|V_i|$ denotes cluster size of $v_i$, $|V_D|$ denotes the total number of DFG nodes.

**Constraints:** 
$$ \sum_{v_j \in adj(v_i^m)} (v_{jr1}+v_{ir1}^m) \leq \xi_1 + \eta * v_{ir1}^m$$
$$ \sum_{v_j \in adj(v_i^m)} (v_{jr1}+v_{ir1}^m) \geq 2 * deg(v_{ir1}^m) - \xi_2 - \eta * (1 - v_{ir1}^m)$$
where $v_i^m$ is the multi degree node. $adj(v_i^m)$ are the set of nodes adjacent to $v_i^m$ and $deg(v_i^m)$ is the degree of $v_i^m$. $\eta$ is a large constant value used to linearize the equations. $\xi_1$ and $\xi_2$ are integer variables used to control the number of diagonal edges allowed. 

##### Row-wise Scattering 

**Boolean Decision Variable:** $v_{irc}$ is 1 if $i$-th CDG node $v_i \in V$ is mapped onto CGRA cluster column $c$ at the row $r$ fixed in the column-wise scattering.

**Objective Function:** Minimize 
{{< math >}}
$ |\sum_{(v_i,v_j) \in \varepsilon} \sum_{c=1}{C}w(v_i,v_j)*c*(v_{irc} - v_{jrc})|$
{{< /math >}}
where $w(v_i, v_j)$ is the number of inter cluster DFG edges between CDG nodes $v_i$ and $v_j$. 

**Constraints:** $\forall i \in V, \sum_{c=1}{C} v_{irc} = |V_i|/(|V_D|/(R*C)), \sum_{\forall v_i \in V}v_{irc} \geq 1$
# Observation

#### Column-wise Scattering

#### Row-wise Scattering 

# Reference 
[1]Dhananjaya Wijerathne, Zhaoying Li, Thilini Kaushalya Bandara, and Tulika Mitra. 2022. PANORAMA: divide-and-conquer approach for mapping complex loop kernels on CGRA. In Proceedings of the 59th ACM/IEEE Design Automation Conference (DAC '22). Association for Computing Machinery, New York, NY, USA, 127–132. https://doi.org/10.1145/3489517.3530429

---
title: ILP-Based Approach for High-level CGRA Kernel Loops Mapping  
# subtitle: 
date: 2023-11-01
math: true
image:
  placement: 4
  caption: 'credit: https://github.com/ecolab-nus/panorama'
summary: "This is the side project during internship at the Industrial Technology Research Institute."
tags:
  - Coarse-Grained Reconfigurable Array
  - Compile
  - Agile Hardware Methodology
---
<!-- This is the side project during internship at the Industrial Technology Research Institute.  -->

<!-- When researching on CGRA mapping problem, I happened to read PANORAMA[1] and found some improvement could be made in the integer-linear programming (ILP) problem defined in the paper.   -->

# Introduction
Coarse-grained reconfigurable array (CGRA) is a promising hardware accelerator for machine learning workloads due to its power efficiency and reconfigurability. However, its unmature software stacks and compiler for complex loop kernels onto the architectures limits its capability. 

Given an application, the loop kernels are represented as Dataflow Graph (DFG), where the nodes represent operations, and the edges represent the dependencies between operations. The compiler needs to map DFG nodes onto CGRA in spatio-temporal way. One common approach to represent CGRA is to use individual nodes to represent every hardware resource inside PEs and edges to represent wire information. The nodes and edges will duplicate in temporal coordinate to represent the same hardware resource in different cycles. In this way, the mapping problem is transformed from mapping applications onto CGRA to map a graph to the other graph. 

### Overview PANORAMA[1]
This mapping problem is proven to be NP-complete. Researchers have strived to come up heuristic algorithms like Simulated Annealing[2] to solve this problem. In PANORAMA[1], they propose a high-level mapping approach to reduce the search space and derive a better starting point for original mapping.

Unlike previous methods, PANORAMA[1] first use spectal clustering to cluster DFG nodes into cluster dependancy graph, where nodes represent clusters of DFG nodes and edge weights represent the number of DFG edges between two clusters. Then the CDGs will be map onto CGRA clusters by the proposed algorithm. This result will be the constraint to find the starting point for later mapping. 

### Cluster mapping algorithm proposed in PANORAMA[1]
The goal of cluster mapping step is to equally distribute the DFG nodes on CGRA, reducing inter-cluster edge distance so that lower-level mapping would be less complex. The proposed clustering mapping algorithm is inspired by split&push algorithm[3][4]. The clustering mapping algorithm consists of two steps: column-wise scattering and row-wise scattering. First, all the CDG nodes are placed in a single CGRA cluster at cluster coordinate $(1,1)$. Then it splits the CDG nodes into two groups: one left on the row, and the other one is pushed into next row. This step is repeated until all the CGRA culsters in the first column are filled with nodes. 
Then the nodes allocated in the first column are scttered row-wise to obtain the final mapping. Both column-wise and row-wise scattering are formulated as ILP problems to realize many-to-many mapping. 

#### Column-wise Scattering 

**Boolean Decision Variable:** $v_{irc}$ is 1 if $i$-th CDG node $v_i \in V$ is not pushed onto the CGRA cluster $P_{(r+1)c}$ from $p_{rc}$, 0 otherwise. $r$ and $c$ are CGRA cluster row and column ids. 

**Objective Function:** Minimize $\sum_{v_i \in V}v_{ir1}*|v_i|-(|V_D / R|)$, where $|V_i|$ denotes cluster size of $v_i$, $|V_D|$ denotes the total number of DFG nodes.

**Constraints:** 
$$ \sum_{v_j \in adj(v_i^m)} (v_{jr1}+v_{ir1}^m) \leq \xi_1 + \eta * v_{ir1}^m$$
$$ \sum_{v_j \in adj(v_i^m)} (v_{jr1}+v_{ir1}^m) \geq 2 * deg(v_{ir1}^m) - \xi_2 - \eta * (1 - v_{ir1}^m)$$
where $v_i^m$ is the multi degree node. $adj(v_i^m)$ are the set of nodes adjacent to $v_i^m$ and $deg(v_i^m)$ is the degree of $v_i^m$. $\eta$ is a large constant value used to linearize the equations. $\xi_1$ and $\xi_2$ are integer variables used to control the number of diagonal edges allowed. 

#### Row-wise Scattering 

**Boolean Decision Variable:** $v_{irc}$ is 1 if $i$-th CDG node $v_i \in V$ is mapped onto CGRA cluster column $c$ at the row $r$ fixed in the column-wise scattering.

**Objective Function:** Minimize 
{{< math >}}
$ |\sum_{(v_i,v_j) \in \varepsilon} \sum_{c=1}^{C}w(v_i,v_j)*c*(v_{irc} - v_{jrc})|$
{{< /math >}}
where $w(v_i, v_j)$ is the number of inter cluster DFG edges between CDG nodes $v_i$ and $v_j$. 

**Constraints:** $\forall i \in V, \sum_{c=1}^{C} v_{irc} = |V_i|/(|V_D|/(R*C)), \sum_{\forall v_i \in V}v_{irc} \geq 1$
# Observation
After closely exminaine the ILP constraints and objective functions, I found there are some improvement can be made in each type of scattering
#### Column-wise Scattering
The original column-wise scattering only consider CDG nodes that are currently in $r$-th row when deciding which nodes are pushed into next row in $r$-th scattering. However, each CGRA cluster represent a 4*4, or even more PEs. I believe that considering CDG nodes in previous rows is also important.

Thus, I add a panelty term in the objective function, and the modified objective function becomes: Minimize $\sum_{v_i \in V}v_{ir1}*|v_i|-(|V_D / R|) + \sum_{v_i \in V} (v_{ir1} * p_i * \lambda)$, where $|V_i|$ denotes cluster size of $v_i$, $|V_D|$ denotes the total number of DFG nodes, $p_i = \sum_{v_j \in adj(v_i) \land v_j \in S_r}(r_{current} - r_{v_j})$, $S_r$ contains all the nodes $v_i$ that have been left on 0 to $r-1$ row in previous iterations.


#### Row-wise Scattering 
The row-wise scattering in PANORAMA[1] does not guarantee a CDG node is mapped on connected PEs in a same row. Take a 16x16 CGRA and 4x4 PE as a cluster for example. Considering a row _ _ _ _, the results might be 0 1 0 1 or 1 0 0 1, which are impratical to later mapping, making the resulting starting point for later mapping suboptimal. 

Thus, forcing the CDG node is mapped on connected PEs is an improvement that can be made. The following discussion is under the setting of 16x16 CGRA and 4x4 PE cluster. 

We know that for every node $v_i$, there is four variables $v_{ir1}, v_{ir2}, v_{ir3}, v_{ir4}$ to express its final location. We can use sum of XOR to formulate into an ILP constraint. For a 2-input XOR $y= x_1 \oplus x_2$ can be formulated as 
$$ x_1+x_2+y < 3$$
$$ x_1 + y \leqslant x_2$$
$$ x_2 + y \leqslant x_1$$

First, we define four variables, $L, M, R, T$, where $L=v_{ir1} \oplus v_{ir2}$, $M=v_{ir2} \oplus v_{ir3}$, $R=v_{ir3} \oplus v_{ir4}$, and $T = L + M + R$, then we can construct two tables:   

{{< table path="valid.csv" header="true" caption="Table 1: valid combination" >}}
{{< table path="invalid.csv" header="true" caption="Table 2: invalid combination" >}}

If we want to set a constraint make ${v_{ir1}, v_{ir2}, v_{ir3}, v_{ir4}}$ satisfy the combination, we can set the ILP constraint as $L+M+R \leqslant 1$. However, there are some cases have to be deal with: 0000, 0010, 0100, 0110.

0000: prevented from the original constraint $\sum_{\forall v_i \in V}v_{irc} \geq 1$.

0010 and 0100: we can set this additional constraint only to node $v_i$ that satisfies $\sum_{\forall v_i \in V}v_{irc} > 1$.

0110: we can use big-M method by setting a variable $v_{i,0110}$,
$$v_{i,0110} \geqslant (1-v_{ir1}) + v_{ir2} + v_{ir3} + (1-v_{ir4}) - 3 $$
$$v_{i,0110} \leqslant (1-v_{ir1})$$
$$v_{i,0110} \leqslant v_{ir2}$$
$$v_{i,0110} \leqslant v_{ir3}$$
$$v_{i,0110} \leqslant (1-v_{ir4})$$
and combine all these constraint as:
$$L+M+R \leqslant 1 + M* v_{i,0110}$$
# Result
Benchmark: jpegfdct, 230 DFG nodes (same in the paper)

Setting: num of spectal cluster = 14, CGRA PE = 16x16, PE cluster = 4x4

{{< table path="result.csv" header="true" caption="" >}}

Future work: 
run more experiment on different benchmark with different numbers of clusters, benchmarks.
# Reference 
[1] Dhananjaya Wijerathne, Zhaoying Li, Thilini Kaushalya Bandara, and Tulika Mitra. 2022. PANORAMA: divide-and-conquer approach for mapping complex loop kernels on CGRA. In Proceedings of the 59th ACM/IEEE Design Automation Conference (DAC '22). Association for Computing Machinery, New York, NY, USA, 127–132. https://doi.org/10.1145/3489517.3530429

[2] S. Kirkpatrick et al., “Optimization by simulated  annealing,” in Science’83

[3] G. Di Battista et al., “A split & push approach to 3D orthogonal drawing,” in Graph Algorithms And Applications 2’04

[4] J. W. Yoon et al., “A graph drawing based spatial mapping algorithm for coarsegrained reconfigurable architectures,” in VLSI’09
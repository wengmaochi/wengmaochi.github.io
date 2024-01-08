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

## Introduction
Coarse-grained reconfigurable array (CGRA) is promising to be hardware accelerator for machine learning workloads due to its power efficiency and reconfigurability. However, its unmature software stacks and compiler for map complex loop kernels onto the architectures limits its capability. 

Application loop kernels are represented as Dataflow Graph (DFG), where the nodes represent operations, and the edges represent the dependencies between operations. The compiler need to map DFG nodes onto CGRA in spatio-temporal way. One common way to represent CGRA is MRRG, which uses individual nodes to represent every resource inside PEs and edges to represent wire and routing information. The nodes and edges will duplicate 
## PANORAMA[1]

## Observation

# Reference 
[1]Dhananjaya Wijerathne, Zhaoying Li, Thilini Kaushalya Bandara, and Tulika Mitra. 2022. PANORAMA: divide-and-conquer approach for mapping complex loop kernels on CGRA. In Proceedings of the 59th ACM/IEEE Design Automation Conference (DAC '22). Association for Computing Machinery, New York, NY, USA, 127–132. https://doi.org/10.1145/3489517.3530429

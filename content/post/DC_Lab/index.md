---
title: FPGA Correlation-based Disparity Map Estimator
date: 2022-01-01
math: true
image:
  placement: 2
  caption: ''
---
This is the final project of NTUEE Digital Circuit Labortory (EE-3016).

In collaboration with H.L. Hsieh, Y.C. Yu.

# Introduction
This is a FPGA disparity calculator. It consists of one FPGA, one camera, and one screen. To calculate disparity map, user has to use the camera take two picture with different angles. The first picture is saved into SD RAM, 


This is the final project of NTUEE IC design Lab (EE4003).

# Introduction
The aim of this project is to implement an accelerator for Elliptic Curve Crypotography(ECC), and go through all procedure of chip manufacturing. 

We reference [1] for hardware structure and [2] for algorithm. After some adjustments, we implemented it in RTL level, and follow the design flow below to tape-out and testing. 
![png](img/DCLab_final_FSM.drawio.png)
# Algorithm
Both ECC encryption and decryption consists of two same point operations - point doubling and point addion in Galois Field $GF(2^m)$. Since the number here is in $GF(2^m)$, the arithmetic has specital properties. 
## Addition&Subtraction
Bit-wise XOR with no carry-in and carry-out.
## Multipliction
#### a*b 
Same as ordinary multiplication, but no carry-in when doing addition.
#### 2*a 
Insert zero between each bit, for example
{{math}}
$P = 0111, 2P = 0010101$ 
{{math}}
## Devision 
#### a/b
We calculate 1/b by Itoh-Tsuji Algorithm and then conduct a * 1/b.

## Modular polynomial
We adopt ECC-163 standard with polynomial basis $x^163+x^7+x^6+x^3+1$. For every number, it is 163-bit and its i-th bit reprsent x^i. If arithematic result exceeds 163 bits, the number has to mod the polynomial basis $x^163+x^7+x^6+x^3+1$.
# Hardware Implementation

# Place&Route Result

# Tape-out Spec

# Reference



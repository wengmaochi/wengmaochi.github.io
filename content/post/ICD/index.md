---
title: Elliptic Curve Cryptography Chip (TAPE-OUTED)
date: 2022-06-01
math: true
image:
  placement: 2
  caption: 'overview of RCTP'
---

This is the final project of NTUEE IC design Lab (EE4003).

# Introduction
The aim of this project is to implement an accelerator for Elliptic Curve Crypotography(ECC), and go through all procedure of chip manufacturing. 

We reference [1] for hardware structure and [2] for algorithm. After some adjustments, we implemented it in RTL level, and follow the design flow below to tape-out and testing. 

# Algorithm
Both ECC encryption and decryption consists of two same point operations - point doubling and point addion in Galois Field $GF(2^m)$. Since the number here is in $GF(2^m)$, the arithmetic has specital properties. 
## Addition&Subtraction
Bit-wise XOR with no carry-in and carry-out.
## Multipliction
### a*b 
Same as ordinary multiplication, but no carry-in when doing addition.
### 2*a (square)
Insert zero between each bit, for example
{{math}}
P = 0111, 2P = 0010101 
{{math}}
## Devision 

## Modular polynomial

# Hardware Implementation

# Place&Route Result

# Tape-out Spec

# Reference



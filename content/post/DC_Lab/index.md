---
title: FPGA Correlation-based Disparity Map Estimator
date: 2022-01-01
math: true
image:
  placement: 1
  caption: ''
---
This is the final project of NTUEE Digital Circuit Labortory (EE-3016).
In collaboration with H.L. Hsieh, Y.C. Yu.

# Introduction
This is a FPGA disparity calculator. It consists of one FPGA, one camera, and one screen. To calculate disparity map, user has to use the camera take two picture with different angles. The first picture is saved into SD RAM, 


This is the final project of NTUEE IC design Lab (EE4003).
We reference [1] for hardware structure and [2] for algorithm. After some adjustments, we implemented it in RTL level, and follow the design flow below to tape-out and testing. 
![png](img/DCLab_final_FSM.drawio.png "Finite state machine")

![png](img/block_diagram.png "Block Diagram")


# Algorithm
We adopted the proposed method in [1], and it consists of three parts:
### Sum of Absolute Difference (SAD) algorithm
The SAD is a correlation-based method with high computational efficency. Given a pixel $(x,y)$ in left image and maximum value of user-defined maximum disparity $d_{max}$, abd correlation index $Crl(x,y,s)$ is calculated for each displacement $S$ of the correlation window in right image by:

{{<math>}}
$$Crl(x,y,s) = \summation_{u=-w,v=-w}{}$$
{{<math>}}
# Hardware Implementation


# Reference



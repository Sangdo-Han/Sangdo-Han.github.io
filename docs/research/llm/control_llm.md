---
layout: default
title: Control theory of LLM
parent: LLM
grand_parent: AI Research
math: katex
nav_order: 2
---

# A Control Theory of LLM prompting

## Introduction

&nbsp; In this post, I will review a very interesting recenet paper about LLM : `What's the Magic Word? A Control Theory of LLM prompting`.[[1](#Bhargava-Aman-et-al)]. The reason why I review this paper is that it gives us a new perspective on LLM, with the glasses of control theory. The main concept of the paper is that analogy on LLM system to control system : LLM be as a `plant` and the prompt be as a `control input`. Unlike other ML/DL framework, most of studies on LLM prompting were emperical, without mathematical evidence. This is because that LLM is too huge to understand. The paper, however, tries to understand LLM itself but treat it as a plant to control and give us a concrete understanding. 

## Basic Control Theory: System

&nbsp; To understand this paper, we need to know basic concepts of state space representation and control theory. The authors put the Appendix on the paper [[1](#Bhargava-Aman-et-al)], however, it is better to read Chapter 1-3 of [[2](#sontag-E-D)]. It is online free.

&nbsp; Briefly speaking, `control theory` depicts a system whether it is reachable (observable, controllable) and find out how to observe the system's state and control the system with external inputs (force, current and so on.)

&nbsp; In mathematically, `system` is the set of `state` of `plant`, `time`, `control input`. and `transition` of `state`. If we have output space (observable output), we could add `output` and `readout map` to the original `system`. In addition to the appendix [[1](#Bhargava-Aman-et-al)],   

&nbsp; A system ($$\Sigma$$) consists of:

 - State ($$X$$): The state of a plant refers to the minimal set of variables that completely define its behavior at any given time. These variables capture the internal condition of the plant.
 - Control Input ($$U$$): Control inputs are the external influences applied to the plant to manipulate its behavior.
 - Time ($$T$$): Time set, the ordered set of positive values.
 - Transition map ($$\phi: X \times U \times T^{2} \rightarrow X$$):  Transition map takes a current state (X(t)) at time t and a control input (u(t)) at time t, and returns the next state (X(t+1)) at the next time step (t+1). 
  > **Note**: $$T$$ or $$T^2$$?  
  If the control varies in time like as in `feedback control` or `time-varying control strategy`, we could add additional time space to describe the transition map. In the paper, however, as the transitted time t' differs from the original t, it seems that author put the additional time space into the collection. I suppose that the authors want to describe t' and t are nomially different.   

 - Output ($$Y$$): Observed output space.  
 - Readout map ($$h: X \times U \times T \rightarrow Y$$): Readout map takes a current state (X(t)) at time t and a control input (u(t)) at the time t, and returns the observed(read out) value (Y(t)).  

## Basic Control Theory: Reachability, Observability and Controllability

&nbsp; In the above definition, we can set almost everything to be a system. For example, a totally random process like rolling a perfect dice, we can set a `system`. Let us say that the `state` be `the top face of dice`, `control input` be the `human random force` of rolling a dice (make the statements stronger, let us assume that this force is just rolling a dice without any intention in every trials), and the `transition map` be the state transition between `trials`. However, even we designed `this rolling a dice` to be a `system` with mathematical terms, it is hard to say that we can control the system. Then, what does make a system be `controllable`?

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/1/1c/6sided_dice_%28cropped%29.jpg" width="30%">   
   ref: dices image from wikipedia 
</p>  


As in the definition of `system`, controllability can also be mathematically defined in control theory.  
But first, we need to know about the concept of `reachability` first.


### Reachability   

&nbsp; 

Back to `the rolling a dice system`, we can say that this system is reachable because we could potentially role any number, and always be observable.

### Controllability   

Again with `the rolling a dice system`, now we can see the problem, we put the `human random force` be the control input.

``

## Application on LLM
WIP {: .label .label-purple}

<!-- 
## Control Theory Background

&nbsp; 

1. System   
$$\Sigma =  (\Tau, X, U, \phi)$$  
$$ \dot{X} =  AX + Bu$$  
$$ Y =  CX + Du$$  
2. Control Input -->

# References  
<span id="Bhargava-Aman-et-al">[1]</span> Bhargava, A., Witkowski, C., Shah, M., & Thomson, M. (2023). What's the Magic Word? A Control Theory of LLM Prompting. arXiv preprint arXiv:2310.04444.  
<span id="sontag-E-D">[2]</span> Sontag, E. D. (2013). Mathematical control theory: deterministic finite dimensional systems (Vol. 6). Springer Science & Business Media.
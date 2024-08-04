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

<div align="center">
<p>
  <img src="https://upload.wikimedia.org/wikipedia/commons/1/1c/6sided_dice_%28cropped%29.jpg" width="30%">
</p>  
   ref: dices image from wikipedia 
</div>

### Reachability, Observable and Controllability   

&nbsp; Back to the `rolling a dice system`, we can say that this system is reachable because we could potentially role any number, and always be observable. However, we cannot say that this system is controllable because we cannot make intended input. Mathematically, we can define the reachability as in [[2](#sontag-E-D)].

#### Event
&nbsp; As we defined before, an arbitary system $$\Sigma$$ can be defined as $$\Sigma = (T, X, U, \phi)$$ . An `event` is a state at a time, a pair of state and time $$(x, t) \in X \times T$$.

#### Reacability and Controllability
The event $$(z, \tau)$$ `can be reached` from a state $$(x,\sigma)$$, if and only if there is a `path` of $$\Sigma$$ on $$[\sigma, \tau]$$ and if there exists an $$\omega \in U^{[\sigma, \tau)}$$ such that $$\phi(\tau, \sigma, x, \omega) = z$$, we can say we `can control` the state x to state z.

&nbsp; Again with `the rolling a dice system`, now we can see the problem, we put the `human random force` be the input, but it is not controllable.

## Control Theory of LLM
&nbsp; Now we can discuss control of LLM with the paper [[1](#Bhargava-Aman-et-al)]. All the following concepts and logics are originated from the paper.    
### Notations
&nbsp; In the paper, they denoted $$P_{LM}$$ be a causal language model, $$V$$ be a vocabulary set, $$V^*$$ be the set of all possible sequences of any length composed of tokens from $$V$$. The bold lowercase stands for sequence (vector) $$\bm{x}$$, while the unbolded lowercase letter $$x$$ be an individual token.

### Assumptions
&nbsp; Here are some assumptions before stating the definitions: 

1. The LLM system is discrete time: It is quite natural if we come across LLM chatbots, we get output token from LLM when we put query input.

2. The LLM system follows Shift-and-Grow State dynamics: Whereas the system state in an ODE-based system has a fixed size over time, the system state x(t) for LLM systems grows as tokens are added to the state sequence.

3. It is assumed that the LLM system follows a Markov Transition (In paper it is said in more gently as `Mutual exclusion on control input token vs. generated token`): The newest token is either drawn from the control input $$u(t)$$ or is generated by the LLM by sampling 
$$x{'} \sim  P_{LM} (x' | x(t)) $$. This differs from traditional discrete stochastic systems, where the control sequence and internal dynamics generally affect the state synchronously.

&nbsp; From the basic control theory and the followed assumptions, the authors defined the LLM systems and Control Input as follows:

### Definitions
#### Definition 1. Autoregressive LLM system

&nbsp; An autoregressive LLM system $$\Sigma = (V, P_{LM})$$ with control input consists of 
time set ($$T$$), state space ($$X$$), input ($$U$$), transition map ($$\phi$$) and readout map $$h$$.

1. Time set $$T$$ is a set of sequence number (natural number): $$T = N$$.
2. The state space $$X$$ is the set of all possible sequences of any length formed from the vocabulary set $$V$$: $$X = V^{*}$$
3. The input space is the set of sequences but also allows the no inputs $$\empty$$: $$U = V \bigcup {\{ \empty \}}$$
4. The transition map $$\phi$$ is sum of current state and input:  $$\phi : X \times U \times T^{2}$$ such that  
$$
\phi(x(t), u(t), t, t+1) =
  \begin{cases}
    x(t) + u(t) & \text{if $u(t) \neq \empty$}\\
    x(t) + x' & \text{otherwise}
  \end{cases}
$$  
where
$$x' \sim P_{LM}(x' | x(t)) $$. Sadly because they use the plus sign as a operator wihtout the definition, we need to assume that this is an operator that represent concatenation of state because we assume that it is autoregressive. More than that, as it is autoregressive, the output is deterministic.
5. The readout map returns the most recent r tokens from the state x: $$h(x(t);r) = [x^{t-r}(t), ... , x^{t}(t)]$$

#### Definition 2. LLM Output Reachability
&nbsp; $$y \in V^r$$ is reachable from the initial state $$x_0 \in V^*$$ if and only if there exists some time $$T$$ and input sequences $$u^* \in U^k$$ for some $$k + |x_0| \leqq  T $$ that makes the initial state $$x_0$$ move to output $$y = h(x(T), r)$$ at time $$T$$

#### Definition 3. Output Reachablility Set
&nbsp; The reachable output set from the initial state $$x_0 \in V^*$$ is denoted $$R_y(x_0)$$.

#### Definition 4. Output Controllabilty (Revised from the paper.)
&nbsp; An LLM system is output controllable if and only if, $$\forall x_0 \in V^*$$ and $$\forall y \in V^r$$, there exists a time $$T \geqq |x_0|$$ and the input sequence $$u^* \in U^k$$, where $$k\leqq T - |x_0|$$ such that the probability $$P(h(x(T),r) = y | x_0, u^*) > 0$$.

#### Definition 5. $$k-\epsilon$$ Controllability
&nbsp; If a datset of state-output pairs $$D = \{(x^{i}_{0}, y^{i})\}_{i\in [N]}$$, an LLM system $$\Sigma = (V, P_{LM})$$ is $$k-\epsilon$$ controllable with respect to D, if and only if $$P\{y \notin R^{k}_{y}(x_0)\} < \epsilon$$ for $$(x_0, y) \sim D$$, where $$R^k_y(x^i_0)$$ is reachable set of outputs as definition 3. under the constraint that prompt (input) u must have length $$|u| \leqq k$$.

### Apply to the Self-Attention Mechanism.


WIP (aug.3.2024)
{: .label .label-purple}

&nbsp; Above the definition on LLM controllability could be nice for generalization, we need to prove it is actually applicable with calculation. The paper choose the transformer based langauge model which is dominating.


<!-- 
## Control Theory Background

&nbsp; 

1. System   
$$\Sigma =  (\Tau, X, U, \phi)$$  
$$ \dot{X} =  AX + Bu$$  
$$ Y =  CX + Du$$  
2. Control Input -->

## References  
<span id="Bhargava-Aman-et-al">[1]</span> Bhargava, A., Witkowski, C., Shah, M., & Thomson, M. (2023). What's the Magic Word? A Control Theory of LLM Prompting. arXiv preprint arXiv:2310.04444.  
<span id="sontag-E-D">[2]</span> Sontag, E. D. (2013). Mathematical control theory: deterministic finite dimensional systems (Vol. 6). Springer Science & Business Media.
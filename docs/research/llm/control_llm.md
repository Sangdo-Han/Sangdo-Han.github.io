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
&nbsp; $$y \in V^r$$ is reachable from the initial state $$x_0 \in V^*$$ if and only if there exists some time $$T$$ and input sequences $$u^* \in U^k$$ for some $$k + |x_0| \leq  T $$ that makes the initial state $$x_0$$ move to output $$y = h(x(T), r)$$ at time $$T$$

#### Definition 3. Output Reachablility Set
&nbsp; The reachable output set from the initial state $$x_0 \in V^*$$ is denoted $$R_y(x_0)$$.

#### Definition 4. Output Controllabilty (Revised from the paper.)
&nbsp; An LLM system is output controllable if and only if, $$\forall x_0 \in V^*$$ and $$\forall y \in V^r$$, there exists a time $$T \geq |x_0|$$ and the input sequence $$u^* \in U^k$$, where $$k\leq T - |x_0|$$ such that the probability $$P(h(x(T),r) = y | x_0, u^*) > 0$$.

#### Definition 5. $$k-\epsilon$$ Controllability
&nbsp; If a datset of state-output pairs $$D = \{(x^{i}_{0}, y^{i})\}_{i\in [N]}$$, an LLM system $$\Sigma = (V, P_{LM})$$ is $$k-\epsilon$$ controllable with respect to D, if and only if $$P\{y \notin R^{k}_{y}(x_0)\} < \epsilon$$ for $$(x_0, y) \sim D$$, where $$R^k_y(x^i_0)$$ is reachable set of outputs as definition 3. under the constraint that prompt (input) u must have length $$|u| \leq k$$.

### Apply to the Self-Attention Mechanism. - Preliminaries

&nbsp; Above the definition on LLM controllability could be nice for generalization, we need to prove it with actual models and calculations. The paper choose the self-attention which dominates in LLMs.

#### Definition 6. Self-attention.

&nbsp; Self-attention $$\Xi$$ is parameterized by weight metrics $$\theta = (W_q, W_{key}, W_v)$$ (query, key, value). $$\Xi$$ is a mapping from the input token ($$R^{N \times d_{in}}$$) to output token ($$R^{N \times d_{out}}$$),

$$\Xi(X;\theta) = D^{-1} exp\left( {QK^{T}}\over{\sqrt{d_{key}}} \right)$$

where exp() denotes element-wise exponential of matrix entries, $$W_q, W_{key} \in R^{d_{in} \times d_{key}}$$, $$W_v \in R^{d_{in} \times d_{out}}$$, $$Q = XW_q$$, $$K=XW_{key}$$, $$V=XW_{v}$$ and D is a diagonal positive definite matrix, $$D:= diag \left( exp\left( {QK^{T}}\over{\sqrt{d_{key}}} \right) \bm{1}_{N \times 1 } \right) $$

&nbsp; In this paper, the reachability of output token representations $$\Xi (X; \theta)$$, they partitioned the input $$X \in R^{(k+m)\times d_{in}}$$ into a $$k \times d_{in}$$ block of control input representations $$U$$ and an $$m\times d_{in}$$ block of imposed state representations $$X_0$$ where $$k + m = N$$. With this partitioning, the definition of self-attention can be re-written as followings:

$$\begin{align}\Xi (X; \theta) = \Xi \left( \left[{U \atop X_0} \right] ; \theta \right) = \Xi ([U; X_0]; \theta) = \left[{U' \atop Y} \right] = [U' ; Y]
\end{align}$$

Also, the output $$X' = \Xi (X; \theta) \in R^{(k+m) \times d_{in}}$$ into a corresponding $$k \times d_{out}$$ matrix $$U'$$ and an $$m \times d_{out}$$ matrix Y.

#### definition 7. Reachability of self-attention
&nbsp; Following the definition 2, let $$Y^* \in R^{m \times d_{out}}$$ be the desired output, reachability for Self-attention can be defined as followings:   
&nbsp; $$Y^*$$ is reachable from initial state $$X_0$$ if and only if there exists some U that steers the output of $$\Xi (\left[U; X_0 \right] ; \theta ]$$ to output $$\left[ U' ; Y \right] $$ such that $$Y = Y^*$$

### Apply to the Self-Attention mechanism - Theorem

&nbsp; The key of defining the reachability begins with partitioning the output: $$Y = Y_u + Y_x$$, assuming that the output can be partitioned by output from control input and that from imposed state. $$Y_x$$ can be bounded as a function of $$X$$, $$k$$, and $$theta$$. While $$Y_u$$ is the remaining component from U. 

$$
\begin{align}
& Y = Y_u + Y_x \\
& = (Y_{u,\|} + Y_{u, \perp}) + (Y_{x, \|} + Y_{x, \perp}) \\
& = (Y_{u,\|} + Y_{x, \|}) + (Y_{u, \perp} + Y_{x, \perp}) \in span(Y^*) \oplus span(Y^*)^\perp \\
\end{align}
$$
  
Here, the parallel to and orthogonal to is in respect to the desired output $$Y^*$$.  
Remember that Y is partitioned output  

$$\Xi ([U; X_0]; \theta) = \left[ {U' \atop Y} \right] $$

Before the theorem and its proof, let us define matrix $$A$$.

$$
 A := exp\left(\frac{QK^T}{\sqrt{d_{key}}}\right) = exp \left( \left[
\begin{matrix}
Q_u K_u^T & Q_u K_x^T\\
Q_x K_u^T & Q_x K_x^T
\end{matrix}
\right] \frac{1} {\sqrt{d_{key}}}
\right) = \left[
  \begin{matrix}
    A_{uu} & A_{ux}\\
    A_{xu} & A_{xx}
  \end{matrix}
  \right]

$$

where, $$Q.$$ and $$K.$$ are partitioned components of $$Q$$ and $$K$$:

$$
Q = \left[ Q_u \atop Q_x \right]= \left[U \atop X_0 \right] W_q\\
$$

$$
K=\left[K_u \atop K_x \right]=\left[U \atop X_0 \right] W_{key}
$$

Similarly, $$D$$ is also be partitioned as followigs:

$$
D = diag \left(exp \left(\frac{Q K^T}{\sqrt{d_{key}}}\right) \bm{1}_{N \times 1}\right)
= \left[\begin{matrix}
D_u & \bm{0}\\
\bm{0} & D_x
\end{matrix}
\right]
$$

  
With the definitions and notions above, $$Y$$ can be defined as follows:

$$\begin{align}
& \Xi(X; \theta) = D^{-1} A V \\
& = \left[
  \begin{matrix} D^{-1}_u & \bm{0}\\
  \bm{0} & D^{-1}_{x}
\end{matrix}
\right]
\left[
  \begin{matrix}
  A_{uu} & A_{xu}\\
  A_{ux} & A_{xx}
  \end{matrix}
\right]
\left[
  \begin{matrix}
  V_{u} \\
  V_{x}
  \end{matrix}
\right] \\
& = \left[ 
 \begin{matrix}
 D^{-1}_u A_{uu} V_u + D^{-1}_u A_{ux}  V_x\\
 D^{-1}_x A_{xu} V_u + D^{-1}_{x} A_{xx} V_x
 \end{matrix}
\right] \\
& = \left[
U' \atop Y
\right]
\end{align}
$$

$$\begin{align}
\therefore Y =  D^{-1}_x A_{xu} V_u + D^{-1}_{x} A_{xx} V_x = Y_u + Y_x
\end{align}$$


#### **Theorem: reachability in the Self-attention mechanism**
&nbsp; Let $$Y^{max}_x = \Xi (X_0 ; \theta) $$ be the output of the self-attention layer given only the imposed state (initial state) $$X_0$$, and _i_-th row of the orthogonal component of $$Y^{max}_x$$ to the desired output $$Y^*$$ be $$Y^{max, i}_{x, \perp}$$.  
&nbsp; $${Y^*}$$ is unreachable for any control input $${U}$$ if, for any $${i \in \{1, ... , m\}  }$$,  

$$
\begin{align}
\|Y^{max, i}_{x, \perp}\| > k \gamma_{i} (X_0, \theta) \\
\end{align}
$$

where,

$$
\begin{align}
\gamma_{i}(X_0 , \theta) := \frac{e^\alpha}{g_i}\sigma_v M_u, \quad \alpha = \sigma_q \sigma_{key} M_u
M_x / \sqrt{d_{key}}
\end{align}
$$

$$
\begin{align}
g_i (X_0, \theta) := \Sigma^{m}_{j=1} exp ((X_0)^i W_q W^T_{key} (X_0)^{jT} / \sqrt{d_{key}})
\end{align}
$$

$$\sigma_v$$, $$\sigma_q$$ and $$\sigma_{key}$$ being the maximum singular values of the value, query, and key projection matrices, respectively. and with $$M_u := \max_j \|(X_0)^j \| $$ being the maximum norms of the control and imposed token embeddings, respectively. 

I will not repeat the proof of the paper because after partitioning, the rest of the proof is only about how to set some-what contrived constant and use basic row-wise summation and applying rudimentary inequity (Cauchy-Schwarz).

<!-- &nbsp; In other words, the output from the control input needs to make the output from the initial state be parallel to the desired output, cancelling out the astrayed direction.

&nbsp; The paper put the proof behind in appendix, howev8er, the decomposition above is the key idea of proof.

&nbsp; -->

## References  
<span id="Bhargava-Aman-et-al">[1]</span> Bhargava, A., Witkowski, C., Shah, M., & Thomson, M. (2023). What's the Magic Word? A Control Theory of LLM Prompting. arXiv preprint arXiv:2310.04444.  
<span id="sontag-E-D">[2]</span> Sontag, E. D. (2013). Mathematical control theory: deterministic finite dimensional systems (Vol. 6). Springer Science & Business Media.
---
layout: default
title: Research on ARC-AGI
nav_order: 3
has_children: false
parent: ARC-AGI
permalink: /docs/ai-research/arc-agi/resarch-note

---


# Feb. 2025.

## Problem definition

Suppose that we're designing a `single model` that solves the ARC-AGI problem. There are several conditions which the model needs to deal with.

1. The dimensionality of input/output is not pre-determined.
2. Though all the tasks can be grouped as a `IQ test`, all the detailed tasks have less in common.
3. The tasks in evaluation steps are totally different from those in the train steps.
4. The model has only 2~9 examples (mostly 3) to solve each task. -->



### Approach - Baseline

Instead of seeking a `direct` transformation that converts the input $$X_{in}$$ into the output $$X_{out}$$, we leverage generative models (such as VAE, flow-based model and diffusion models) to create a latent representation $$X_{latent}$$ from both input and output. To achieve this, we introduce two projections:  

$$\begin{align}
f: X^{*} \rightarrow X_{in}\\
g: X^{*} \rightarrow X_{out}
\end{align}$$   

Here, $$X^*$$ represents a higher-dimensional latent space designed to capture complex relationships and interactions between $$X_{in}$$ and $$X_{out}$$. By utilizing dual parameterized projections $$f$$ and $$g$$, we enable a more controlled and nuanced learning process, allowing the model to optimize representations that are tailored for both input reconstruction and output generation.
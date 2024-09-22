---
layout: default
title: Workthrough
parent: ARC-AGI Challenge
grand_parent: AI Research
math: katex
nav_order: 1
---

## Dataset
&nbsp;&nbsp;&nbsp;&nbsp; Let us begin with understanding the data.

### Data format
&nbsp;&nbsp;&nbsp;&nbsp; All the data is JSON encoded, the structures of JSON are as followings (* stands for training | evaluation | test) :

- *_challenges
```json
{
    "task_id": {
        "test": [
            {
                "input": [[...], ..., [...]]
            }
        ],
        "train": [
            {
                "input": [[...], ..., [...]],
                "output": [[...], ..., [...]]
            },
            ...
        ]
    }, 
    ...
}
```

- *_solutions
```json
{
    "task_id": [[...], ..., [...]],
    ...
}
```

- submission format
```json
{
    "task_id": [
        {
            "attempt_1": [[...], ..., [...]],
            "attempt_2": [[...], ..., [...]]
        },
    ],
    ...
}
```

All the arrays have integer digits in range 0 to 9, and each element stands index of some hex_colors,

```python
hex_colors = ['#000000', '#0074D9','#FF4136','#2ECC40','#FFDC00',
     '#AAAAAA', '#F012BE', '#FF851B', '#7FDBFF', '#870C25']
```

<p align="center">
  <img src="https://sangdo-han.github.io/docs/research/arc-agi/images/hex_colors.png">
</p>

### Visualization
&nbsp;&nbsp;&nbsp;&nbsp; Let's visualize the data (I choose `f35d900a` and `fcb5c309`)
Basically, we visaulize the data in 2d-grid with colors, however, we could treat the `color values` not as color channel but as `positional values` in z-axis, thinking that we applied one-hot encoding. The third one is colored 3-d scatter, just for highlights the z-level.

 &nbsp; | f35d900a | fcb5c309
|----|----|----|
Basic | <img src="https://sangdo-han.github.io/docs/research/arc-agi/images/f35d900a.png">| <img src="https://sangdo-han.github.io/docs/research/arc-agi/images/fcb5c309.png">
3d scatter| <img src="https://sangdo-han.github.io/docs/research/arc-agi/images/f35d900a_pointcloud.png"> | <img src="https://sangdo-han.github.io/docs/research/arc-agi/images/fcb5c309_pointcloud.png">
3d scatter (colored)| <img src="https://sangdo-han.github.io/docs/research/arc-agi/images/f35d900a_pointcloud_colored.png"> | <img src="https://sangdo-han.github.io/docs/research/arc-agi/images/fcb5c309_pointcloud_colored.png">



## Problem definition

&nbsp;&nbsp;&nbsp;&nbsp; Suppose that we're designing a `single model` that solves the ARC-AGI problem. There are several conditions which the model needs to deal with.

1. The dimensionality of input/output is not pre-determined.
2. Though all the tasks can be grouped as a `IQ test`, all the detailed tasks have less in common.
3. The tasks in evaluation steps are totally different from those in the train steps.
4. The model has only 2~9 examples (mostly 3) to solve each task.

### Approach - Baseline
&nbsp;&nbsp;&nbsp;&nbsp; Instead of seeking a `direct` transformation that converts the input $$X_{in}$$ into the output $$X_{out}$$, we leverage generative models (such as VAE, flow-based model and diffusion models) to create a latent representation $$X_{latent}$$ from both input and output. To achieve this, we introduce two projections:  

$$\begin{align}
f: X^{*} \rightarrow X_{in}\\
g: X^{*} \rightarrow X_{out}
\end{align}$$   

Here, $$X^*$$ represents a higher-dimensional latent space designed to capture complex relationships and interactions between $$X_{in}$$ and $$X_{out}$$. By utilizing dual parameterized projections $$f$$ and $$g$$, we enable a more controlled and nuanced learning process, allowing the model to optimize representations that are tailored for both input reconstruction and output generation. 

<!-- 
### Novelty

&nbsp;&nbsp;&nbsp;&nbsp; Addition to the traditional generative architecture, As ARC-AGI is not ordinary data, we proposed geo-topological layer that guiding the projections. -->


<!-- ### TDA
Assuming that the output data is quite induced from the input, the output data needs to be reversed  -->


<!-- ## Possible Approaches
Thinking of consistency in the fewshot samples, we can apply
- RNN / LSTM
- Transformers

Thinking of Generating the output solution
- RNN / LSTM
- Transformers
- GCN
- flow-based
- diffusion (score-based)


### TIC

topologically induced condition in Diffusion

Suppose that we have a topological homolgy in `Input + output`s sample space. The task of diffusion is now trying to make output that keeps `input+output` homology. -->

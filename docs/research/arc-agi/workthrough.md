---
layout: default
title: Workthrough
parent: ARC-AGI Challenge
grand_parent: AI Research
math: katex
nav_order: 1
---
## Conditions

Suppose that we're designing a `single model` that solves this problem. There are several conditions which the model needs to deal with.

1. The dimensionality of input/output is not pre-determined.
2. Though all the tasks can be grouped as a `IQ test`, all the detailed tasks have less in common.
3. The tasks in test steps are totally different from those in the train steps.
4. The model has only 2~9 examples to solve each task.


## Dataset
### EDA

#### Data format
All the data is JSON encoded, the structures of JSON are as followings (* stands for training | evaluation | test) :

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
<!-- 
#### Visualization

### TDA
Assuming that the output data is quite induced from the input, the output data needs to be reversed 


## Possible Approaches
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

Suppose that we have a topological homolgy in `Input + output`s sample space. The task of diffusion is now trying to make output that keeps `input+output` homology.

 -->

---
layout: default
title: ARC-AGI dataset
nav_order: 2
has_children: false
parent: ARC-AGI
permalink: /docs/ai-research/arc-agi/dataset
---

# ARC-AGI Dataset (2024)

Let us begin with understanding the data.

## Data format

All the data is JSON encoded, the structures of JSON are as followings (* stands for training , evaluation and test) :

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
  <img src="https://sangdo-han.github.io/docs/ai-research/arc-agi/images/hex_colors.png">
</p>

### Visualization

Let's visualize the data (I choose `f35d900a` and `fcb5c309`)
Basically, we visaulize the data in 2d-grid with colors, however, we could treat the `color values` not as color channel but as `positional values` in z-axis, thinking that we applied one-hot encoding. The third one is colored 3-d scatter, just for highlights the z-level.

 &nbsp; | f35d900a | fcb5c309
|----|----|----|
Basic | <img src="https://sangdo-han.github.io/docs/ai-research/arc-agi/images/f35d900a.png">| <img src="https://sangdo-han.github.io/docs/ai-research/arc-agi/images/fcb5c309.png">
3d scatter| <img src="https://sangdo-han.github.io/docs/ai-research/arc-agi/images/f35d900a_pointcloud.png"> | <img src="https://sangdo-han.github.io/docs/ai-research/arc-agi/images/fcb5c309_pointcloud.png">
3d scatter (colored)| <img src="https://sangdo-han.github.io/docs/ai-research/arc-agi/images/f35d900a_pointcloud_colored.png"> | <img src="https://sangdo-han.github.io/docs/ai-research/arc-agi/images/fcb5c309_pointcloud_colored.png">


---
layout: default
title: Machine Unlearning
nav_order: 1
has_children: false
parent: AI Research
permalink: /docs/research/unlearning
---

# Machine Unlearning
Initialized (Dec.02.2023)
{: .label .label-purple}

&nbsp;&nbsp;&nbsp;&nbsp;This code is based on my submissions on NeurIPS 2023 Machine Unlearning Challenge, It seems that the following methods achieved huge accuracy decoupling between retain and forget dataset in training, seams that it has a chance to achieve the `machine unlearning`.

I will upload the code in [my-github](https://github.com/Sangdo-Han/unlearning) when all the code (including mock of nips unlearning, verifications) is ready, so far, I will just post the brief algorithms and ideation as below (sRFL : done).

# Unlearning Ideation

The code contains two novel way of unlearning, both of them achieved decoupling in forget/retain accuracy : forget goes low, while retain goes high.

1. sRFL (simple Rolling in Forget Logits) 
2. ALIF (Auxilary Layer for Inducing Forget)

## sRFL   
Initialized (Dec.02.2023)
{: .label .label-purple}
simple Rolling in Forget Logits (sRFL) is a simple way of disturbing the forget label in finetuning framework. 

The following is about train accuracy about `CIFAR-10`

<p align="center">
 <img src="https://sangdo-han.github.io/docs/research/unlearning/cifar10_accuracy.png">
</p>

In the figure above, facc stands for accuracy of forgetting dataset, racc stands for accuracy of retain dataset.

### Algorithm with Pseudo Code
1\. **Copy** a model from original trained model    
```
teacher_model <- copy.deepcopy(model)
teacher_model.eval() // teacher model will not be trained
```
2\. **Freeze layers of model except for first two backbones (feature extracting)**    

3-1. **Loop : Generate logits by teacher model**   
```
for data in datasets (retain + forget dataset):
    retain_data, forget_data <- data
    pseudo_forget_logits <- teacher_model(forget_data)
    pseudo_retain_logits <- teacher_model(retain_data)
```

3-2. **Roll** only the pseudo_forget_logits (same mechanism in torch.roll with random roll steps in [-1,1] )  
```
    rolled_pseudo_logits <- roll(pseudo_forget_logits)
```  
3-3. **Minimize** KL-divergence between rolled_pseudo_logits and model's output (from forget data), and use cosine similarity be a regularization and use retain_loss (KL-divergence between pseudo-label(not-rolled) and model outputs about retain dataset)    
```
    forget_outputs <- model(forget_data)
    retain_outputs <- model(retain_data)

    forget_loss <-  KLDiv(rolled_pseudo_logits, forget_outputs) \
                    - cosine_similarity(rolled_pseudo_logits, forget_outputs)\
                    + KLDiv(pseudo_retain_logits, retain_outputs)
```

3-4. **backward** propagation by forget_loss
```
   forget_loss.backward()
```
End of Loop.  

## ALIF  
TBD  

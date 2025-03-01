---
layout: default
title: LLM
nav_order: 1
has_children: true
parent: AI Research
permalink: /docs/ai-research/llm
---

# LLM
Large Language Model studies and utilizations


## Paradigm Shifts in Language Models

From beginning of Markov based approach that predicting next n-words, Language Model (LM) evolved drastically with the Transformers and GPT-2 / BERT - approaches. LLM stands for concurrent (2025) LM, mostly served by big tech companies and startups, most of them are now closed source but several LLM architectures and their way of training are officially open (LLama, Deepseek, and so on.)


(ref. https://arxiv.org/pdf/2303.18223.pdf
)      


| Type | Backbone Architecture | Description | Example |
|---|---|---|---|
| LLM (Large Language Model, early 2020s ~ present) | Transformers (primarily) | Scaled up PLMs, dataset size, and computational resources. Trained with Human interactions (RLHF - Reinforcement Learning from Human Feedback) or other Reinforcement Learning techniques | GPT-3, GPT-4, PaLM-2, Gemini-1.5, LLama-3.1, DeepSeek-V3 |
| PLM (Pre-trained Language Model, late 2010s) | Transformers, RNNs (earlier models) | Massive data induced deep learning model when pre-training, followed by fine-tuning for specific (downstream) tasks. Self-supervised learning with masked language model (MLM - ELMo, BERT, RoBERTa) and causal language model (GPT) applied for training. | ELMo (RNNs), BERT, RoBERTa, GPT-2 (Transformers) |
| NLM (Neural Language Model, early-mid 2010s) | RNNs, Feedforward Neural Networks | Introduces neural networks to model language, enabling distributed representations (word vectors) and overcoming some limitations of SLMs. | word2vec (Feedforward), GloVe (Matrix Factorization), Recurrent Neural Networks |
| SLM (Statistical Language Model, Pre 2010s) | N/A (Statistical methods) | Traditional probabilistic models (Markov assumption). Predicts next words / context(n-words). | n-gram language model |


In brief, there are no huge differences between LLMs and PLMs in model base architecture. However, as a LLM has much larger size in terms of model parameters and it requires much more data, AI scientists tried various training techniques to train / fine-tuning the models.

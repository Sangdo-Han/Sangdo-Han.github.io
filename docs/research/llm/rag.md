---
layout: default
title: RAG
parent: LLM
grand_parent: AI Research
math: katex
nav_order: 1
---

# RAG

&nbsp;&nbsp;&nbsp;&nbsp;After the adevent of LLM, many AI researchers try to engage pretrained LLM with their own data. This is because training LLM from scratch is costly and hard. 
In this background, one of the powerful engaging method is RAG (Retrieval-augmented generation) [[1](#lewis-et-al)]. The concept of RAG is quite simple but powerful.   
&nbsp;&nbsp;&nbsp;&nbsp;The following diagram is an architecture of LLM service with RAG, drawn by myself, which could be a little bit different from the original paper[[1](#lewis-et-al)].

<p align="center">
 <img src="https://sangdo-han.github.io/docs/research/llm/rag_architecture.png">
</p>

&nbsp;&nbsp;&nbsp;&nbsp;In batch process, we can save our constarints (contexts, documents or policies) with embedding models, (vectorization / encoding / graph embedding and so on). Once we have a language query from user, we simply encode the query with the same embedding models, and findout the document near the vector. Finally, we simply put found documents and original user's query to LLM, we can get generated output with our intended contexts. 


# References
<span id="lewis-et-al">[1]</span> Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., ... & Kiela, D. (2020). Retrieval-augmented generation for knowledge-intensive nlp tasks. Advances in Neural Information Processing Systems, 33, 9459-9474.
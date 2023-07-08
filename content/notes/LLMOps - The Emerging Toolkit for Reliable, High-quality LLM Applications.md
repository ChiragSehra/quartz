---
title: "LLMOps - The Emerging Toolkit for Reliable, High-quality LLM Applications"
date: 2023-07-08
by: Matei Zaharia
tags: 
    - llms
    - production
    - mlops 
    - mlflow
    - GPT
---



## Many operational challenges
- Cost
- Privacy
- Performance
- Need for frequent updates (knowledge is limited)


## How can we make LLM apps reliable
- MLOps tools for gen AI: LLMOps in mlflow
- New programming models: Stanford DSP

## Start with Basics
Need ways to
- Swap and compare different LMs (or pipelines) for an app
- Track and evaluate outputs
- Deploy pipelines
- Monitor deployed pipelines


## Swapping and comparing LLMs
- mlflow has a "model" abstraction that can wrap an arbitrary function and knows it schema
- Can use this ot abstract over LLM and pipeline implementations
	- OpenAI, HuggingFace, PyTorch, Langchain

Just calling `model.predict, deploy ` or `evaluate` on each one

## Auto-Logging Models
![[Pasted image 20230623123432.png]]

- mlflow also provides a UI for evaluating different models. 
- mlflow allows **evaluating models programmatically** using `mlflow.evaluate()`


## Monitoring data from deployment
- mlflow models can be deployed with docker, kubernetes, databricks sagemaker
- Once deployed, it can be use existing monitoring tools in mlflow 


## Stanford DSP (Demonstrate, Search, Predict)
- Declarative language
- Primitives
	- **Demonstrate**: expressing constraints on desirable behaviour
	- **Search**: breaking down problems and looking for information etc
	- **Predict**: use information and double check work etc.
- DSP provides
	- clear path for iterative development
	- its modular - unties system design from "Prompting" and "fine tuning"
	- Auto Optimisations - Improvements to the `DSP compiler and Engines`

## Good to refer
[LMQL.ai](https://lmql.ai/)

[jsonformer](https://github.com/1rgs/jsonformer)

[Guardrails](https://shreyar.github.io/guardrails/)

[FrugalGPT](https://www.arxiv-vanity.com/papers/2305.05176/)

[Chain of Thought Prompting Elicits Reasoning in LLMs](https://arxiv.org/abs/2201.11903)

[Tree of Thoughts: Deliberate Problem Solving with LLMs](https://arxiv.org/pdf/2305.10601.pdf)

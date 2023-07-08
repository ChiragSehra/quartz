
---
title: "Building LLM Apps for Production"
date: 2022-07-08
by: Chip Huyen
tags: 
    - llms
    - production
    - mlops 

---
Following are the challenges faced while building LLM apps for production

## Challenge 1: Consistency
1. How to ensure user consistency?
2. How to ensure downstream apps can run without breaking?

if somebody says that using `temperature=x` can fix it, then it cannot. 


## Challenge 2: Hallucination
- because the LLM's internal knowledge and labeler's internal knowledge cause by behaviour cloning.
![[notes/images/RLHF.png]]
												Reinforcement learning from human feedback


## Challenge 3: Privacy
- Building a chatbot to let users talk to data, how to ensure that chatbot doesn't reveal PII data.
- Make sure APIs are compliant


## Challenge 4: Context Length
- Need context-dependent answers...? 
- Use cases:
	- Document processing
	- summarisation
	- Narrative
	- Ask any task involving genes and proteins etc.


## Challenge 5: Data Drift
- Existing models which are trained on data collected in the past, fail to generalise answering questions even when the provided with an updated evidence corpus
- [Situated QA](https://situatedqa.github.io/)


## Challenge 6: Forward and backward compatibility
- same model ,new data - will that work? can it work? have the coders made it compatible
- How to make sure prompts will still work with newer models?



## Challenge 7: LLM on the edge
- Healthcare devices
- Autonomous vehicles
- Drive-thru voice bots
- Personal GPT, trained on own data, running on the macbook


We will get bottlenecked by compute + memory + tech available
if its trained on a server, how do we send data across in real-time and produce results(PII data included)?


## Choosing a Model Size
7B param model can run on a macbook
- bfloat16 = 14GB memory
- int8 = 7GB memory

7B param model costs approx*
- $100 to finetune
- $25k to train from scratch

![[notes/images/choosing_a_model.png]]

## Challenge 9: Efficiency of chat as an interface
1. Search interface
2. Chat interface

Which costs more?
Which is preferred for your use-case

- Chat is **NOT** efficient, but is very robust
- user prefers chat, but is expensive than search interface
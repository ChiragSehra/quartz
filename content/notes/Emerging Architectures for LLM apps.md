
---
title: "LLM app architecture"
by: "Matt Bornstein and Rajko Radovanovic"
date: 2023-07-08
tags: 
    - llms
    - dataprocessing
    - prompting
    - tools
    - langchain
---

If we don't need to train our own model from scratch, we can use **`In Contextual Learning`**
approach.
In contextual learning
- is clever prompting 
- sharing only relevant documents with LLM, instead of sharing the complete bunch of information

## Stages
- Data Preprocessing and Creating embedding
- Prompt Construction and retrieval
- Prompt Execution and Inference


### Data preprocessing and creating embeddings
- Storing private data to be used later.
- Break the documents into chunks, pass through embedding model and then storing in a vector database.

![[notes/images/data_preprocessing and embedding creation.png]]


### What is contextual data?
Contextual data is the text documents, PDFs, or any structured format  information like CSVs and SQL tables.

In the data ingestion process, we can use dataops etl tools to build wrappers for ingesting and all these structured format information to feed to our embedding model

### What are embeddings?
- Embedding are vectors that represent the meaning of a sentence
	- OpenAI API
	- Cohere
	- Opensource models from Huggingface

### What are vector databases?
- store efficiently embeddings and query them quickly.
- Vector databases that can be used:
	- Opensource system - Weaviate, Vespa and Qdrant
	- Local Vector Management Library: Chroma and FAISS
	- `pgvector` for postgres


## Prompt Construction and Retrieval

![[notes/images/prompt_construction_end_user.png]]

1. End user queries the hosted app with the question.
2. Question is sent to the `Langchain Master Orchestrator`  #langchain
3. #langchain  sends a request to embedding store to chunk the question
4. The chunks are queries on a #vectordb .
5. #vectordb returns the response to #langchain
6. #langchain orchestration can return the response on the hosted app or toggle some APIs/Plugins to perform some action based on the response returned. (example: the returned response contains a suggestion to reset the connection and open a new connection. The APIs/Plugins can help to open a ticket in JIRA for the operation team to visit the customer and reset their connection or send a signal to reset the connection.)
7. Once an action is toggled by the APIs/Plugins, the hosted app can show the notification to approve by the end user to either `approve` or `decline` the action suggested.

## Prompt Creation and Retrieval for SME

![[notes/images/prompt_creation_sme.png]]

1. SME interacts in a `testing` setup i.e. in the `Prompt Lab`. This lab offers the capability to test prompts against various version of the LLM model and change hyper-parameters like `temperature` etc.
2. The request is forwarded to `Langchain Master Orchestration`. #langchain 
3. The request received by #langchain is converted into chunks by the embedding store.
4. These chunks are queried on the #vectordb 
5. #vectordb returns the response to #langchain orchestration.
6. #langchain orchestration can return the response on the `Prompt Lab` or toggle some APIs/Plugins to perform some action based on the response returned. (example: the returned response contains a suggestion to reset the connection and open a new connection. The APIs/Plugins can help to open a ticket in JIRA for the operation team to visit the customer and reset their connection or send a signal to reset the connection.)
7. Once an action is toggled by the APIs/Plugins, the hosted app can show the notification to approve by the end user to either `approve` or `decline` the action suggested.

Another thing to add here is that SME would be able to provide feedback to the responses and help in structuring the next knowledge base for LLM to be trained on and the Prompt Optimisation.


## Prompt Execution/Inference


![[notes/images/prompt_execution_inference.png]]
1. **LLM Cache**: 
	1. Caching the frequently accessed content which is popular and trending.
	2. Helps in reducing retrieval time, improves response time and eases burdens on the backend servers.
	3. Read more about `Semantic Caching`  - stores similar or related queries, thereby increasing cache hit probability and overall caching efficiency.
	4. What can be used?
		1. Redis
		2. SQLite
		3. GPTCache
2. **Logging/MLOPS/LLMOPS**
	1. Logging of replies from LLMs
	2. Managing and versioning dynamic prompts
	3. Scoring of different versions of LLM with rapid development
	4. What can be used?
		1. Weights and Biases
		2. MLflow
		3. PromptLayer
		4. Helicone
3. Verification and Validation
	1. Avoiding to share PII data with LLMs
	2. Manging Privacy and Anonimity
	3. What can be used?
		1. Guardrails
		2. Redbuff
		3. Guidance
		4. LMQL
4. LLMs API and Hosting
	1. Telco LLM API: 
		1. Modular APIs to be shared with different clients to use our LLM and pay per API call based on tokens (similar pricing model as OpenAI)
		2. What can be used?
			1. OpenAI
			2. Anthropic
	2. Opensource LLM API: 
		1. Build a Leaderboard for all Telco Opensource LLMs trained. These LLMs help to achieve `Prompt Optimization` using *Chain of thought* or `Knowledge Retrieval Systems` prompting
		2. What can be used?
			1. Hugging Face
			2. Replicate
	3. Hyperscaler Hosting
		1. Model fine tuning and hyper-parameter tests on GPUs on cloud
		2. What can be used?
			1. AWS
			2. GCP
			3. Azure
			4. Corewave
			5. OVH
	4. Notebooks on Cloud
		1. Experiments and Jupyter notebooks
		2. What can be used?
			1. Jupyterhub
			2. Databricks
			3. Anyscale
			4. Mosaic
			5. Runpod
			6. Modal
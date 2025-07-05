# Domain-Specific Knowledge Graph Construction (Inspired by PHALM)

This repository provides a lightweight and cost-efficient pipeline to construct a domain-specific knowledge graph using structured information retrieved from Wikipedia and processed using Large Language Models (LLMs). The methodology takes inspiration from the PHALM research paper:

**PHALM: A Graph Prompting Framework for Reasoning with Language Models**  
[https://arxiv.org/abs/2310.07170](https://arxiv.org/abs/2310.07170)

While PHALM uses a crowdsourced seed graph to initiate the reasoning process, this project introduces a fully automated alternative, leveraging modern search APIs and LLMs for end-to-end graph generation.

---

## Objectives

- Automate the process of constructing a knowledge graph in a specific domain.
- Use credible, factual sources like Wikipedia for data retrieval.
- Extract structured knowledge (entities, relationships) using prompt-based LLM reasoning.
- Maintain a cost-efficient pipeline without the use of GPUs or paid infrastructure.

---

## Core Features

### 1. Retrieval-Enhanced Prompting
The system retrieves domain-relevant text from Wikipedia using the [Tavily API](https://docs.tavily.com/), ensuring the credibility and reliability of the content.

### 2. Structured Information Extraction
Each retrieved text passage is processed using a curated prompt and passed into the [Gemini LLM API](https://ai.google.dev/), which returns structured outputs such as:
- Entities
- Relations
- Descriptions
- Categories

### 3. DataFrame-Based Modular Processing
The extracted knowledge is parsed and stored in `pandas` DataFrames for inspection, cleaning, and later graph construction. Each DataFrame corresponds to a specific semantic category such as scientists, countries, or awards.

### 4. Manual Batching with Free API Tiers
Due to API rate limits, the dataset is split into 8 batches. During each batch run:
- The `start` index and list (`scientist_x`) are updated manually.
- Gemini and Tavily API keys are rotated for uninterrupted access.

This ensures compliance with the free-tier limitations while maintaining a fully functional pipeline.

### 5. Preparation for Graph Construction
In Part 2 of the notebook, the cleaned DataFrames are converted into graph-ready structures using NetworkX or PyG (PyTorch Geometric).

---

## Recommended Improvements

To scale this pipeline into a production-grade system, the following changes are recommended:

- Use GPT-4o or LLaMA 4 for more accurate and nuanced information extraction.
- Upgrade to Tavily Pro or integrate a scalable search tool with higher query limits.
- Automate batching and key rotation to eliminate manual intervention.
- Use LangGraph’s [`create_react_agent`](https://python.langchain.com/api_reference/langchain/agents/langchain.agents.react.agent.create_react_agent.html) to implement iterative agents capable of multi-step reasoning and error correction.

---

## Repository Structure

```plaintext
.
├── data/
│   └── raw_inputs/                  # Initial name lists and domain-specific inputs
├── notebooks/
│   ├── 1_data_retrieval.ipynb       # Retrieves Wikipedia text and extracts structured knowledge
│   └── 2_dataframe_to_graph.ipynb   # Converts cleaned DataFrames into graph structures
├── prompts/
│   └── prompt_template.txt          # Prompt used for LLM-based extraction
├── utils/
│   └── api_utils.py                 # Helper functions for API requests and response parsing
├── README.md
└── requirements.txt

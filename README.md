
# In-Surely - An Interactive Insurance Query-Answering System

[![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/) &nbsp;
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AbhishekSinghDhadwal/In-Surely/blob/main/In_Surely.ipynb) &nbsp;
[![Instructions](https://img.shields.io/badge/Usage%20Instructions-8A2BE2)](#installation-and-usage-aka---how-to-run-the-notebook)

## Overview

This notebook contains an **Interactive Question-Answering (QA) System** for insurance policies using a Retrieval-Augmented Generation (RAG) pipeline. The system integrates semantic search, document embeddings, caching, cross-encoder reranking, and GPT-based response generation.

## Key Features

- Extracts and processes text, including tables, from insurance policy documents (or generic documents of your choice with minor tweaks).
- Generates semantic embeddings for text retrieval.
- Implements caching for efficient query handling.
- Reranks results using a cross-encoder for higher accuracy.
- Synthesizes detailed responses using a model of your choice.
- Interactive query inputs and visualizations.

## Project Workflow

### 1. Setup and Configuration

- **Environment:** The following details are required to be setup in the notebook prior to using. Originally tested in Google Colab.
- **User Inputs:**
  - `OPENAI_API_KEY`: API key for the GPT model.
  - `DOCUMENT_LOCATION`: Path to the folder containing insurance policy PDFs.
  - `CHROMA_PATH`: Directory for storing ChromaDB embeddings and cache. Can be an empty folder in your Drive.
  - `MODEL_NAME`: GPT Model Name for response generation. Default is 4o-mini.

### 2. Data Preparation

- Extracts text and table content from PDFs using **pdfplumber**.
- Filters text to remove noise (e.g., text outside table boundaries) and excludes pages with less than 10 meaningful words.
- Adds metadata for each page, including the document name and page number.
- Filters processed data to ensure only relevant text is used.

### 3. Embedding and Semantic Search

- Uses Sentence Transformers (`all-MiniLM-L6-v2`) to generate semantic embeddings for all document pages.
- Stores embeddings in ChromaDB for efficient retrieval.

#### Query Workflow

- Checks for cached responses with a similarity threshold of 0.2. If found, retrieves results from the cache.
- If cache is missed, performs semantic search on embeddings to retrieve the top 5 results and stores them in the cache.

### 4. Cross-Encoder Reranking

- Refines the relevance of retrieved documents using the **cross-encoder model** (`ms-marco-MiniLM-L-6-v2`).
- Scores each document-query pair and reranks the results for higher accuracy.

### 5. Response Generation

- Generates responses by providing GPT with the query and top 3 reranked documents.
- Utilizes OpenAI's `gpt-4o-mini` model by default for synthesis.

### 6. Visualization and Interaction

- Users can input queries through an interactive widget.
- Visualizes relevance and reranking scores for better understanding.
- Bar charts displaying:
  - **Relevance Scores:** Scores of retrieved documents from semantic search.
  - **Reranked Scores:** Updated scores after applying cross-encoder reranking.
- Sample queries can be run through Section 7 of the notebook.

## Installation and Usage (AKA - How to run the notebook)

This notebook was created and tested in Colab. It can run locally in Jupyter by commenting out the `Mounting GDrive` section and modifying the pip command present. Colab instructions are provided below -

1. Clone this repository or download the `.ipynb` file.
2. Upload the notebook to Google Colab.
3. Replace `OPENAI_API_KEY` with your [API Key](https://platform.openai.com/account/api-keys)
4. Set the paths for `DOCUMENT_LOCATION`, `MODEL_NAME` and `CHROMA_PATH`. A sample set of documents are available in [HDFC Life Policy Documents](https://www.hdfclife.com/policy-documents)
5. Run the notebook.

## Dependencies

- **openai:** For GPT-based response generation.
- **chromadb:** For embedding storage and semantic search.
- **pdfplumber:** For PDF text and table extraction.
- **sentence-transformers:** For semantic embeddings and cross-encoder reranking.
- **ipywidgets:** For interactive query inputs.
- **matplotlib:** For visualizing document relevance and reranking scores.
- **pandas**: For processing and managing extracted data.
- **numpy**: For numerical operations during data handling.
- **ast**: For parsing cached data stored as string literals.
- **time**: To add delays or manage API rate-limiting.

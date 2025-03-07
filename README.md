# Independent_Study-RAG_Improvements-

##  RAPTOR: Hierarchical Retrieval and Summarization Framework

## Overview

RAPTOR is a hierarchical retrieval and summarization framework designed to process large-scale textual data efficiently. It follows a bottom-up summarization strategy combined with FAISS-based vector retrieval and retrieval-augmented generation (RAG) for final response synthesis.

## Architecture

RAPTOR consists of three main components:

1. **Hierarchical Summarization (Bottom-Up Information Compression)**
2. **FAISS-based Vector Retrieval**
3. **RAG for Final Response Generation**

## Workflow

### Step 1: Document Ingestion and Preprocessing

- Uses `RecursiveUrlLoader` for recursive web scraping to fetch documentation.
- Parses raw HTML into text using `BeautifulSoup`.
- Splits text into smaller semantic chunks to improve embedding and retrieval.

### Step 2: Hierarchical Summarization

#### Intra-Document Summarization (Low-Level Processing)

- Embeds text chunks using `GoogleGenerativeAIEmbeddings`.
- Clusters text chunks using Gaussian Mixture Models (GMM).
- Computes cosine similarity to filter out low-similarity chunks.
- Generates hierarchical summaries at multiple levels using an LLM (Gemini 1.5).
- Builds a hierarchical summarization tree:
  - Level 1: Raw document chunks
  - Level 2: Clustered summaries of chunks
  - Level 3: Aggregated high-level summaries across documents

#### Inter-Document Summarization (High-Level Processing)

- Stores both raw text and hierarchical summaries in a FAISS vector store.
- Enables retrieval at different levels of abstraction.
- Balances high-level context with fine-grained details.

### Step 3: FAISS Vector Store for Retrieval

- Stores raw texts and hierarchical summaries.
- Enables efficient and context-aware retrieval.
- Supports both fine-grained and abstracted retrieval.

### Step 4: RAG-based Query Processing

- Retrieves high-level summaries first (inter-document search).
- Retrieves detailed information from documents if needed (intra-document search).
- Passes structured context to an LLM (Gemini 1.5) for final response generation.
- Uses a structured prompt to synthesize responses:
  
  ```
  Answer based on context:
  {context}
  Question: {question}
  ```

## Bottom-Up Search Strategy

- RAPTOR follows a bottom-up hierarchical search mechanism:
  - **Intra-Document Summarization:** Structures fine-grained document-level details into summaries.
  - **Inter-Document Summarization:** Connects multiple documents using aggregated summaries.
  - **Final Query Execution:** Starts from broad summaries, drills down into details as needed, and generates structured responses.

## Evaluation with DeepEval

### Simple Test Results

Initial testing was conducted using `DeepEval` on basic queries. More extensive tests with larger document sets are planned.

#### Test Cases

1. **How does LangChain handle long-form text retrieval?**
   - Actual Output: LangChain employs hybrid search to retrieve long-form text efficiently.
   - Retrieval Context: LangChain combines keyword-based and dense retrieval models for handling long-form content.
   
2. **What is the role of FAISS in LangChain?**
   - Actual Output: FAISS is used in LangChain for structured prompting.
   - Retrieval Context: FAISS is integrated into LangChain to enhance document retrieval and improve search accuracy.

3. **What are the core features of LangChain's RAG pipeline?**
   - Actual Output: LangChain provides vector-based retrieval, prompt chaining, and API integrations for LLMs.
   - Retrieval Context: LangChain integrates FAISS for retrieval and enables prompt chaining across multiple models.

4. **How does LangChain handle hierarchical expression trees?**
   - Actual Output: LangChain structures expression trees using modular components to allow hierarchical processing.
   - Retrieval Context: LangChain enables hierarchical compositions using modular components for structured processing.

### Evaluation Metrics Summary

| Metric                 | Pass Rate |
|------------------------|-----------|
| Answer Relevancy      | 75.00%    |
| Faithfulness         | 100.00%   |
| Contextual Relevancy | 100.00%   |
| Hallucination        | 25.00%    |

### Observations

- The model performs well in **faithfulness and contextual relevancy**.
- Some incorrect assumptions led to **hallucination issues**, particularly with FAISS's role in LangChain.
- **Answer relevancy needs further improvements** for hierarchical expressions.

## Next Steps

- Expand evaluation with larger document sets.
- Improve hallucination handling by refining retrieval and prompt strategies.
- Optimize retrieval ranking to improve answer relevancy.
- Conduct stress testing with complex queries.

## Conclusion

RAPTOR provides a scalable and efficient hierarchical retrieval system. Its bottom-up search strategy ensures effective information compression while maintaining accuracy. Further testing and optimizations will improve its robustness for real-world applications.

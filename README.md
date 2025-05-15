# Independent_Study-RAG_Improvements-

##  RAPTOR: Hierarchical Retrieval and Summarization Framework

## Overview

RAPTOR is a two-level hierarchical retrieval system designed for scalable, interpretable information access across multiple documents. It combines bottom-up summarization with cross-document clustering and supports efficient, context-aware Retrieval-Augmented Generation (RAG).

---

## Architecture

### 1. Intra-Document Hierarchy (Local Structure)
- Documents are chunked and embedded using `BAAI/bge-small-en-v1.5`.
- Bottom-up summaries are generated with LLM (`Phi-2`), forming hierarchical trees.
- Each node captures increasing semantic abstraction, with leaf-to-root traceability.
- Node metadata includes `doc_index`, enabling linkage across inter/intra levels.

### 2. Inter-Document Raptor Tree (Global Structure)
- Intra-document roots become leaf nodes in a cross-document Raptor tree.
- Clustering (via GMM + UMAP) merges related document segments.
- Top-down summarization builds a higher-level abstraction layer.
- Enables global context-aware search across documents.

### 3. Hierarchical Retrieval Module
- Supports **bottom-up traversal**: from intra-doc leaves to inter-doc roots.
- Retrieves both **fine-grained local context** and **coarse global summaries**.
- Balances precision with abstraction, supporting long-range semantic dependencies.

---

##  Project Highlights

-  Integrated intra + inter hierarchy with shared `TreeNode` structure.
-  Unified traversal across trees using `doc_index` and metadata.
-  FAISS-based similarity search at all levels for scalable performance.
-  CUDA memory optimizations (e.g., `torch.no_grad`, `empty_cache`) for long doc support.
-  Traceable path from query to final generated answer.

---

##  Next Steps

1. **Summary Length Handling**  
   Auto-truncate or recursively re-chunk overlength LLM summaries to avoid exceeding embedding or token limits.

2. **Context Budgeting**  
    Dynamically control retrieval depth to avoid overflowing LLM input window.

3. **Metadata-Aware Grouping**  
    Use headers, timestamps, or topical structure for more interpretable inter-doc clustering.

4. **RAG Evaluation Pipeline**  
    Integrate with RAGBench/DeepEval for multi-metric performance diagnostics at each stage.

---

## Contributors

- **Sumit** & **Sagnick**: Inter/Intra hierarchy integration & hierarchical retrieval.
- **Aman**: RAGBench integration and evaluation metrics setup.



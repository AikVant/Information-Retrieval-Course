# Phase 2: Semantic Search with Transformers & FAISS

This phase implements a **Semantic Information Retrieval** system. Unlike Phase 1, which relied on exact keyword matching, this implementation uses deep learning to understand the contextual meaning of documents and queries.

## Methodology

### 1. Text Preprocessing & Cleaning

To improve the quality of the embeddings, the **IR2025** collection underwent a cleaning process using the `clean-text` library:

* **Unicode Fix:** Resolved encoding issues.

* **Casing:** Retained original casing (case-sensitive) as the Transformer model utilizes capitalization to better understand context.

* **Punctuation:** Kept punctuation to assist the tokenizer in identifying sentence structures.

* ** Output**: A new column `clean_text` was generated for both documents and queries.

### 2. Dense Vector Representations (Embeddings)

* **Model:** `sentence-transformers/all-MiniLM-L6-v2`.

* **Reasoning:** This model was chosen for its excellent balance between performance and computational efficiency (mapping text to a 384-dimensional vector space).

* **Batch Processing:** Documents were processed in batches to optimize GPU/CPU utilization.

### 3. Vector Indexing with FAISS

* **Index Type:** `IndexFlatIP` (Inner Product).

* **Process:** The document embeddings were normalized and loaded into the FAISS index.

    * For each query, a vector was generated and a **Nearest Neighbor Search** was performed.

    * The system calculated the **Cosine Similarity** to rank the top **k** documents.

## Evaluation & Comparative Analysis

The results were evaluated using **trec_eval** and compared against the BM25 baseline from Phase 1.

## Results (Model: all-MiniLM-L6-v2)
| Metric | k=20 | k=30 | k=50 |
|:---:|:---:|:---:|:---:|
| MAP | 0.3575 | 0.3862 | 0.4269 |
| P@5 |	0.7000	| 0.7000 | 0.7000 |
| P@10 | 0.5300	| 0.5300 | 0.5300 |
| P@15 | 0.4333 | 0.4333 | 0.4333 |
| P@20 | 0.3850 | 0.3850 | 0.3850 |

## Key Observations

* **Semantic vs. Lexical:** While the semantic search (Phase 2) effectively captures concepts, the lexical search (Phase 1/BM25) achieved higher scores in this specific dataset.

* **Hybrid Potential:** The high precision at low ranks (P@5) in Phase 2 suggests that semantic search is effective at identifying the most relevant documents, which sets the stage for the Hybrid Re-ranking approach in Phase 3.

## Requirements
* transformers
* sentence-transformers
* datesets
* faiss-cpu
* clean-text
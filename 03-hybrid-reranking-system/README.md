# Phase 3: Hybrid Retrieval System (Re-ranking & Fusion)

This phase implements a **Hybrid Information Retrieval** system that combines the strengths of lexical search (Elasticsearch) and semantic search (Transformers/FAISS). The goal is to improve retrieval accuracy by using a two-stage pipeline.

## Methodology

The system operates in two main stages:

### 1. Stage 1: Candidate Retrieval (BM25)

* Using the optimized **BM25** parameters from Phase 1 (b=0.8,k1​=1.8), the system retrieves the top **200 candidate documents** for each query from Elasticsearch.

* This stage acts as a high-recall filter, narrowing down the search space from 18,316 documents to the most promising 200.

### 2. Stage 2: Semantic Re-ranking (Fusion)

* For these 200 candidates, the system calculates a **Semantic Score** using the all-MiniLM-L6-v2 model and FAISS.

* **Score Fusion:** A final score is calculated for each document by combining the BM25 score and the FAISS (Cosine Similarity) score.

* The combined score formula used is:

    `Final_Score = BM25_Score + FAISS_Score`

* The documents are then re-ranked based on this fusion score, and the top k results are produced.

## Evaluation & Comparison

We evaluated three different configurations in this phase to understand the impact of each component:

* **BM25 (Baseline):** The lexical search results from Phase 1.

* **FAISS Only (on 200 docs):** Using only the semantic score of the candidates retrieved by BM25.

* **Fusion (BM25 + FAISS):** Combining both scores for the final ranking.

### Performance Results (at k=50)

| Configuration | MAP | P@5 | P@10 |
|:---|:---:|:---:|:---:|
| BM25 (Phase 1)| 0.7964 | 0.8800 | 0.7800 |
| FAISS Only	| 0.5898 | 0.8200 |	0.7200 |
| Fusion System | 0.7303 | 0.8800 | 0.7800 |

### Insights

* The Fusion system managed to maintain the high precision of the BM25 baseline while incorporating semantic information.

* The hybrid approach is particularly effective in ensuring that relevant documents which might lack exact keyword matches (but are semantically related) are promoted in the final ranking.

### Requirements

This phase requires all tools from the previous phases:

* Elasticsearch 8.x
* transformers
* Sentence-Transformers
* FAISS-cpu
* Clean-text library
* datasets
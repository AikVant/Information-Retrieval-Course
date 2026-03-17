# Phase 1: Lexical Search with Elasticsearch (BM25)

This directory contains the implementation of the baseline retrieval system using **Elasticsearch**. This phase focuses on traditional lexical matching (term matching) using the **BM25** probabilistic model to establish a performance benchmark.
## Technical Implementation

### 1. Data Preparation & Preprocessing

To make the **IR2025** collection compatible with Elasticsearch, the following steps were taken:

* **Format Conversion:** The source documents.csv file was converted to .jsonl (JSON Lines) format using UTF-8 encoding.

* **Data Loading:** The records were parsed into a list of JSON objects ready for bulk indexing.

### 2. Elasticsearch Configuration (Index Mapping)

A custom index was created with specific settings to optimize retrieval:

* **Analyzer:** Used the built-in `english` analyzer for both indexing and searching to handle tokenization and stemming .

* **Similarity Model:** The **BM25** algorithm was configured as the similarity function .

* **Fields:**

    * `ID`: Configured as a `keyword` for exact matching.

    * `Text`: Configured as `text` for full-text search capabilities.

### 3. Indexing Strategy

* Documents were indexed using the `elasticsearch.helpers.bulk` tool for high performance.

* The system uses a single shard (`number_of_shards: 1`) to maintain scoring consistency for this dataset size.

### 4. Search & Retrieval

* **Query Execution:** Queries from `queries.csv` were executed using the `match` query type against the `Text` field .

* **Output Generation:** For each query, the top k results (k=20,30,50) were retrieved and formatted for evaluation.

## Experiments & Parameter Tuning

Multiple experiments were conducted to find the optimal BM25 parameters (b and k1​). Based on the evaluation metrics, the final system uses:

    b=0.8

    k1​=1.8

### Performance Rationale

These values were chosen because they yielded the highest **MAP (Mean Average Precision)** and **Precision@k** scores across the test queries . Specifically:

* **MAP:** Showed better ranking order for relevant documents.

* **Recall:** The system achieved the highest `num_rel_ret` (number of relevant documents retrieved), minimizing information loss.

## Results Summary (k=50)
| Metric | Score |
|:---:|:---:|
| MAP | 0.7964 |
| P@5 |	0.8800 |
| P@10 | 0.7800 |
| P@20 | 0.6550 |

(Scores based on optimized parameters: b=0.8, k1​=1.8 )
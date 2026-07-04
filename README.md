# Real-Time Customer Support Intelligence Platform

## Project Overview

This project is a capstone submission for SDAIA Academy's **Modern Data Engineering for AI Systems** course.

It implements a customer support intelligence pipeline that ingests support tickets, stores them in curated lakehouse layers, builds a retrieval pipeline over resolved cases, orchestrates the workflow, and produces quality and lineage artifacts.

## Dataset

Kaggle dataset:

`suraj520/customer-support-ticket-dataset`

Records used in this run: `8469`

The Gold knowledge base and RAG pipeline exclude customer name and customer email columns.

## Pipeline Results

| Deliverable | Result |
|---|---|
| 1. Ingestion | Produced `8469` events, with `8469` valid and `0` invalid records |
| 2. Lakehouse | Bronze `8469`, Silver before MERGE `8469`, Silver after MERGE `8470`, Gold after MERGE `2771` |
| 3. RAG | Built `4829` chunks from `2771` Gold documents using `all-MiniLM-L6-v2` |
| 4. Orchestration | DAG status `success` with `3` completed tasks |
| 5. Quality & Lineage | Quality gate `True`, `9/9` expectations passed, `10` lineage events emitted |

## Implementation Details

- Ingestion is implemented with an in-memory Kafka-style producer and consumer.
- Schema validation is implemented using Pydantic.
- The lakehouse layer is implemented with Parquet-backed Bronze, Silver, and Gold zones.
- MERGE/UPSERT logic updates matching ticket IDs and inserts new ticket IDs.
- RAG uses chunking, SentenceTransformer embeddings, vector search, BM25, Reciprocal Rank Fusion, and cross-encoder reranking.
- Orchestration is implemented with a DAG-style runner, with a reference Airflow DAG generated in `dags/`.
- Quality checks are implemented with a Great Expectations-style suite.
- Lineage is emitted as OpenLineage-style JSONL events.

## Main Outputs

| Output | Path |
|---|---|
| Valid events | `outputs/ingestion/valid_events.jsonl` |
| Bronze table | `outputs/bronze/support_tickets_bronze.parquet` |
| Silver table after MERGE | `outputs/silver/support_tickets_silver_merged.parquet` |
| Gold knowledge base after MERGE | `outputs/gold/support_knowledge_base_gold_merged.parquet` |
| RAG chunks | `outputs/rag/rag_chunks.parquet` |
| RAG search results | `outputs/rag/rag_search_results.json` |
| DAG report | `outputs/orchestration/dag_run_report.json` |
| Quality report | `outputs/quality/quality_report.json` |
| Lineage events | `outputs/lineage/openlineage_events.jsonl` |
| Reference Airflow DAG | `dags/customer_support_intelligence_dag.py` |

## How to Run

Install dependencies:

```bash
pip install -r requirements.txt
```

Start JupyterLab:

```bash
jupyter lab
```

Open `capstone_project.ipynb` and run all cells from top to bottom.

## Repository Structure

```text
customer-support-intelligence-platform/
│
├── README.md
├── requirements.txt
├── capstone_project.ipynb
│
├── dags/
│   └── customer_support_intelligence_dag.py
│
├── outputs/
│   ├── ingestion/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   ├── rag/
│   ├── orchestration/
│   ├── quality/
│   ├── lineage/
│   └── final_project_summary.json
```

## Author

Reem Alamri
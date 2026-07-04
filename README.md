# Real-Time Customer Support Intelligence Platform

## Project Overview

This capstone project implements a real-time customer support intelligence platform for modern data engineering and AI systems.

The platform ingests customer support tickets, validates their schema, stores them in a lakehouse-style architecture, builds a RAG pipeline over the curated knowledge base, orchestrates the workflow using an Airflow-style DAG, and emits quality and lineage reports.

## Dataset

The project uses the Kaggle Customer Support Ticket Dataset:

`suraj520/customer-support-ticket-dataset`

PII columns such as customer name and customer email are excluded from the Gold knowledge base and RAG pipeline.

---

## Architecture

The project pipeline includes five main layers:

1. Ingestion Layer
2. Delta Lakehouse Simulation
3. RAG Pipeline
4. Orchestration Layer
5. Quality and Lineage Layer

---

## Deliverable 1 — Ingestion Layer

Components:

- Mock Kafka producer
- Mock Kafka consumer
- Pydantic schema validation
- Valid and invalid event outputs

Outputs:

- `outputs/ingestion/valid_events.jsonl`
- `outputs/ingestion/invalid_events.jsonl`

Execution result:

- Produced events: 50
- Valid events: 50
- Invalid events: 0

---

## Deliverable 2 — Delta Lakehouse Simulation

This deliverable implements a lightweight Delta Lakehouse simulation using Parquet files.

Components:

- Bronze zone
- Silver zone
- Gold zone
- Schema enforcement
- MERGE / UPSERT simulation

Execution result:

- Bronze records: 50
- Silver records before merge: 50
- Silver records after merge: 51
- Gold documents before merge: 18
- Gold documents after merge: 20
- Updated records: 1
- Inserted records: 1
- Schema enforcement: passed

Outputs:

- `outputs/bronze/support_tickets_bronze.parquet`
- `outputs/silver/support_tickets_silver_merged.parquet`
- `outputs/gold/support_knowledge_base_gold_merged.parquet`

---

## Deliverable 3 — RAG Pipeline

Components:

- Chunking
- SentenceTransformer embeddings
- Vector search using cosine similarity
- BM25 keyword search
- Hybrid search using Reciprocal Rank Fusion
- Cross-encoder reranking

Execution result:

- Gold documents: 20
- Chunks created: 36
- Embedding model: `all-MiniLM-L6-v2`
- Reranking model: `cross-encoder/ms-marco-MiniLM-L-6-v2`

Test query:

`How can I solve a product setup issue?`

Outputs:

- `outputs/rag/rag_chunks.parquet`
- `outputs/rag/rag_search_results.json`

---

## Deliverable 4 — Orchestration

The DAG connects the main modules end-to-end:

1. Ingestion
2. Lakehouse storage
3. RAG pipeline

A reference Airflow DAG file is generated under the `dags/` folder.

Execution result:

- DAG status: success
- Tasks executed:
  - task_1_ingestion
  - task_2_lakehouse_storage
  - task_3_rag_pipeline

Outputs:

- `outputs/orchestration/dag_run_report.json`
- `dags/customer_support_intelligence_dag.py`

---

## Deliverable 5 — Quality Gate and OpenLineage

Components:

- Great Expectations-style validation suite
- Quality report
- Failed quality records output
- OpenLineage-style JSONL events

Execution result:

- Quality gate passed: True
- Total expectations: 9
- Passed expectations: 9
- Failed expectations: 0
- OpenLineage events emitted: 10

Outputs:

- `outputs/quality/great_expectations_suite.json`
- `outputs/quality/quality_report.json`
- `outputs/quality/failed_quality_records.parquet`
- `outputs/lineage/openlineage_events.jsonl`

---

## How to Run

Install dependencies:

```bash
pip install -r requirements.txt
```

Run JupyterLab:

```bash
jupyter lab
```

Then open the notebook and run all cells from top to bottom.

---

## Project Notes

This project is designed as a two-day educational capstone.

Simulated components:

- Kafka is simulated using a Python `MockKafka` class.
- Delta Lake is simulated using Parquet zones with schema enforcement and MERGE-like logic.
- Airflow is simulated using an Airflow-style DAG runner.
- Great Expectations is simulated using expectation-style checks.
- OpenLineage is simulated by emitting JSONL lineage events.

The RAG pipeline uses real embedding, retrieval, hybrid search, and reranking components.

---

## Repository Structure

```text
customer_support_intelligence_platform/
│
├── README.md
├── requirements.txt
├── capstone_project_organized.ipynb
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

---

## Author

Reem Alamri
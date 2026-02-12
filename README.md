# AI for Bharat – Anveshaka Readiness

This repository contains our submission for the AI for Bharat competition.  
**Anveshaka Readiness** is a serverless readiness index system built on AWS that processes State/UT hospital beds CSV data (~39 rows, 8 columns) to generate actionable readiness scores.

## 🚀 Features
- **Serverless-first architecture** using AWS Lambda, API Gateway, S3, DynamoDB
- **Data pipeline**: ingestion → validation → scoring → explainability → retrieval
- **Explainability engine**: top 3 contributing factors per score
- **Provenance tracking**: complete lineage of datasets and transformations
- **Cost-optimized**: designed for small datasets with scalability
- **Observability**: CloudWatch logs, metrics, and alerts

## 🛠️ Tech Stack
- AWS Lambda (Python/TypeScript)
- S3 for raw/processed data
- DynamoDB for metadata and scores
- API Gateway for REST endpoints
- CloudWatch + SNS for monitoring

## 📊 Endpoints
- `POST /ingest` – Upload dataset
- `GET /scores` – Readiness scores
- `GET /details/{stateId}` – Detailed metrics
- `GET /explain/{stateId}` – Score explanations
- `GET /export/csv` – Export processed data
- `GET /provenance/{datasetId}` – Data lineage

## 🧪 Testing
- Unit tests for ingestion, validation, scoring, API
- Property-based tests (fast-check / Hypothesis) for correctness properties
- Continuous testing integrated with CI/CD

## 📜 License
MIT License – open for reuse with attribution.

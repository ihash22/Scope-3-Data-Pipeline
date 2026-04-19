# Scope 3 Unstructured Data Parser & Pipeline
**Status:** 🚧 In Active Development (Architecture Phase)

## The Problem
Calculating Scope 3 (Supply Chain) emissions is the largest bottleneck in corporate decarbonization. Enterprise sustainability teams are drowning in unstructured vendor data—sustainability reports, utility bills, and text-heavy PDFs. Currently, extracting specific consumption metrics (e.g., MWh of electricity, tons of steel) and mapping them to standardized emission factors is a highly manual, error-prone, and unscalable process.

## The Solution
An AI-assisted data pipeline designed to ingest messy, unstructured vendor sustainability PDFs, extract highly specific consumption entities using Large Language Models (LLMs), and automatically map them to standard greenhouse gas (GHG) emission factors to calculate a structured Scope 3 footprint. 

This tool acts as the bridge between raw, unstructured physical-world data and audit-ready corporate ESG metrics.

## Proposed Architecture & Tech Stack
* **Ingestion & Orchestration:** `Python`, `FastAPI`
* **Document Parsing & Chunking:** `LlamaIndex` / `PyPDF2`
* **Entity Extraction & NLP:** `LangChain`, `OpenAI API (GPT-4o)` (utilizing strict JSON schema enforcement/function calling)
* **Data Storage & Mapping:** `SQLite` / `SQLAlchemy`
* **Emissions Database Integration:** Mock EPA / Ecoinvent emission factor tables.

## System Data Flow
1. **Ingestion:** A user (or API trigger) uploads a vendor's annual sustainability report (PDF).
2. **Preprocessing:** `LlamaIndex` extracts the text, cleans OCR errors, and chunks the document contextually.
3. **LLM Extraction:** A prompt-engineered LLM agent scans the chunks specifically for consumption metrics. 
    * *Example Input:* "...our Texas facility utilized 4,500 MWh of grid electricity this year..."
    * *Structured Output:* `{"vendor": "Acme Corp", "facility_location": "TX", "activity_type": "Purchased Electricity", "amount": 4500, "unit": "MWh"}`
4. **Validation Pipeline:** The extracted JSON is validated against expected data types (e.g., Great Expectations).
5. **SQL Mapping & Calculation:** The structured data is queried against an internal SQL database of emission factors (e.g., eGRID factors for Texas) to calculate total `tCO2e`.
6. **Output:** The final footprint is appended to a master Scope 3 ledger.

## Project Roadmap
- [x] Phase 1: Architecture design and schema definition.
- [ ] Phase 2: Build the PDF ingestion and LlamaIndex chunking pipeline.
- [ ] Phase 3: Implement LangChain extraction with strict JSON output parsing.
- [ ] Phase 4: Build the SQLite mapping database and calculation engine.
- [ ] Phase 5: Containerize with Docker and deploy a simple FastAPI endpoint for testing.

---
*Developed by [Ian Hash](https://ianhash.com)


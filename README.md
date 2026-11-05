# 🔍 LedgerLens: Financial Due-Diligence Research Prototype

LedgerLens turns company financial reports into reviewable financial facts, traceable calculations, and source-linked briefings. 

Its central question is: **“What evidence supports this figure, and what changes if that evidence is corrected?”**

An analyst can inspect documents, approve extracted figures, calculate financial measures, and track corrections without ever losing the original supporting evidence.

---

## 🛑 The Problem Addressed

Financial review involves much more than finding numbers in a report. An analyst must carefully verify whether a figure:
* 🏢 Belongs to the correct company and reporting period.
* 📏 Uses exact units (dollars, thousands, or millions).
* 📑 Represents consolidated results or a specific business segment.
* 📋 Conforms to the intended accounting definition.
* 🔄 Has subsequently been corrected or withdrawn.

*Example:* Cash and cash equivalents differ from cash plus short-term investments. Similarly, reported EBITDA and adjusted EBITDA carry distinct definitions. Using the wrong input produces a mathematically correct but misleading ratio. LedgerLens makes these distinctions explicit and leaves calculations unresolved when their inputs lack sufficient support.

---

## 🏗️ Architecture & Workflow

The application runs as a local Python service. A companion Google Colab notebook executes the same Python components directly and tests the API without exposing a public server.

### 1. Document Ingestion — Preserve the Evidence
* **Supported Formats:** Accepts text, Markdown, HTML, text-based PDFs, and a limited subset of conventional XBRL financial data (scanned documents require additional OCR).
* **Data Preservation:** For each document, it retains the original file, extracted text, source metadata, timestamps, and a **SHA-256 fingerprint** to ensure exact identification.
* **References:** PDF evidence utilizes extracted page references, while HTML tables use logical sections and table locations.

### 2. Candidate Extraction — Propose Figures for Review
* **Extraction Methods:** Manual capture with exact supporting quotations, extraction of standard US-GAAP XBRL concepts, and parsing from explicitly selected simple annual HTML tables.
* **Richer Metadata Model:** Captures financial concepts, raw values/scales, normalized values, currencies, reporting periods (flows vs. balance-sheet snapshots), definitions/scopes, source quotations, and extraction details (e.g., retaining parentheses for negative cash outflows like `(64,551)` while mapping them to positive calculation values).

### 3. Analyst Review — Control Usable Facts
* **Approval Gates:** Extracted figures begin as unapproved candidates and cannot power calculations until explicitly approved by an analyst with a recorded reason.
* **Conflict Handling:** If two approved facts compete for the same financial concept, the system reports an explicit **conflict** instead of silently choosing whichever arrived last.

### 4. Financial Engine — Calculate from Accepted Inputs
* **Precision Engine:** Built using Python `Decimal` arithmetic and a fixed formula registry.
* **Supported Measures:** 
  * Gross Margin
  * Operating Margin
  * Net Debt
  * Net Debt / EBITDA
  * Operating Cash / EBITDA
  * Operating Cash less CapEx ($CapEx$)
* **Validation Checks:** Prior to calculation, the engine validates evidence availability, review status, conflicts, currency compatibility, and matching flow periods. Ratios with zero or negative denominators remain unresolved.

### 5. Revision Tracking — Explain Consequences of Changes
* **Audit Trail:** Records subsequent corrections and withdrawals, dynamically reflecting their impact on downstream calculations.
* **Preservation:** Preserves the original fact, the replacement value, the explanation, and affected calculations without overwriting historical data.

### 6. Agent Component — Limited Tool Orchestration
* **Bounded Loop:** Features a constrained agent loop restricted to three permitted actions:
  * `retrieve`: Finds relevant document sections through keyword overlap.
  * `calculate`: Runs the deterministic financial engine.
  * `finish`: Returns the source-linked financial briefing.
* **Guardrails:** Driven by a default fixed planner or an optional LLM adapter. The LLM cannot approve facts, supply financial values, or execute arbitrary code. Unsupported tools and premature completions are automatically rejected.

---

## 💾 Storage & Execution Modes

* **📓 Colab Notebook:** Writes embedded project files, runs the Python workflow, executes tests, and exports results.
* **🖥️ Local Application:** Uses FastAPI, Python calculations, and SQLite persistence.
* **🌐 Hosted Browser Demo:** Uses a JavaScript calculation mirror and browser storage (does not run the Python service).
* *Note:* SQLite stores workspace versions and snapshots for traceability, though they do not constitute independently tamper-proof audit logs.

---

## 🧪 Current Validation & Status

* 📊 **Test Suite:** 67 Python unit/integration tests and 7 JavaScript UI tests.
* 📁 **Development Benchmark:** Evaluates 12 reference records from two fiscal-year columns in a Microsoft annual report, alongside 8 controlled variations testing changed units, corrections, withdrawals, conflicts, and missing values.
* 📌 **Current Scope:** LedgerLens is a working research prototype. Its strongest contribution is its traceable evidence and revision architecture. Broad document extraction accuracy, live LLM performance tuning, and independent accounting validation remain future work.

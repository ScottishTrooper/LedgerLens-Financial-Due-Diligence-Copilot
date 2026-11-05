# 🔍 LedgerLens

**Traceable, reproducible financial report analysis.**

LedgerLens helps financial analysts review company reports, verify critical figures, and trace calculations back to their original supporting evidence. By combining document processing, financial analysis engines, and an interactive review interface, LedgerLens makes financial research transparent and audit-ready.

---

## ✨ Key Features

* 📊 **Table & Figure Extraction** – Extracts candidate figures from supported documents, including selected HTML financial tables.
* 📝 **Metadata Tracking** – Automatically records each figure’s source quotation, reporting period, currency, and exact units.
* 🛡️ **Human-in-the-Loop Review** – Requires explicit analyst approval before using extracted figures in downstream calculations.
* 📈 **Financial Metrics Engine** – Calculates 8 core financial measures, including profit margins, net debt, and cash flow after capital expenditure ($CapEx$).
* 🔄 **Audit Trail & Revisions** – Tracks figure corrections and withdrawals, dynamically reflecting their impact on the analysis.
* ⚠️ **Conflict Handling** – Leaves results unresolved when supporting evidence is missing or conflicting.
* 📄 **Reproducible Export** – Exports source-linked briefings and runs reproducible check suites directly in Google Colab.

---

## 🛠️ Tech Stack & Architecture

* 🐍 **Backend:** Python, FastAPI, SQLite, Decimal arithmetic (for precision handling)
* ⚡ **Frontend:** JavaScript
* 🧠 **Optional AI:** LLM tool-planning component
* 💻 **Zero Dependency:** The default workflow runs entirely locally (no GPU or external API key required).

---

## 🧪 Current Validation

* 🟢 **Test Suite:** 67 Python unit/integration tests & 7 JavaScript UI tests.
* 📁 **Development Benchmark:** Includes 12 reference records sourced from Microsoft’s annual report alongside 8 controlled test variations.

---

## 📌 Status & Future Work

LedgerLens is currently a working research prototype. Its main strength is traceable, reviewable financial analysis.

### Future Roadmap
- [ ] Broad document parsing accuracy improvements
- [ ] Live LLM integration performance tuning
- [ ] Independent accounting & compliance validation

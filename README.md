# Mia Ritschard Portfolio

Welcome to my professional portfolio repository.

I am an undergraduate student at the University of Hawaiʻi at Mānoa pursuing degrees in Accounting and Finance. This repository serves as a portfolio of my coursework, projects, and professional development during my time at the university.

## FIN 321 — FX Hedging Project

This portfolio includes a multi-phase foreign-exchange hedging project completed for FIN 321: International Business Finance. The project analyzes a U.S. pharmaceutical exporter expecting to receive EUR 8,000,000 and evaluates alternative strategies for managing the resulting EUR/USD exchange-rate exposure.

The project progressed from initial exposure identification and model specification through workbook construction, live market-data population, independent LLM validation, hand verification, and a final executive hedge recommendation.

### Project Artifacts

* **Phase 1 — Exposure & Hedge Framing:** [Executive Hedge-Framing Memo](./docs/decisions/2026-07-31-Ritschard-pharma-exporter-hedge-framing.md)
* **Phase 2 — Model Specification:** [FX Hedging Model Specification](./docs/specs/2026-08-07-Ritschard-pharma-exporter-spec.md)
* **Phase 3 — AI-Assisted Build & Audit:** [Workbook Build Audit](./analysis/2026-08-07-Ritschard-build-audit.md)
* **Phase 4 — Market Data & Population:** [Market-Data Memo](./data/2026-08-07-Ritschard-market-data.md)
* **Phase 5 — LLM Analysis & Validation:** [LLM Validation & Hand Verification](./analysis/2026-08-14-Ritschard-pharma-exporter-validation.md)
* **Phase 5 — Independent LLM Output:** [Raw Independent LLM Output](./analysis/2026-08-14-Ritschard-pharma-exporter-llm-output.md)
* **Phase 5 — Final Recommendation:** [Executive Hedge Recommendation](./docs/decisions/2026-08-14-Ritschard-pharma-exporter-hedge-recommendation.md)
* **AI Documentation:** [Prompt Log](./prompt-log.md)

### Final Recommendation

The completed analysis recommends fully hedging the EUR 8,000,000 receivable with a one-year forward contract. Using the Phase 4 market data, the forward locks in **$9,397,600** of USD proceeds while eliminating downside exposure to EUR depreciation and requiring no upfront option premium.

## Repository Structure

* `analysis/` — Financial analysis, workbook audits, and validation
* `data/` — Market-data documentation and project data
* `docs/` — Decision memos, plans, specifications, and supporting documentation
* `models/` — Excel models and project workbooks
* `RESUME.md` — Professional resume
* `BIO.md` — Professional biography
* `prompt-log.md` — Record of AI assistance and prompts used throughout the project

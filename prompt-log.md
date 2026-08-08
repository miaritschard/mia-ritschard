# Prompt Log

## July 24, 2026

### Task
Set up a professional GitHub portfolio repository.

### AI Assistance
Used ChatGPT to:

- Understand the repository setup instructions and technical difficulties
- Install and navigate GitHub Desktop
- Create the required folder structure

### Key Prompt
“Help me set up my FIN 321 GitHub portfolio repository and make sure it meets the assignment requirements.”

### Outcome
Created and published a public GitHub repository with the required folders and documentation.


## August 7, 2026 — Phase 2: Model Specification

### Task

Develop a technical specification for the U.S. Pharmaceutical Exporter FX hedging model before building the Excel workbook. The specification needed to define the inputs, workbook architecture, calculation flow, sensitivity analysis, validation rules, and required outputs clearly enough for the workbook to be built from the specification alone.

### AI Assistance

Used ChatGPT to:

- Interpret the Phase 2 requirements and rubric
- Organize the technical specification
- Define the ten required named ranges
- Specify the forward, money-market, and option hedge calculations
- Develop the sensitivity and validation plans
- Review the specification for reproducibility and missing information

### Key Prompt

“Using my assigned Scenario #2, U.S. Pharmaceutical Exporter, help me draft a Phase 2 technical specification that follows the FIN 321 requirements and can be used to build the complete workbook in Phase 3.”

### Human Review and Iteration

During review, I noticed that my assigned scenario row only explicitly provided the EUR 8,000,000 receivable and the indicative 1-year forward rate of 1.0890 USD/EUR. The remaining market inputs had not been provided as scenario-specific values.

I corrected the draft so that the additional Phase 2 values are clearly identified as indicative modeling placeholders rather than being presented as professor-provided data. I also specified that these market inputs will be replaced with documented live market data during Phase 4. This distinction improves the model's data provenance and prevents placeholder assumptions from being mistaken for actual market observations.

### Outcome

Completed a reproducible technical specification for the U.S. Pharmaceutical Exporter FX hedging workbook. The specification defines all required named ranges, tabs, hedge calculations, sensitivity analysis, validation checks, outputs, and the Phase 4 population plan.


## August 7, 2026 — Phase 3: AI-Assisted Build + Audit

### Task

Build the Phase 3 Excel workbook directly from the Phase 2 technical specification and audit the completed model against the required build contract.

### AI Assistance

Used ChatGPT to:

- Generate the Excel workbook structure from the Phase 2 specification
- Create the required tabs and all ten named ranges
- Build formula-driven forward, money-market, put, and call hedge calculations
- Create the 11-row sensitivity analysis and comparison chart
- Add visible validation checks
- Review the workbook for formula errors and model consistency
- Draft the Phase 3 audit note

### Key Prompt

“Create the Phase 3 FX hedging workbook from my Phase 2 specification, including all required named ranges, formula-driven hedge calculations, sensitivity analysis, chart, color conventions, and validation checks.”

### Human Review and Iteration

After reviewing the initial model output, I checked whether the placeholder interest-rate assumptions were consistent with the scenario-provided indicative forward rate of 1.0890 USD/EUR.

The first placeholder rate combination created a larger covered-interest-parity difference than desired. I revised the illustrative Phase 3 interest-rate assumptions so that the CIP-implied forward was approximately 1.08894 and the parity check returned PASS. These values remain clearly labeled as placeholders and will be replaced with live market data in Phase 4.

I also reviewed the sensitivity table, confirmed that it contained 11 formula-driven scenarios from -5% to +5%, and confirmed that the put calculation remained continuous around the strike.

### Outcome

Completed and audited the Phase 3 FX hedging workbook. The workbook contains all required named ranges, formula-driven hedge calculations, a three-step money-market hedge, option calculations, a sensitivity table and chart, and visible validation checks. The audit documented three substantive findings and confirmed that the model is ready for live-data population in Phase 4.

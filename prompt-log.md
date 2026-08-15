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


## August 7, 2026 — Phase 4: Market Data + Population

### Task

Replace the Phase 3 placeholder inputs with current market data, repopulate the existing FX hedging workbook, rerun the model checks, and cross-check the results against the FIN 321 FX Hedging Lab.

### AI Assistance

Used ChatGPT to:

- Identify suitable live market-data sources for EURUSD spot and interest rates
- Calculate the covered-interest-parity implied forward rate
- Populate the existing workbook with the Phase 4 inputs
- Recalculate the forward, money-market, option, and sensitivity outputs
- Compare workbook results against the FIN 321 FX Hedging Lab
- Identify and correct a difference in option-premium treatment
- Draft the Phase 4 market-data memo and document the cross-check

### Key Prompt

“Help me replace my Phase 3 placeholder inputs with current market data, repopulate the existing workbook, rerun the validation checks, and compare the outputs against the FIN 321 FX Hedging Lab.”

### Human Review and Iteration

I reviewed the market-data choices and used an ECB EURUSD reference rate for spot, a 1-year U.S. Treasury rate for the USD rate, and the ECB €STR as a documented euro reference-rate proxy. Because I did not use a reliable free 1-year forward quote, I used covered interest parity to calculate the Phase 4 forward rate.

During the FX Hedging Lab cross-check, the forward and money-market hedge results matched the workbook. However, I identified a discrepancy in the option results. The original workbook carried the option premium forward at the USD interest rate, while the FX Hedging Lab deducted the premium directly.

I corrected the workbook's option formulas to use the same premium treatment as the Lab. After the correction, the put-hedge result at the 0% sensitivity case was $9,068,000, matching the Lab. I reran the sensitivity analysis and confirmed that the formulas recalculated without Excel errors.

### Outcome

Completed the Phase 4 market-data population and validation. The workbook now uses the documented market inputs, the covered-interest-parity check passes, the sensitivity analysis recalculates around the live spot rate, and the forward, money-market, and option results were cross-checked against the FIN 321 FX Hedging Lab. The identified option-premium discrepancy was resolved before submission.

## August 14, 2026 — Phase 5: LLM Analysis & Validation

### Task

Conduct an independent LLM execution of the U.S. Pharmaceutical Exporter hedge analysis, compare the LLM results with the completed workbook, verify key hedge outcomes by hand, make a final executive hedge recommendation, and evaluate the strengths and weaknesses of the Phase 2 specification.

### AI Assistance

Used ChatGPT to:

* Conduct a fresh independent hedge analysis using only the Phase 2 Model Specification and Phase 4 Market-Data Memo
* Calculate and compare the unhedged, forward, money-market, EUR put, and EUR call outcomes
* Compare the independent LLM results with the completed Phase 4 workbook
* Select representative -5%, 0%, and +5% sensitivity scenarios for validation
* Recompute the forward hedge, three-step money-market hedge, and EUR put outcome by hand
* Diagnose potential discrepancies and specification ambiguities
* Evaluate the tradeoffs among certainty, flexibility, upside participation, liquidity, and premium cost
* Develop the final recommendation to use the forward hedge
* Review the Phase 2 specification and identify improvements for a future version

### Key Prompt

“Using only these two documents, independently compute the complete FX hedge analysis for the U.S. Pharmaceutical Exporter scenario. Calculate and compare the USD proceeds for the unhedged position, forward hedge, money-market hedge, EUR put option, and EUR call option. Evaluate the strategies across the ending spot-rate sensitivity points specified in the documents, show the formulas and calculations, and recommend the most appropriate hedge based solely on the information contained in the two documents.”

### Human Review and Iteration

I preserved the initial LLM response without correcting or coaching it during the independent execution. I then compared the LLM results with the Phase 4 workbook at the -5%, 0%, and +5% sensitivity points. The LLM and workbook results matched for all five strategies at the selected points.

I independently hand-verified the forward hedge, all three steps of the money-market hedge, and the EUR put outcome under a 5% EUR depreciation scenario. These calculations reconciled to the workbook and the independent LLM output.

Although there were no numerical discrepancies in the selected comparison cases, the validation revealed a specification ambiguity involving option-premium treatment. Phase 2 described carrying the option premium forward to maturity, while Phase 4 documented the correction to deduct the $160,000 premium directly. The independent LLM correctly treated the Phase 4 convention as controlling. I also identified the need for a clearer rounding policy and a more explicit explanation that the EUR call is included for comparison rather than as an economically appropriate primary hedge for an EUR receivable.

### Outcome

Completed the Phase 5 independent LLM execution, workbook comparison, hand verification, specification retrospective, and executive recommendation. The analysis supports fully hedging the EUR 8,000,000 receivable with a forward contract, which locks in $9,397,600 of USD proceeds with no upfront option premium. The Phase 5 validation also identified specific improvements that would make a future version of the model specification more precise and reproducible.

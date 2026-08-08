# FX Hedging Model Specification — U.S. Pharmaceutical Exporter

**Author:** Amelia Ritschard  
**Course:** FIN 321 — International Business Finance, Sec. 701  
**Scenario:** U.S. Pharmaceutical Exporter  
**Scenario Slug:** pharma-exporter  
**Date:** August 7, 2026  

> **Data status:** Unless specifically identified as scenario-provided, all market values in this specification are indicative modeling placeholders only and will be replaced with live market data at Phase 4.

## 1. Problem Statement

The U.S. Pharmaceutical Exporter expects to receive EUR 8,000,000 from a European customer in approximately one year. Because the company reports and operates in U.S. dollars, the euro-denominated receivable creates foreign-exchange transaction exposure.

The primary risk is depreciation of the euro against the U.S. dollar before the receivable is collected. If EURUSD falls, each euro received will convert into fewer U.S. dollars, reducing the dollar value of the receivable and potentially reducing the firm's expected cash flow and profit margin.

The workbook will compare three hedge families: a forward hedge, a money-market hedge, and currency-option strategies. The model will show the resulting USD proceeds under each strategy and how those proceeds respond to changes in the ending EURUSD spot rate, S_T.

The scenario-provided foreign-currency amount is EUR 8,000,000, and the indicative one-year EURUSD forward rate is 1.0890 USD/EUR. All other market inputs below are indicative placeholders used for model construction and will be replaced with live market data in Phase 4.

## 2. Inputs — Named-Range Contract

The workbook must use the following ten named ranges exactly. No cell-address references should be required to understand the model.

| Named Range | Phase 2 Placeholder | Unit | Meaning | Phase 4 Source |
|---|---:|---|---|---|
| FC_AMT | 8,000,000 | EUR | Foreign-currency receivable | Assigned scenario; remains fixed |
| S0_in | 1.0900 | USD/EUR | Current spot exchange rate | Live EURUSD spot from Yahoo Finance, ECB, Bloomberg, or comparable source |
| F0_in | 1.0890 | USD/EUR | One-year forward rate | Scenario-provided indicative rate; replaced by live quote or CIP-implied forward |
| R_USD | 0.0500 | decimal annual rate | One-year USD interest rate | Current U.S. government yield or appropriate USD reference/deposit rate |
| R_FC | 0.0300 | decimal annual rate | One-year EUR interest rate | Current euro-area government/reference/deposit rate |
| K_PUT | 1.0900 | USD/EUR | EUR put option strike | Set at or near live spot according to scenario convention |
| K_CALL | 1.0900 | USD/EUR | EUR call option strike | Set at or near live spot according to scenario convention |
| PREM_PUT | 0.0200 | USD per EUR | Put premium | Scenario assumption retained at Phase 4 |
| PREM_CALL | 0.0200 | USD per EUR | Call premium | Scenario assumption retained at Phase 4 |
| T_DAYS | 360 | days | Time until receipt/hedge maturity | Assigned one-year scenario |

The placeholders for S0_in, R_USD, R_FC, K_PUT, K_CALL, PREM_PUT, and PREM_CALL are not claimed to be live quotes. They exist only so that the Phase 3 workbook can be built and tested. They are **indicative — replaced with live market data at Phase 4**, except option premiums, which Phase 4 instructions require to remain scenario assumptions.

## 3. Workbook Tab Architecture

The workbook will contain the following tabs:

### Cover
Identifies the scenario, author, course, model date, exposure amount, exposure direction, and data-provenance status. It must state that Phase 2/3 market values are indicative placeholders and that live market data will be populated in Phase 4.

### Legend-Key
Explains the workbook color convention used consistently throughout:
- Yellow = inputs
- Blue = assumptions
- Green = formulas/calculated cells
- Gray = outputs

It will also define important notation, including S0 as current spot and S_T as ending spot.

### Inputs
Contains all ten required named-range input cells, units, descriptions, and source/provenance notes. Each named range must be attached to the correct input cell.

### Forward-Hedge
Calculates the locked USD proceeds from selling the EUR receivable forward at F0_in.

### Money-Market-Hedge
Displays the money-market hedge in three explicit steps: borrowing the present value of the EUR receivable, converting the borrowed EUR to USD at spot, and investing the USD until maturity. It also contains the covered-interest-parity check.

### Options-Hedge
Calculates put and call option outcomes as functions of S_T. Premiums must be shown separately in USD and incorporated into net proceeds. The put is the economically relevant protective option for an EUR receivable because it establishes a minimum conversion rate while preserving upside if EUR appreciates. The call is included as a required comparison strategy but is not the primary hedge recommendation for this receivable.

### Sensitivity
Contains a formula-driven sensitivity table for ending spot rates from 95% to 105% of S0_in in 1% increments. It compares unhedged, forward, money-market, put-option, and call-option USD proceeds and contains one comparison chart.

### Notes-Assumptions
Documents conventions, data limitations, rate basis, premium treatment, transaction-cost assumptions, and Phase 4 data-source requirements.

## 4. Assumptions and Constraints

1. Exchange rates are quoted as USD per EUR.
2. FC_AMT represents a EUR receivable, so the firm's principal FX risk is EUR depreciation.
3. Interest calculations use simple interest and an ACT/360-style convention represented by T_DAYS/360.
4. Transaction costs, bid-ask spreads, taxes, credit risk, and counterparty default risk are ignored.
5. The entire EUR 8,000,000 exposure is hedged under each strategy for comparison.
6. The forward contract is assumed to require no upfront premium.
7. Covered interest parity is expected to approximately connect spot, USD interest rates, EUR interest rates, and the forward rate.
8. Option premiums are quoted in USD per EUR. Total premium equals the quoted premium multiplied by FC_AMT.
9. For maturity-value comparisons, the option premium will be carried forward at R_USD for the remaining hedge horizon. This places the premium cost on the same maturity-date basis as the hedge proceeds.
10. S_T is expressed in USD/EUR.
11. Phase 2 market values are indicative placeholders. Phase 4 will replace the appropriate values with live market data without changing the workbook's calculation structure.

## 5. Calculation Flow

All calculations must use named ranges and formulas. Calculated outputs must never be hard-coded.

### A. Unhedged Position

At any ending spot rate S_T:

**Unhedged USD Proceeds**

`FC_AMT × S_T`

This serves as the benchmark for evaluating hedge outcomes.

### B. Forward Hedge

The exporter sells the future EUR receivable forward.

**Forward USD Proceeds**

`FC_AMT × F0_in`

The forward proceeds must not depend on S_T.

Using the assigned indicative forward:

`8,000,000 × 1.0890 = 8,712,000 USD`

This is a Phase 2 check figure only and will change when F0_in is replaced in Phase 4.

### C. Money-Market Hedge

The money-market hedge must appear as three separate steps.

**Step 1 — Borrow the present value of the future EUR receivable**

`EUR Borrowed Today = FC_AMT / (1 + R_FC × T_DAYS / 360)`

The EUR borrowing grows to approximately FC_AMT at maturity, allowing the receivable to repay the borrowing.

**Step 2 — Convert borrowed EUR into USD at current spot**

`USD Available Today = EUR Borrowed Today × S0_in`

**Step 3 — Invest USD until the receivable date**

`Money-Market USD Proceeds = USD Available Today × (1 + R_USD × T_DAYS / 360)`

The resulting USD maturity value is the money-market hedge proceeds.

### D. Covered Interest Parity Check

Calculate the forward rate implied by spot and interest rates:

`CIP-Implied Forward = S0_in × (1 + R_USD × T_DAYS / 360) / (1 + R_FC × T_DAYS / 360)`

Then calculate:

`Parity Difference = F0_in - CIP-Implied Forward`

and

`Parity Difference % = ABS(F0_in - CIP-Implied Forward) / F0_in`

With internally consistent market inputs, the forward hedge and money-market hedge should produce approximately equal USD proceeds. Any material difference must be investigated and documented.

### E. EUR Put Option

Because the firm will receive EUR, purchasing a put on EUR protects against EUR depreciation while allowing the firm to benefit if EUR appreciates.

**Put Gross Conversion Rate**

`MAX(K_PUT, S_T)`

**Put Gross USD Proceeds**

`FC_AMT × MAX(K_PUT, S_T)`

**Put Premium Paid Today**

`FC_AMT × PREM_PUT`

**Put Premium at Maturity**

`FC_AMT × PREM_PUT × (1 + R_USD × T_DAYS / 360)`

**Put Net USD Proceeds**

`FC_AMT × MAX(K_PUT, S_T) - Put Premium at Maturity`

The calculation must be continuous around K_PUT. Below the strike, exercise protects the conversion rate. Above the strike, the put expires unexercised and the EUR receivable is converted at S_T.

### F. EUR Call Option Comparison

The workbook will also calculate the required call-option comparison.

**Call Payoff per EUR**

`MAX(S_T - K_CALL, 0)`

**Call Gross USD Proceeds**

`FC_AMT × S_T + FC_AMT × MAX(S_T - K_CALL, 0)`

**Call Premium Paid Today**

`FC_AMT × PREM_CALL`

**Call Premium at Maturity**

`FC_AMT × PREM_CALL × (1 + R_USD × T_DAYS / 360)`

**Call Net USD Proceeds**

`FC_AMT × S_T + FC_AMT × MAX(S_T - K_CALL, 0) - Call Premium at Maturity`

The call calculation is included for model completeness and comparison. A long EUR call does not provide the same downside protection as a long EUR put for an exporter already receiving EUR.

## 6. Sensitivity Plan

The Sensitivity tab will test ending EURUSD spot rates around the current input S0_in.

S_T will run from:

`0.95 × S0_in`

through:

`1.05 × S0_in`

in increments of:

`0.01 × S0_in`

This produces 11 scenarios corresponding to -5%, -4%, -3%, -2%, -1%, 0%, +1%, +2%, +3%, +4%, and +5% relative to S0_in.

The sensitivity table must be formula-driven. The S_T rows must be calculated from S0_in and the percentage changes rather than manually typing exchange rates.

The table will contain the following columns exactly:

1. `Spot Change %`
2. `Ending Spot S_T`
3. `Unhedged USD Proceeds`
4. `Forward USD Proceeds`
5. `Money-Market USD Proceeds`
6. `Put Net USD Proceeds`
7. `Call Net USD Proceeds`

One comparison line chart will plot Ending Spot S_T on the horizontal axis and USD proceeds on the vertical axis for the five strategies. Forward and money-market proceeds should appear approximately flat across ending spot scenarios, while unhedged and option results will respond to S_T.

## 7. Validation Rules and Check Figures

The following checks will be visible in the workbook and will become the Phase 3 audit checklist.

### Check 1 — Forward Calculation
Using the scenario-provided indicative forward:

`FC_AMT × F0_in = 8,000,000 × 1.0890 = 8,712,000 USD`

The Phase 3 placeholder workbook should return this amount before Phase 4 replaces F0_in.

### Check 2 — Money-Market / CIP Parity
The workbook must calculate the CIP-implied forward using S0_in, R_USD, R_FC, and T_DAYS.

The parity check will display `PASS` when the percentage difference between F0_in and the CIP-implied forward is within a clearly displayed tolerance, and otherwise display `REVIEW`.

Because the assigned 1.0890 forward and the other Phase 2 market inputs come from different placeholder assumptions, exact parity is not required before live data population. In Phase 4, if F0_in is calculated from CIP, the parity difference should approach zero aside from rounding.

### Check 3 — Forward vs. Money-Market Proceeds
When F0_in equals the CIP-implied forward:

`Forward USD Proceeds ≈ Money-Market USD Proceeds`

A difference beyond rounding tolerance must be investigated.

### Check 4 — Put Continuity
Immediately below and above K_PUT, put net proceeds must transition correctly without a formula error or artificial discontinuity. At S_T = K_PUT, exercise and non-exercise values must meet at the same gross conversion value.

### Check 5 — Formula Integrity
Every calculated output must contain a formula. No calculated hedge proceeds, sensitivity outputs, parity results, or option payoffs may be pasted or manually hard-coded.

### Check 6 — Error Check
The completed workbook must contain no `#REF!`, `#DIV/0!`, `#VALUE!`, `#NAME?`, or other Excel error values.

### Check 7 — Sensitivity Range
The sensitivity table must contain exactly 11 ending-spot scenarios from 95% through 105% of S0_in in 1% increments and must automatically recalculate if S0_in changes.

## 8. Required Outputs

The workbook will display the following summary outputs using these labels exactly:

- `Unhedged USD Proceeds`
- `Forward USD Proceeds`
- `EUR Borrowed Today`
- `USD Available Today`
- `Money-Market USD Proceeds`
- `CIP-Implied Forward`
- `Parity Difference`
- `Parity Difference %`
- `Put Premium Paid Today`
- `Put Premium at Maturity`
- `Put Net USD Proceeds`
- `Call Premium Paid Today`
- `Call Premium at Maturity`
- `Call Net USD Proceeds`
- `Parity Check Status`

The sensitivity table will be titled:

`FX Hedge Sensitivity Analysis`

The comparison chart will be titled:

`USD Proceeds by Hedge Strategy vs. Ending EURUSD Spot`

## 9. Reproducibility Requirement

A person or AI receiving only this specification should be able to construct the workbook without needing the Phase 1 memo or additional verbal instructions. All required inputs, tab names, calculation sequences, formulas, assumptions, validation checks, outputs, and sensitivity requirements are defined above.

The Phase 3 build must follow this specification. If the builder requires additional explanation that should reasonably have appeared here, the specification will be corrected and recommitted before the workbook is regenerated.

## 10. Phase 4 Population Plan

At Phase 4, the model structure and formulas will remain unchanged. The following values will be populated with live or documented market data: S0_in, R_USD, R_FC, F0_in, K_PUT, and K_CALL. PREM_PUT and PREM_CALL will remain scenario assumptions as directed. FC_AMT and T_DAYS remain based on the assigned scenario.

Every Phase 4 input will be documented with its value, source, retrieval timestamp, and any proxy or computation used. The model will then be revalidated, the sensitivity table will recalculate around the live S0_in, and the workbook outputs will be cross-checked against the FIN 321 FX Hedging Lab.

# Phase 4 Market Data — U.S. Pharmaceutical Exporter

**Author:** Amelia Ritschard  
**Course:** FIN 321 — International Business Finance, Sec. 701  
**Scenario:** U.S. Pharmaceutical Exporter  
**Data Date:** August 7, 2026  

## Market Data Summary

The Phase 3 workbook was initially constructed using indicative placeholder market values. For Phase 4, the appropriate inputs are replaced with current market data. The assigned exposure amount and maturity remain unchanged, while the option premiums remain modeling assumptions as directed by the assignment.

| Input | Phase 4 Value | Source | Retrieval / Reference Time | Notes |
|---|---:|---|---|---|
| `FC_AMT` | 8,000,000 EUR | Assigned FIN 321 Scenario #2 | August 7, 2026 | Scenario-provided EUR receivable; unchanged |
| `S0_in` | 1.1535 USD/EUR | European Central Bank EUR/USD reference rate | August 7, 2026 | ECB reference rate: EUR 1 = USD 1.1535 |
| `F0_in` | 1.1747 USD/EUR | CIP-implied calculation | August 7, 2026 | Computed from live spot and selected USD/EUR rates because a reliable free 1-year forward quote was not used |
| `R_USD` | 0.0406 | FRED / Federal Reserve, 1-Year Treasury Constant Maturity Rate | Observation: August 6, 2026; retrieved August 7, 2026 | 4.06% annual rate; latest available observation when retrieved |
| `R_FC` | 0.02185 | European Central Bank, €STR | Reference date: August 6, 2026; published August 7, 2026 at 08:00 | 2.185% annualized euro reference-rate proxy |
| `K_PUT` | 1.1535 USD/EUR | Set at live spot | August 7, 2026 | At-the-money strike based on live `S0_in` |
| `K_CALL` | 1.1535 USD/EUR | Set at live spot | August 7, 2026 | At-the-money strike based on live `S0_in` |
| `PREM_PUT` | 0.0200 USD/EUR | Phase 3 modeling assumption | August 7, 2026 | Retained as required rather than attempting to obtain a retail option quote |
| `PREM_CALL` | 0.0200 USD/EUR | Phase 3 modeling assumption | August 7, 2026 | Retained as required rather than attempting to obtain a retail option quote |
| `T_DAYS` | 360 days | Assigned one-year scenario | August 7, 2026 | Course ACT/360 convention |

## Source and Rate Selection

### EURUSD Spot Rate

I used the European Central Bank reference rate for August 7, 2026. The ECB reported EUR 1 = USD 1.1535. I selected the ECB because it provides an official and reproducible daily reference rate.

### U.S. Interest Rate

For `R_USD`, I used the 1-Year U.S. Treasury Constant Maturity Rate reported through FRED. The latest available observation at retrieval was 4.06% for August 6, 2026. I selected this rate because the hedge horizon is approximately one year and the assignment permits a government yield as the USD rate.

### Euro Interest Rate

For `R_FC`, I used the ECB euro short-term rate (€STR) as a euro reference-rate proxy. The ECB published a rate of 2.185% on August 7, 2026 for the August 6 reference date. Although €STR is an overnight reference rate rather than a one-year government yield, the Phase 4 instructions allow a deposit/reference rate, and the source is official and reproducible.

## CIP-Implied Forward

Because I did not use a reliable freely accessible live 1-year EURUSD forward quote, I calculated `F0_in` using covered interest parity:

`F0 = S0 × (1 + R_USD × T/360) / (1 + R_FC × T/360)`

Using the Phase 4 inputs:

`F0 = 1.1535 × (1 + 0.0406 × 360/360) / (1 + 0.02185 × 360/360)`

`F0 = approximately 1.1747 USD/EUR`

Therefore:

`F0_in = 1.1747`

The original scenario's indicative forward was 1.0890 USD/EUR. The Phase 4 CIP-implied forward is approximately 0.0857 USD/EUR higher, or about 7.9% above the indicative scenario value. The difference reflects the fact that the scenario value was an illustrative placeholder, while the Phase 4 calculation uses market data observed in August 2026.

## Strike and Premium Assumptions

Both `K_PUT` and `K_CALL` were set equal to the live spot rate of 1.1535 USD/EUR, creating at-the-money comparison strikes.

`PREM_PUT` and `PREM_CALL` remain 0.0200 USD per EUR. These values are retained as model assumptions because the assignment instructs students to keep the scenario/model premiums rather than rely on potentially inconsistent retail-accessible option quotes.

## Model Population and Validation

The Phase 4 values will be entered only into the workbook's named-range input cells. No hedge formulas should require modification.

After population, the following checks will be rerun:

- Covered-interest-parity check
- Forward versus money-market proceeds check
- Put continuity check
- Formula integrity and Excel error check
- Sensitivity range from 95% to 105% of the new `S0_in`
- Sensitivity chart recalculation

Because `F0_in` is calculated directly from covered interest parity using the selected spot and interest rates, the forward and money-market hedge results should agree aside from rounding.

I entered the Phase 4 market inputs into the FIN 321 FX Hedging Lab and compared its results with the populated Excel workbook.

The FX Hedging Lab produced a forward hedge value of $9,397,600 and money-market hedge proceeds of approximately $9,397,325. These results matched the workbook. The Lab also calculated a CIP-implied forward of approximately 1.1747 USD/EUR, matching the quoted `F0_in` of 1.1747. The approximately $275 difference between the forward and money-market proceeds is attributable to rounding, and the Lab confirmed that covered interest parity holds.

The sensitivity analysis also produced the required 11 scenarios from -5% through +5% around the live spot rate. At the 0% ending-spot scenario of 1.1535 USD/EUR, the Lab calculated unhedged proceeds of $9,228,000, forward proceeds of $9,397,600, and put-hedge proceeds of $9,068,000.

During this comparison, I identified a discrepancy in the option-premium treatment. The initial workbook carried the option premium forward using the USD interest rate, while the FX Hedging Lab deducts the $160,000 premium directly from option proceeds without financing the premium to maturity.

I revised the workbook's option formulas to match the FX Hedging Lab convention. After this correction, the workbook's put-hedge proceeds at the 0% scenario equal $9,068,000, matching the Lab. I also reran the sensitivity calculations and confirmed that the corrected option formulas recalculate across all 11 ending-spot scenarios without formula errors.

The Phase 4 cross-check therefore confirmed the forward and money-market calculations and identified and resolved the option-premium treatment difference before submission.

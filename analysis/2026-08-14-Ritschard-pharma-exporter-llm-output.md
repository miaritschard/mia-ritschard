# Phase 5 — Independent LLM Raw Output

**Student:** Amelia Ritschard
**Course:** FIN 321 — International Business Finance, Sec. 701
**Scenario:** U.S. Pharmaceutical Exporter
**Date:** August 14, 2026

## Independent Execution Setup

This file preserves the raw output from the Phase 5 independent LLM execution. The LLM session was opened as a fresh conversation with no prior project history. It was provided only the Phase 2 Model Specification and Phase 4 Market-Data Memo. No workbook results were supplied, and the LLM was not corrected or coached during the run.

## Prompt Submitted to the LLM

Here are the only two documents you should use for this analysis:

Phase 2 Model Specification:
https://github.com/miaritschard/mia-ritschard/blob/main/docs/specs/2026-08-07-Ritschard-pharma-exporter-spec.md

Phase 4 Market-Data Memo:
https://github.com/miaritschard/mia-ritschard/blob/main/data/2026-08-07-Ritschard-market-data.md

Using only these two documents, independently compute the complete FX hedge analysis for the U.S. Pharmaceutical Exporter scenario.

Calculate and compare the USD proceeds for the following strategies:

* Unhedged position
* Forward hedge
* Money-market hedge
* EUR put option
* EUR call option

Evaluate the strategies across the ending spot-rate (S_T) sensitivity points specified in the documents. Show the formulas, assumptions, calculations, and resulting USD proceeds clearly.

Then compare the strategies in terms of downside protection, upside participation, certainty, flexibility, and cost, and recommend the hedge strategy you believe is most appropriate based solely on the information contained in the two documents.

Do not use any information outside the two provided documents.

---

## Raw LLM Response

Using **only the Phase 2 Model Specification and Phase 4 Market-Data Memo you provided**, here is the independent hedge analysis. One important reconciliation point is that Phase 4 explicitly changes the option-premium convention: although Phase 2 originally carries premiums forward to maturity, Phase 4 says the workbook was revised to deduct the **$160,000 premium directly**, matching the FX Hedging Lab. I therefore use that corrected Phase 4 convention.

### 1. Inputs and assumptions

The exporter expects to receive **EUR 8,000,000** in one year. The Phase 4 inputs are:

| Input             |          Value |
| ----------------- | -------------: |
| EUR receivable    |     €8,000,000 |
| Current spot, S0  | 1.1535 USD/EUR |
| Forward rate, F0  | 1.1747 USD/EUR |
| USD interest rate |          4.06% |
| EUR interest rate |         2.185% |
| Put strike        | 1.1535 USD/EUR |
| Call strike       | 1.1535 USD/EUR |
| Put premium       |    $0.0200/EUR |
| Call premium      |    $0.0200/EUR |
| Time              |       360 days |

The specification assumes USD/EUR quotations, simple interest using a 360-day convention, the full exposure hedged under each strategy, and no transaction costs, taxes, bid-ask spreads, credit risk, or counterparty default risk.

The total option premium under either option strategy is:

`8,000,000 × 0.0200 = $160,000`

The sensitivity analysis runs from **95% to 105% of the current spot rate in 1% increments**, producing 11 ending-spot scenarios.

### 2. Hedge calculations

#### Unhedged position

The specification defines:

`USD Proceeds = FC_AMT × S_T`

Therefore:

`Unhedged = 8,000,000 × S_T`

There is no protection against euro depreciation, but the company receives the full benefit if the euro appreciates.

#### Forward hedge

The exporter sells the EUR 8 million receivable forward:

`Forward Proceeds = FC_AMT × F0`

`= 8,000,000 × 1.1747`

`= $9,397,600`

This amount is fixed regardless of S_T.

#### Money-market hedge

First, borrow the present value of the EUR receivable:

`8,000,000 / (1 + 0.02185) = approximately €7,828,938`

Convert those euros into dollars at spot and invest the dollars for one year:

`[8,000,000 / (1.02185)] × 1.1535 × 1.0406`

`= $9,397,325.24`

The Phase 4 memo reports approximately **$9,397,325**, confirming this calculation. The approximately $275 difference from the forward hedge results from rounding because the forward rate was rounded to 1.1747.

#### EUR put option

The put protects the exporter against EUR depreciation while preserving appreciation potential.

Using the corrected Phase 4 premium treatment:

`Put Net Proceeds = 8,000,000 × max(1.1535, S_T) - 160,000`

When `S_T ≤ 1.1535`:

`8,000,000 × 1.1535 - 160,000 = $9,068,000`

Above the strike, the exporter allows the put to expire and converts the EUR at the higher spot rate, less the $160,000 premium.

#### EUR call option

Following the exact comparison formula specified in Phase 2, but using Phase 4's corrected premium treatment:

`Call Net Proceeds = 8,000,000 × S_T + 8,000,000 × max(S_T - 1.1535, 0) - 160,000`

The specification explicitly notes that the long EUR call is included for comparison and **does not provide the same downside protection as the put** for an exporter already receiving EUR.

### 3. Sensitivity analysis

| Change |      S_T |   Unhedged |    Forward | Money Market |    EUR Put |   EUR Call |
| -----: | -------: | ---------: | ---------: | -----------: | ---------: | ---------: |
|    -5% | 1.095825 | $8,766,600 | $9,397,600 |   $9,397,325 | $9,068,000 | $8,606,600 |
|    -4% | 1.107360 | $8,858,880 | $9,397,600 |   $9,397,325 | $9,068,000 | $8,698,880 |
|    -3% | 1.118895 | $8,951,160 | $9,397,600 |   $9,397,325 | $9,068,000 | $8,791,160 |
|    -2% | 1.130430 | $9,043,440 | $9,397,600 |   $9,397,325 | $9,068,000 | $8,883,440 |
|    -1% | 1.141965 | $9,135,720 | $9,397,600 |   $9,397,325 | $9,068,000 | $8,975,720 |
|     0% | 1.153500 | $9,228,000 | $9,397,600 |   $9,397,325 | $9,068,000 | $9,068,000 |
|    +1% | 1.165035 | $9,320,280 | $9,397,600 |   $9,397,325 | $9,160,280 | $9,252,560 |
|    +2% | 1.176570 | $9,412,560 | $9,397,600 |   $9,397,325 | $9,252,560 | $9,437,120 |
|    +3% | 1.188105 | $9,504,840 | $9,397,600 |   $9,397,325 | $9,344,840 | $9,621,680 |
|    +4% | 1.199640 | $9,597,120 | $9,397,600 |   $9,397,325 | $9,437,120 | $9,806,240 |
|    +5% | 1.211175 | $9,689,400 | $9,397,600 |   $9,397,325 | $9,529,400 | $9,990,800 |

As an internal check, the Phase 4 memo specifically reports that at the **0% scenario**, the unhedged position equals $9,228,000, the forward equals $9,397,600, and the corrected put equals $9,068,000. Those figures match the independent calculations above.

### 4. Strategy comparison

| Strategy     | Downside Protection    | Upside Participation | Certainty     | Flexibility | Cost                          |
| ------------ | ---------------------- | -------------------- | ------------- | ----------- | ----------------------------- |
| Unhedged     | None                   | Full                 | Low           | High        | No explicit hedge cost        |
| Forward      | Full                   | None                 | Very high     | Low         | No upfront premium            |
| Money market | Full                   | None                 | Very high     | Low         | Financing/borrowing structure |
| EUR put      | Strong floor           | Yes                  | Moderate/high | High        | $160,000 premium              |
| EUR call     | Poor for this exposure | Strong if EUR rises  | Low           | High        | $160,000 premium              |

The **forward and money-market hedges provide the strongest certainty**. Both effectively lock in approximately $9.398 million, and their results do not depend on the future spot rate. Because the forward requires no upfront premium and is simpler than establishing the borrowing, currency conversion, and investment transactions required by the money-market hedge, it provides a particularly straightforward way to eliminate the FX exposure.

The **put option** provides meaningful downside protection while preserving upside. Its minimum proceeds are $9,068,000. If the euro appreciates, proceeds rise with S_T. The tradeoff is the $160,000 premium. Thus, the company pays for flexibility and upside participation.

The **unhedged position** has maximum direct exposure to exchange-rate movements. At the -5% sensitivity point, proceeds fall to only $8,766,600, but at +5% they increase to $9,689,400.

The **EUR call is not an appropriate primary hedge for this receivable**. It fails to protect the exporter from the fundamental risk identified in the specification—EUR depreciation—and also requires a $160,000 premium. The Phase 2 specification itself identifies the put, rather than the call, as the economically relevant protective option.

### 5. Recommendation

Based solely on these two documents, I would recommend the **forward hedge**.

The company's stated risk is that the euro will depreciate before its EUR 8 million receivable is collected. The forward directly eliminates that risk and locks in **$9,397,600** with no upfront option premium. It also produces slightly higher proceeds than the money-market hedge as calculated from the rounded Phase 4 inputs, although economically those strategies are essentially equivalent under covered interest parity.

The put is attractive if management strongly values participation in future EUR appreciation, but that flexibility costs $160,000. Within the specified ±5% sensitivity range, the forward provides a strong combination of **downside protection, certainty, simplicity, and absence of an upfront premium**. It therefore appears to be the most appropriate hedge for protecting the USD value of this receivable based on the information contained in the Phase 2 specification and Phase 4 memo.

---

**Note:** The response above is preserved as the raw Phase 5 independent LLM output. It was not corrected after the independent run.

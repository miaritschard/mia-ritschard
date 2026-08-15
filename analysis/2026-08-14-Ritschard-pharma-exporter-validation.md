Phase 5 — LLM Analysis & Validation

Student: Amelia Ritschard
Course: FIN 321 — International Business Finance, Sec. 701
Scenario: U.S. Pharmaceutical Exporter
Scenario Slug: pharma-exporter
Date: August 14, 2026

Part 1 — Independent LLM Execution

For the independent LLM execution, I opened a fresh ChatGPT conversation with no prior conversation history. I provided only the Phase 2 Model Specification and the Phase 4 Market-Data Memo. I did not provide the workbook results or correct the LLM during its analysis.

The LLM was instructed to independently calculate the USD proceeds from the unhedged position, forward hedge, money-market hedge, EUR put option, and EUR call option across the specified ending spot-rate sensitivity points. It was also asked to compare the strategies and recommend a hedge based only on the two provided documents.

The independent LLM recommended the forward hedge, primarily because it locks in USD 9,397,600, eliminates the downside risk associated with EUR depreciation, requires no upfront option premium, and is operationally simpler than the money-market hedge.

Key LLM Results
Forward hedge: $9,397,600
Money-market hedge: $9,397,325.24
Put minimum net proceeds: $9,068,000
Unhedged proceeds at the 0% scenario: $9,228,000
Call net proceeds at the 0% scenario: $9,068,000

The raw LLM output from the clean two-document execution is retained as the Phase 5 independent-run record and should be read together with this validation document.

Part 2 — Comparison with Workbook

To validate the independent LLM output, I compared its results with the completed Phase 4 workbook. I selected the -5%, 0%, and +5% ending-spot scenarios because they represent EUR depreciation, the base spot rate, and EUR appreciation.

S_T Scenario	Strategy	LLM Result	Workbook Result	Discrepancy
-5% (1.095825)	Unhedged	$8,766,600	$8,766,600	None
-5% (1.095825)	Forward	$9,397,600	$9,397,600	None
-5% (1.095825)	Money Market	$9,397,325	$9,397,325	None
-5% (1.095825)	EUR Put	$9,068,000	$9,068,000	None
-5% (1.095825)	EUR Call	$8,606,600	$8,606,600	None
0% (1.153500)	Unhedged	$9,228,000	$9,228,000	None
0% (1.153500)	Forward	$9,397,600	$9,397,600	None
0% (1.153500)	Money Market	$9,397,325	$9,397,325	None
0% (1.153500)	EUR Put	$9,068,000	$9,068,000	None
0% (1.153500)	EUR Call	$9,068,000	$9,068,000	None
+5% (1.211175)	Unhedged	$9,689,400	$9,689,400	None
+5% (1.211175)	Forward	$9,397,600	$9,397,600	None
+5% (1.211175)	Money Market	$9,397,325	$9,397,325	None
+5% (1.211175)	EUR Put	$9,529,400	$9,529,400	None
+5% (1.211175)	EUR Call	$9,990,800	$9,990,800	None
Discrepancy Diagnosis

No numerical discrepancies were found between the independent LLM results and the workbook at the three selected sensitivity points. The LLM correctly reproduced the workbook formulas and results for all five strategies.

However, the LLM did identify an important documentation issue involving the treatment of option premiums. The Phase 2 specification originally described carrying the premium forward to maturity, while the Phase 4 market-data memo documented a revision that deducts the $160,000 premium directly. The LLM chose the Phase 4 convention, which is also the convention used in the final workbook. Therefore, this did not create a numerical discrepancy, but it did reveal an area in which the specification could be made more precise.

The forward and money-market results also differ by approximately $275. This is not an LLM or workbook error. The workbook's covered-interest-parity implied forward rate is approximately 1.174666 USD/EUR, while the documented forward input is rounded to 1.1747 USD/EUR. The small proceeds difference therefore results from rounding.

Hand Verification

The following calculations were recomputed independently using a calculator and the named-range notation from the model rather than Excel.

1. Forward Hedge

Named-range formula:

Forward Proceeds = FC_AMT × F0_in

Substitution:

= EUR 8,000,000 × 1.1747 USD/EUR

Result:

= USD 9,397,600

Hand-verified forward proceeds: $9,397,600

This agrees exactly with both the workbook and the independent LLM result.

2. Money-Market Hedge

The money-market hedge requires three steps.

Step 1 — Borrow the Present Value of the EUR Receivable

Named-range formula:

EUR Borrowed Today = FC_AMT / (1 + R_FC × T_DAYS / 360)

Substitution:

= 8,000,000 / (1 + 0.02185 × 360 / 360)

= 8,000,000 / 1.02185

= EUR 7,828,937.71

Step 2 — Convert the Borrowed EUR to USD at Spot

Named-range formula:

USD Available Today = EUR Borrowed Today × S0_in

Substitution:

= 7,828,937.71 × 1.1535

= USD 9,030,679.65

Step 3 — Invest the USD for One Year

Named-range formula:

Money-Market USD Proceeds = USD Available Today × (1 + R_USD × T_DAYS / 360)

Substitution:

= 9,030,679.65 × (1 + 0.0406 × 360 / 360)

= 9,030,679.65 × 1.0406

= USD 9,397,325.24

Hand-verified money-market proceeds: $9,397,325.24

This reconciles to the workbook and the independent LLM result.

3. EUR Put Option — 5% EUR Depreciation Scenario

For the -5% scenario:

S_T = 1.095825 USD/EUR

The put strike is:

K_PUT = 1.1535 USD/EUR

Because S_T < K_PUT, exercising the EUR put provides the higher conversion rate.

Gross Proceeds

Gross Put Proceeds = FC_AMT × max(K_PUT, S_T)

= 8,000,000 × 1.1535

= USD 9,228,000

Premium

Put Premium = FC_AMT × PREM_PUT

= 8,000,000 × 0.0200

= USD 160,000

Net Proceeds

Put Net Proceeds = 9,228,000 - 160,000

= USD 9,068,000

Hand-verified put proceeds: $9,068,000

This agrees exactly with both the workbook and the independent LLM output.

Validation Conclusion

The hand calculations confirm the key workbook outputs. The forward hedge produces $9,397,600 with certainty, while the money-market hedge produces approximately $9,397,325.24. The difference between these two strategies is immaterial relative to the EUR 8 million exposure and is explained by rounding in the forward-rate input.

The put calculation also validates the workbook's downside-protection result. At a 5% EUR depreciation scenario, the put produces $9,068,000 after the $160,000 premium, compared with only $8,766,600 if the exposure is left unhedged.

Overall, the independent LLM output, workbook, and hand calculations reconcile.

Spec Retrospective

The independent LLM execution showed that the Phase 2 specification was generally detailed enough to reproduce the model successfully, but it also exposed areas that could be improved in a second version.

The clearest issue was the treatment of option premiums. Phase 2 described carrying the option premium forward to maturity, while the Phase 4 market-data memo documented a later correction that deducts the $160,000 premium directly. The LLM noticed this conflict and correctly treated the Phase 4 instruction as the controlling convention. Although the resulting analysis matched the final workbook, the LLM had to decide which instruction should take precedence. A stronger specification would eliminate this ambiguity rather than requiring the analyst or LLM to reconcile two different conventions.

A second issue is the EUR call strategy. The specification provides a formula for the call comparison, but a long EUR call is not a natural hedge for an exporter that is already expecting to receive EUR. The LLM correctly noted that the call does not protect the company against the central exposure, which is EUR depreciation. In a revised specification, I would explain more explicitly that the call is included only as a comparative payoff exercise and should not be interpreted as an economically appropriate primary hedge for the receivable.

A third improvement would be to state more explicitly how rounding should be handled. The market-data memo reports the forward rate as 1.1747, while the rates used in the covered-interest-parity calculation imply a forward of approximately 1.174666. This creates the small difference between the forward proceeds of $9,397,600 and money-market proceeds of approximately $9,397,325. A revised specification could require calculations to retain full precision internally and round only displayed values.

For a Version 2 specification, I would therefore establish one definitive option-premium convention, clarify the purpose of the EUR call comparison, specify a rounding and precision policy, and state explicitly that later documented market-data revisions supersede earlier placeholder assumptions. These changes would make the model easier to reproduce without requiring judgment about conflicting instructions.

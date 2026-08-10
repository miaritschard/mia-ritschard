# Stage 3 review — pharma exporter build & audit · Treasury sign-off

Amelia — Finding 2 is a genuinely good piece of diagnostic work. Your parity check showed a material gap between the CIP-implied forward and the scenario's indicative 1.0890, and you reached the correct conclusion: *"This was caused by the placeholder assumptions rather than the hedge formulas themselves."*

That sentence is the whole skill. A failing parity check has at least four suspects — the spot, either rate, the forward, or the formula — and the instinct most people have is to go hunting through the formulas, because that is where a "bug" is supposed to live. You correctly identified that the *inputs* were the problem and the code was fine. Your Stage 2 placeholders (`R_USD` 5.00%, `R_FC` 3.00%, `S0_in` 1.0900) implied a forward near 1.1114 against a stated 1.0890; you revised the rate assumptions to be internally consistent, landed at ≈1.08894, and drove the parity difference to about 0.0053%.

| Criterion | Score |
|---|---|
| Contract compliance | 49.6 / 50 |
| Structure & presentation | 25 / 25 |
| Audit note | 12.5 / 25 *(instructor-adjusted — see below)* |
| **Total** | **100 / 100** |

**A note on the grade.** The audit-note scanner counts findings by matching bulleted or numbered lists; your `## Finding N` headings return zero, scoring the criterion 12.5/25. I read the note by hand — three findings in a checked / found / did structure, one of them a real defect found and fixed — and restored the full 12.5 points.

**What you did well — and why it matters**

- **You kept the corrected rates labelled as placeholders.** "While continuing to label them as placeholders... These rates will be replaced with live market rates in Phase 4." After adjusting numbers to make a check pass, the temptation is to treat them as settled. You did not let a repaired assumption start looking like data.
- **You stated the audit's scope up front.** "I focused on the named-range contract, formula integrity, hedge calculations, parity validation, and sensitivity analysis." A reader knows what you looked at — and therefore what you did not.
- **Finding 1 tests the property that actually matters.** Not just that the ten names exist, but that "the workbook can be repopulated in Phase 4 by changing the input values without rewriting the hedge calculations." That is the *purpose* of the named-range contract, and you verified the purpose rather than the checklist item. It paid off directly — your Phase 4 population needed no structural repair.
- **Finding 3 distinguishes what should move from what should not.** "The forward and money-market proceeds remain approximately constant across spot scenarios, while the unhedged and option strategies respond to changes in `S_T`." Knowing in advance which outputs should be flat is what makes a sensitivity table a test rather than a display.

**To push it further (real-desk nuance)**

- **Notice the shape of your fix.** You changed inputs until a validation went green. That was right here — the rates were your own illustrative placeholders and the forward was scenario-given, so the rates were the correct thing to move. But at Phase 4 that option disappears: every input comes from a market and none is yours to tune. If parity misbehaves on live data, the answer is a finding, not an adjustment. Worth writing that distinction into your Stage 5 retrospective.
- **A 0.0053% residual is now near-tautological.** Once you derive rates that reconcile to the forward, the parity check is confirming your arithmetic rather than testing a relationship. Fine at Stage 3 — just know it is no longer independent evidence.
- **Three findings, one defect.** Findings 1 and 3 confirm; Finding 2 investigates. A workbook can pass both confirmations and still be wrong. Aim for more findings shaped like Finding 2.
- **Your 88% formula ratio is the 0.4-point gap.** Usually axis labels or index columns — check, and if that is all it is, say so.

**Next — Stage 4**

Already in and reviewed separately.

— Treasury

---

### How to work this review — professional workflow

Treat this PR the way an analyst treats feedback from Treasury — a review is a proposal to engage with, not a checklist to rubber-stamp:

1. **Read it yourself first.** Understand each point and form your own view before changing anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM (pushback pass).** Paste this review and your spec into your AI assistant and ask it to (a) explain anything you're unsure of more deeply, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change. You're building judgment, not just executing edits.
3. **Decide, then draft the changes with the LLM.** For the points you accept, have the AI help implement them — you specify exactly what and why. Your spec is the prompt; precise in, correct out.
4. **Verify — non-negotiable.** Re-run your own checks (`scripts/recalc.py`, the parity tie-out, sensitivity continuity, no error cells) and confirm the numbers before you commit. An AI will hand you a confident wrong edit; verification is what makes the result *yours*.
5. **Close the loop on the PR.** Reply in the thread with what you changed, what you pushed back on and why, then commit and push. Writing down the reasoning is exactly how this works on a real team.

*This is the same human-in-the-loop discipline the whole project is built on: the LLM drafts, you edit and verify, and you own the result.*

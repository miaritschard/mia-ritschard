# Stage 2 review — pharma exporter · Treasury sign-off

Amelia — the spec is clean and complete: all ten named ranges, three hedge families, a sensitivity plan, and a data-status banner at the top that marks every value as indicative until Phase 4. That banner is worth more than it looks — it means no reader can mistake a construction placeholder for a market number, which is the single most damaging failure mode of a document like this.

| Criterion | Score |
|---|---|
| Named-range contract & tab architecture | 30 / 30 |
| Calculation flow | 30 / 30 |
| Validation & sensitivity plan | 20 / 20 |
| Reproducibility & prompt log | 20 / 20 |
| **Total** | **100 / 100** |

**What you did well — and why it matters**

- **You stated the no-cell-addresses rule as a requirement.** "No cell-address references should be required to understand the model." That is the actual engineering purpose of the named-range contract, written as a testable rule rather than left implicit — and your Stage 3 build honoured it.
- **You distinguished scenario-provided values from your own placeholders.** `FC_AMT` (8,000,000) and `F0_in` (1.0890) are marked scenario-provided; everything else is yours. When something later turns out to be inconsistent, knowing which numbers you chose and which you were handed is what lets you fix the right one — and that is exactly what happened at Stage 3.
- **You explained the exposure mechanism, not just the exposure.** "If EURUSD falls, each euro received will convert into fewer U.S. dollars, reducing the dollar value of the receivable and potentially reducing the firm's expected cash flow and profit margin." A reader who has never seen an FX exposure understands the risk from that sentence alone.
- **Your Phase-4 source column allows alternatives with a standard.** "Live EURUSD spot from Yahoo Finance, ECB, Bloomberg, **or comparable source**" — flexible about the vendor, specific about the instrument. That is the right way to write a sourcing instruction you will execute two weeks later.

**The one real issue — and you caught it yourself**

Your Stage 2 placeholder set is not parity-consistent. `S0_in` = 1.0900, `R_USD` = 5.00%, `R_FC` = 3.00%, `F0_in` = 1.0890. Covered interest parity with those rates gives:

```
F = 1.0900 × (1 + 0.05 × 365/360) / (1 + 0.03 × 365/360)
  = 1.0900 × 1.050694 / 1.030417
  ≈ 1.1114
```

against a stated forward of 1.0890 — a gap of about 0.022 USD/EUR, roughly **$180,000 on EUR 8,000,000**. The direction is the tell: `R_USD` exceeds `R_FC` by 200bp, so the euro must trade at a forward *premium* and the forward has to sit above spot. Yours sits just below it.

**I am flagging this as a note, not a deduction, because your Stage 3 audit found it and fixed it.** Finding 2 of your build audit reads: "the initial illustrative interest-rate combination produced a larger difference between the CIP-implied forward and the scenario-provided indicative forward of 1.0890. This was caused by the placeholder assumptions rather than the hedge formulas themselves." You then revised the rates to be internally consistent and got the parity difference down to ~0.0053%. That is precisely the right diagnosis and the right fix.

The lesson worth carrying forward is about *when* you caught it. Your validation rules could not have flagged this at Stage 2, because a spec's inputs are never checked until something computes them. A ten-minute parity calculation while writing §2 would have caught it before you built anything. On a real desk, an inconsistent assumption set that survives into a built model is expensive to unwind; caught in the spec, it costs one line of arithmetic.

**One thing for Stage 4**

Keep `T_DAYS` and the day-count basis separate in your head. `T_DAYS` is the actual number of days to settlement; 360 is the *denominator* in the ACT/360 convention. They are different quantities that happen to be similar numbers, and conflating them is easy to do.

**Next — Stages 3 and 4**

Both are in and reviewed separately.

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

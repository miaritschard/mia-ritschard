# Stage 5 review — pharma exporter LLM analysis & validation · Treasury sign-off

Amelia — the most valuable observation in this document is one you almost undersold. Your comparison table shows zero discrepancies across fifteen cells, and a weaker validation would have stopped there and called the spec vindicated. You did not:

> *"The Phase 2 specification originally described carrying the premium forward to maturity, while the Phase 4 market-data memo documented a revision that deducts the $160,000 premium directly. The LLM chose the Phase 4 convention… Therefore, this did not create a numerical discrepancy, but it did reveal an area in which the specification could be made more precise."*

That is the sharpest kind of finding: **a match that required arbitration is not really a match.** Two of your own documents gave conflicting instructions, and the numbers agreed only because the LLM happened to resolve the conflict the same way your workbook did. Flip that coin the other way and you have a $160,000 discrepancy with no obvious cause. Noticing that your clean reconciliation was partly luck is exactly the instinct this stage is meant to build.

| Criterion | Score |
|---|---|
| LLM execution & comparison | 25 / 25 |
| Hand verification | 25 / 25 |
| Recommendation & executive voice | 25 / 25 |
| Spec retrospective | 17 / 17 |
| Repo polish | 6.4 / 8 |
| **Total** | **99 / 100** |

**What you did well — and why it matters**

- **Every figure reconciles.** I recomputed the money-market chain independently: `8,000,000 / 1.02185 = €7,828,937.7` ✓ → `× 1.1535 = $9,030,679.65` ✓ → `× 1.0406 = $9,397,325.2` ✓. The CIP forward implied by your own rates is `1.1535 × 1.0406 / 1.02185 = 1.174659` against a documented 1.1747 ✓ — so your $275 gap really is display rounding, exactly as you diagnosed.
- **You explained the $275 instead of excusing it.** *"The workbook's covered-interest-parity implied forward rate is approximately 1.174666, while the documented forward input is rounded to 1.1747."* Naming the mechanism — and then proposing "retain full precision internally and round only displayed values" as the v2 rule — turns an anomaly into a control.
- **You disqualified the call on economics, not on convention.** *"Because the company already expects to receive euros, its principal risk is a decline in the value of the euro. The call does not provide protection against that depreciation."* Correct, and correctly reasoned from the exposure rather than from the spec saying so.
- **Your §E states the cost of your own recommendation.** *"Accepting that opportunity cost is consistent with the purpose of hedging: reducing uncertainty rather than speculating on favorable exchange-rate movements."* That is the right principle, in the right register, and it is the sentence that makes the memo advice rather than advocacy.
- **You committed the raw LLM output as its own file** and told the reader it *"should be read together with this validation document."* Evidence a reader can check is what separates a validation from a claim.

**The one substantive correction — you resolved the conflict as a precedence question; it is a finance question**

You found the premium-convention conflict, and then your v2 fix says: *"state explicitly that later documented market-data revisions supersede earlier placeholder assumptions."*

That rule would lock in the **wrong** convention.

The Phase 2 spec said to carry the premium forward to maturity. The Phase 4 memo revised that to deduct $160,000 directly. Phase 2 was right, and the "correction" introduced an error. Here is why: the premium is paid **today**; the receivable settles in **one year**. Subtracting a today-dollar from a settlement-dollar mixes two valuation dates — the exact thing your own money-market hedge exists to handle correctly, since it discounts the euro leg and compounds the dollar leg precisely because timing matters.

Carried forward at the same `R_USD` and basis you use everywhere else:

```
FV_PREM_PUT = 160,000 × 1.0406 = $166,496
Put floor (like-for-like) = 9,228,000 − 166,496 = $9,061,504     (vs. your $9,068,000)
Breakeven vs. forward     = (9,397,600 + 166,496) / 8,000,000 = 1.19551  → 3.64% above spot
```

So the floor is $6,496 lower and the breakeven sits about 7 bp higher than the memo states. Neither changes your recommendation — the forward still wins, by slightly more.

What I want you to take from it is the ordering of the question. When two documents disagree, "which one is more recent?" is the second question. The first is **"which one is right?"** A precedence rule that always defers to the latest revision propagates whichever error was introduced most recently, and it removes the analyst from the loop entirely. The better v2 clause reads: *"the option premium is paid at inception and must be carried to the settlement date at `R_USD` before netting against proceeds"* — a convention stated once, with its reason, so no future reader has to arbitrate at all.

**A question rather than a correction — is `T_DAYS` 360 or 365?**

Your hand verification uses `360 / 360`, collapsing both interest legs to a period fraction of exactly 1.0, for a receivable you describe throughout as due *"in one year."* If the settlement horizon really is 365 days, the ACT/360 fraction is 1.013889 and the CIP forward moves from 1.174659 to about **1.174954** — roughly $2,360 on this notional.

It may well be deliberate and declared in your spec, in which case it is fine and self-consistent (you apply it to both legs, which is what matters most). But a classmate found a genuinely expensive version of this same question — a euro rate sourced at overnight tenor against a 365-day horizon — so it is worth confirming that `T_DAYS`, the rate tenors, and the contractual settlement date all describe the same period.

**One framing note**

§D: *"The forward locks in $9,397,600, which is approximately $169,600 more than the USD value of the receivable at today's spot rate."* Arithmetically right (9,397,600 − 9,228,000 ✓), but those are dollars at two different dates, and the gap is the USD-EUR interest differential rather than value the hedge creates — your own money-market replication reaches nearly the same figure with no forward involved. You immediately redirect to the correct reason (*"More importantly, the amount is known in advance"*), so nothing downstream breaks. Just retire the comparison, or label it as carry.

**Repo polish — 1.6 points, one item**

`LICENSE` is the only open box; description, per-directory READMEs, public status and commit history are all in place. Add an MIT license at the repo root for a 100.

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

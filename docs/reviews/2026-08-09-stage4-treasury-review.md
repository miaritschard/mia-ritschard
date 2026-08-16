# Stage 4 review — pharma exporter market data & population · Treasury sign-off

Amelia — you disclosed the weakness in your own euro leg without being asked: *"Although €STR is an overnight reference rate rather than a one-year government yield, the Phase 4 instructions allow a deposit/reference rate, and the source is official and reproducible."*

That is the right way to use a proxy. You named the instrument, named the mismatch, named the authority you were relying on, and named the offsetting virtue. A reader can now decide for themselves whether to accept it. The alternative — writing "ECB, 2.185%" and letting the reader assume it is a one-year rate — is how tenor mismatches survive into models people trade on.

| Criterion | Score |
|---|---|
| Data quality & provenance | 50 / 50 |
| Model resolves cleanly | 33 / 33 |
| Lab cross-check | 17 / 17 |
| **Total** | **100 / 100** |

**What you did well — and why it matters**

- **You recorded reference date, publication time, and retrieval date separately.** €STR is logged as "Reference date: August 6, 2026; published August 7, 2026 at 08:00." Overnight rates are published the following morning for the prior day, and getting that right — rather than treating the publication date as the rate date — is a detail most people miss entirely.
- **You gave a reason for each source, not just a name.** ECB spot because "it provides an official and reproducible daily reference rate"; DGS1 because "the hedge horizon is approximately one year and the assignment permits a government yield." Reproducibility as an explicit selection criterion is exactly the right standard for a model someone else will inherit.
- **You said why `F0_in` is computed.** "Because I did not use a reliable freely accessible live 1-year EURUSD forward quote." That converts a derived number from a shortcut into a documented fallback.
- **Your Phase 3 discipline paid off.** No structural repair was needed at population — a direct consequence of Finding 1 in your build audit, where you verified the workbook could be repopulated by changing inputs alone.

**Two things to correct**

**1. `T_DAYS = 360` is the wrong quantity.**

Your table lists `T_DAYS` as **360 days**, with the note "Assigned one-year scenario / Course ACT/360 convention." Those are two different numbers that have been collapsed into one.

- `T_DAYS` is the **actual days to settlement** — 365 for a one-year receivable.
- **360** is the **denominator** in the ACT/360 day-count convention.

The formula is `1 + r × T_DAYS/360`, and it needs both: the real elapsed time on top, the convention on the bottom. By setting `T_DAYS = 360` you have made that ratio exactly 1.0, which silently drops five days of interest from both legs. On your numbers the effect is small — roughly 5/360 of the rate differential, about 0.0003 on the forward, or ~$2,400 on EUR 8M — but the error is conceptual rather than numerical, and at a longer tenor or a wider differential it would matter much more.

The fix is one cell: set `T_DAYS = 365` and leave the 360 where it belongs, in the denominator.

**2. Your parity check has become circular.**

`F0_in` (1.1747) is CIP-implied from the same `S0_in`, `R_USD`, and `R_FC` that drive the money-market leg. Both legs are therefore built from one input set, and they will agree by construction. That confirms your implementation is internally consistent; it cannot detect a wrong input, a wrong convention, or a market dislocation.

Worth stating explicitly in the memo, because a passing parity check reads to a casual reader as market validation, and here it is not. The genuine unknown — dealer spread plus cross-currency basis — never enters your model, since no quoted forward does. Getting even one real forward quote and reporting the gap against your 1.1747 would turn the check from an arithmetic identity into a measurement.

**Next — Stage 5**

Hand the workbook and your Phase 2 spec to an LLM, get its analysis, then break it. Recompute at least three outputs by hand with explicit arithmetic — forward proceeds, the put floor, and the crossover spot where the put overtakes the forward. Then write the recommendation in a CFO's voice framed on risk tolerance rather than on which strategy tops the grid.

Your spec retrospective has an unusually strong, honest story ready: the Phase 2 placeholder rates were not parity-consistent with the scenario forward, your Phase 3 audit caught it, and you fixed the assumptions rather than the formulas. Write that up — it is exactly what the retrospective is for.

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

# Moove Developer Program Alignment

Source: [Everything You Need To Know About Moove Developer Program](https://www.moove.xyz/blog/everything-you-need-to-know-about-moove-developer-program) (official article, retrieved and archived during research).

This file maps LATCH against every requirement the program states, then lists the gaps that must close before applying.

## Requirement-by-requirement check

| # | Program requirement | LATCH status | Verdict |
|---|---|---|---|
| 1 | Build a real product with Moove Agentic Payments | LATCH is designed around the verified Moove Payments API (`payment_link:create`, `payment_link:read`) and the payment-link lifecycle | Aligned in design; not yet demonstrated |
| 2 | Eligible stage: idea, prototype, or live product | LATCH is a documented, locked idea with a validation roadmap | Aligned (idea stage is explicitly eligible) |
| 3 | Builder profile: independent developers/hackers with a public GitHub and a serious idea | Solo builder, GitHub account active (`Jayanng`) | Aligned |
| 4 | Application needs: what you are building, which Moove features you will integrate, milestones, links to GitHub/repo/demo | Definition and feature scope documented; milestones drafted in roadmap; repo exists but is private | Partial — see gaps |
| 5 | Every application reviewed, including a short call | Prepared to walk through thesis, API findings, and roadmap | Aligned |
| 6 | Grants disbursed in USDC to your Moove Handle | Moove Handle not yet claimed (unverified) | Gap |
| 7 | Milestones agreed in writing before first disbursement; tranches released against verified progress | Roadmap exists but is not yet written as verifiable program milestones | Partial — see gaps |
| 8 | "Proper and good use": genuine integration demonstrated with live code in a public repo, real transactions settling on-chain, and real users touching the product | Repo is private; no live code or transactions yet | Gap — see gaps |
| 9 | Projects that go quiet, miss milestones, or misuse funds forfeit the remainder | Roadmap phased to be verifiable; no overpromising in documentation | Aligned in approach |
| 10 | No equity, no token allocation | Nothing requested or offered | Aligned |

## Program-fit strengths

- **Load-bearing integration.** Without Moove payment links there is no LATCH. The product is an operational layer on top of Moove's own reconciliation surface, not a wrapper around an unrelated service.
- **Real-usage story.** Payment recovery produces on-chain transactions, merchant operations, and payer interactions that are all verifiable through Moove's own API data.
- **Emerging-market relevance.** The program explicitly welcomes emerging-market projects; LATCH targets small cross-border merchants who accept crypto payments through Moove links.
- **Agentic-payments connection.** LATCH case states are machine-readable by design, so AI agents that pay through links can detect `underpaid`, `expired_with_value`, and `excess_or_duplicate` states instead of guessing. This aligns with the program's core agentic-payments narrative.
- **Distribution value to Moove.** LATCH makes Moove links safer for real commerce, which supports Moove's own adoption goals rather than competing with them.

## Gaps to close before applying

1. **Public repository.** The program's "proper and good use" test expects live code in a public repo. LATCH is currently private. Decide whether to publish the documentation repo and, later, the implementation repo at application time.
2. **Claim a Moove Handle.** Grants are disbursed in USDC to the project's Moove Handle. This must exist before the application and before any milestone verification.
3. **Write program-format milestones.** Convert the roadmap into written, verifiable milestones, for example:
   - M1: live Moove API key, real payment link created, real link-listed reconciliation, polling demo (capability proof).
   - M2: payment-case engine classifying underpaid and expired-with-value from live Moove data.
   - M3: payer and merchant recovery views with a real transaction completing a case.
   - M4: first external user or merchant touching the product.
4. **Phase 0 live capability proof.** Run the verification plan already documented in the README (create link, inspect fields, test partial payment and expiry) so the application can cite observed behavior, not only the public schema.
5. **Emphasize the agentic angle in the application copy.** Lead with agent-readable recovery states; keep human payer recovery as the parallel flow.

## What must NOT be claimed in the application

Consistent with the evidence ledger:

- No claim of a working integration before Phase 0 passes.
- No claim of automatic refunds, outgoing transfers, or webhook support from Moove.
- No claim of first-of-kind status for payment recovery as a category.
- No promise of capabilities that depend on unverified early-access APIs; those are phrased as requests during the review call.

## Bottom line

LATCH is aligned with the program as an **idea-stage application from an independent builder**, which the program explicitly supports. The binding constraints are operational, not conceptual: public repo, Moove Handle, written milestones, and live capability proof before claiming any tranche.

Last reviewed: 2026-09-07.

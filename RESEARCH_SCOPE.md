# Research scope and honesty boundary

LATCH is based on:

- Official Moove public API schema and documentation
- Official Moove Developer Program material
- Public documentation from comparable crypto-payment products
- Product decisions recorded in the README and evidence ledger

## We do not claim

- LATCH is globally first.
- Moove has no private or unreleased recovery tooling.
- Moove supports webhooks, refunds, outgoing transfers, payer identity, or native order metadata in the public API.
- A payment can be reversed after it is broadcast.
- A simulated test state is a real Moove transaction.
- The repository is a working integration before the live capability gate passes.

## Why this repository exists

This repository freezes the product thesis before implementation so future code cannot drift into a generic payment processor, AI marketplace, treasury dashboard, or undocumented Moove integration.

Any future change should answer:

1. Does it improve the shared payer/merchant recovery case?
2. Does it preserve non-custodial boundaries?
3. Is the capability verified in Moove or explicitly labelled as an assumption?
4. Does it reduce fulfilment risk without forcing a restart?
5. Does it preserve an auditable final business outcome?

If not, it is outside LATCH's locked scope.

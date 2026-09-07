# LATCH Evidence Ledger

This file records verified facts that constrain the product. It deliberately separates source-backed facts from product decisions and open questions.

| ID | Claim | Evidence | Status | Product impact |
|---|---|---|---|---|
| M-001 | Moove has a public Payments API for creating and managing payment links. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | LATCH integrates as an overlay, not a replacement processor. |
| M-002 | Public endpoints include `POST /v1/payment-link` and `GET /v1/payment-link`. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | Initial adapter scope. |
| M-003 | Public API authentication uses `X-API-Key`. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | Use least-privilege API scopes. |
| M-004 | Public scopes include `payment_link:create` and `payment_link:read`. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | Read/create MVP boundary. |
| M-005 | Link listing supports status filtering and offset pagination. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | Polling reconciler must paginate and persist observations. |
| M-006 | Public link fields include requested amount, received amount, token, chain, status, expiry, description, max usage, and transaction URL when available. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | These fields power first classifications. |
| M-007 | Public status enum is `active`, `completed`, or `inactive`. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | LATCH must add its own finer-grained case states. |
| M-008 | The inspected public schema documents no fund-moving endpoint. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified within inspected schema | No automatic refund or outgoing-transfer promise. |
| M-009 | The public API description says payment-link keys cannot move funds and links settle to the owner’s default wallet. | [Moove OpenAPI](https://api.moove.xyz/openapi.json) | Verified | LATCH remains non-custodial. |
| M-010 | Moove says the Developer Program funds products using Moove Agentic Payments, with API/SDK early access and milestone-based funding. | [Developer Program](https://www.moove.xyz/blog/everything-you-need-to-know-about-moove-developer-program) | Verified from official article | Early access may unlock capabilities beyond public API, but cannot be assumed. |
| P-001 | Payment exception handling is a real category across crypto payment products. | [BTCPay](https://docs.btcpayserver.org/Development/ecommerce-integration-guide/), [Cobo](https://mcpnow.mintlify.app/payments/en/guides/features), [BVNK](https://help.bvnk.com/hc/en-us/articles/27122836779666-Auto-Refund-to-Source-Address), [MainPay](https://mainpay.com/reconciliation) | Verified as prior art | LATCH must not claim global first-of-kind. |
| P-002 | Existing systems handle partial, late, overpaid, underpaid, expired, and refund cases in different ways. | Same sources above | Verified as prior art | LATCH differentiates through the Moove-specific shared case and fulfilment boundary. |
| D-001 | LATCH uses a shared payer/merchant payment case. | Product decision | Locked | Core product definition. |
| D-002 | LATCH does not custody funds or promise undocumented Moove actions. | Product decision | Locked | Safety and truth boundary. |
| D-003 | Hero workflow is expired-with-value; underpayment is supporting workflow. | Product decision | Locked | MVP/demo focus. |

## Open questions

- Does a live Moove test account expose partial-payment behavior as expected?
- How quickly do `receivedAmount`, `status`, and `transactionUrl` update?
- What exactly happens to a payment after `expirationDate`?
- Can multiple payments be safely attributed to one payment link?
- Are webhooks available to Developer Program members?
- Are refund or outgoing-payment capabilities available to approved builders?
- Can an external order reference be safely encoded in `description`?

## Evidence rules

- A source-backed field is not proof of a live transaction until it is exercised.
- A missing public endpoint is not proof that Moove has no private or early-access capability.
- Search absence is not proof of global novelty.
- Simulated states must be labelled as simulated in any future demo.
- No production or security claim is valid until independently tested.

Last reviewed: 2026-09-07.

## Sources

The complete URL list and rationale are maintained in `README.md` and this ledger. The authoritative live schema is the Moove OpenAPI document linked above.

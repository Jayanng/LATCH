# LATCH

## Recover imperfect Moove payments without losing the order

LATCH is a proposed non-custodial recovery layer for Moove payment links. It turns an imperfect payment into a shared case that the payer and merchant can resolve without restarting checkout or risking duplicate fulfilment.

> **A payment does not become a business failure just because it arrives imperfectly.**

LATCH is currently a documented product thesis and validation repository. It is not yet claiming a completed integration, production readiness, automatic refunds, or native Moove support for features that are not present in the public API.

## The problem

Cross-chain crypto payments are flexible, but real business payments do not always arrive in a clean, final form. A payment can be partial, late, excess, or still unresolved when a payment link expires. A merchant then has to decide whether to fulfil, wait, request the remaining balance, offer credit, or manually resolve the case.

The usual alternatives are poor:

- Reject the payment and make the customer start again.
- Fulfil too early and risk a loss or duplicate order.
- Ask support to inspect transaction hashes manually.
- Refund automatically when the customer would rather complete the payment.
- Treat a payment status as if it were the same thing as a completed business outcome.

LATCH focuses on the missing operational layer between a Moove payment-link record and safe order fulfilment.

## Product definition

> **LATCH is a shared recovery-case layer for Moove payment links. It keeps underpaid, late, partial, and excess payments attached to the original order, gives the payer a clear way to continue or resolve the payment, and gives the merchant a safe fulfilment decision with an auditable final outcome.**

LATCH does not replace Moove, custody funds, or pretend to reverse an on-chain transfer.

## Why Moove

Moove’s public Payments API provides programmatic creation and reconciliation of payment links. The verified public schema exposes:

- `POST /v1/payment-link` to create a link
- `GET /v1/payment-link` to list links, with status filtering and pagination
- Link ID and checkout URL
- Requested amount
- Received amount when available
- Settlement token and chain
- Expiration date
- Max usage
- Description
- Link status: `active`, `completed`, or `inactive`
- Transaction URL when available

Moove’s API documentation also states that payment-link keys cannot move funds, payment links settle to the authenticated owner’s default wallet, and the link-list endpoint is the reconciliation surface. These constraints define LATCH’s initial non-custodial boundary: LATCH interprets and coordinates payment cases; it does not claim to refund or transfer funds through undocumented endpoints.

Moove is load-bearing because LATCH is designed around Moove’s payment-link lifecycle and settlement configuration, not a generic chain-agnostic invoice abstraction.

## What LATCH adds

```text
Moove payment link
    ↓
Business order binding
    ↓
Polling reconciliation
    ↓
Payment-case classification
    ↓
Payer and merchant recovery flow
    ↓
Fulfilment decision
    ↓
Final business-outcome receipt
```

Moove’s payment-link record describes what happened to the link. LATCH adds what the business should do next.

## Initial case types

The first release should stay narrow:

### Underpaid

The received amount is below the requested amount. Fulfilment is blocked or sent to a merchant policy decision. The payer sees the remaining amount where the verified payment state supports that calculation.

### Expired with value

The payment link is no longer active or has passed its expiration while value is associated with the link. LATCH preserves the case for merchant review instead of silently treating the value as a new order or silently fulfilling twice.

### Excess or duplicate payment case

The link or business order appears to have received more value or usage than expected. LATCH flags the case for review and prevents automatic duplicate fulfilment. It must not claim that two independent transfers are duplicates unless the evidence supports that conclusion.

## Shared recovery case

Each LATCH case records, when available:

- LATCH case ID
- External order ID
- Moove payment-link ID
- Requested amount
- Received amount
- Settlement token
- Settlement chain
- Creation time
- Expiration time
- Moove status
- Transaction URL
- Immutable observed-state events
- Merchant fulfilment policy
- Payer-visible resolution state
- Merchant decision
- Final business outcome

A description field may carry a compact reference to an external order, but LATCH must not imply that Moove provides native arbitrary metadata unless this is verified separately.

## Payer experience

The payer should see a plain-language recovery page, not a blockchain investigation task.

Example:

```text
You paid $23 of the requested $25.
The merchant has not fulfilled the order yet.

You can:
- complete the remaining balance, if the payment state supports it;
- ask the merchant to review the payment;
- follow the merchant’s stated credit or return process.
```

LATCH does not promise automatic refunds. Refunds, credits, and outgoing transfers require explicit Moove capability verification or merchant-controlled external handling.

## Merchant experience

The merchant sees:

- What was requested
- What Moove reports as received
- Which token and chain were configured
- Whether the link is active, completed, or inactive
- Transaction evidence exposed by Moove
- Why fulfilment is blocked or allowed
- The recommended next action
- The complete case event history

The initial policy outputs are:

```text
FULFIL
HOLD
REQUEST_BALANCE
MANUAL_REVIEW
EXCESS_OR_DUPLICATE_REVIEW
CLOSE
```

These are LATCH decisions. They are not native Moove statuses.

## Hero workflow

The primary demo should be late payment after expiry, with underpayment as the supporting workflow.

```text
1. Merchant creates a Moove payment link for a fixed order.
2. The link expires or becomes inactive.
3. Value is observed against the link or the link is left in an unresolved state.
4. LATCH creates an EXPIRED_WITH_VALUE or review case.
5. The payer sees what happened and does not silently start a second order.
6. The merchant chooses reattachment, credit, manual return, or closure according to policy.
7. LATCH records the decision and final business outcome.
```

A second demo path:

```text
1. Merchant requests $25.
2. Moove reports $23 received.
3. LATCH marks the case PARTIALLY_RECEIVED.
4. Fulfilment is blocked.
5. The payer sees the remaining amount or review path.
6. A verified resolution is recorded.
7. The merchant approves fulfilment only when safe.
```

## What LATCH is not

LATCH is not:

- A new payment processor
- A wallet or custody service
- A replacement for Moove payment links
- A generic crypto accounting suite
- An AI shopping agent
- An API marketplace
- An escrow protocol
- An automatic refund engine
- A claim that every transaction can be reversed
- A claim that Moove exposes webhooks, refunds, outgoing transfers, payer identity, per-transfer history, or native order metadata

## Differentiation

Similar products demonstrate that payment exception handling is a real category. Coinbase focuses on controlled settlement and reducing asset/network mismatch. BTCPay provides detailed Bitcoin/Lightning invoice states and partial-payment handling. Cobo supports payment orders, underpayment, overpayment, late payment, cancellations, and refunds. BVNK supports configurable auto-refunds for payment exceptions. MainPay and Request Finance focus on payment matching, invoices, finance operations, and reconciliation.

LATCH should not copy those products or claim to be the first payment-recovery product. Its qualified differentiation is:

> **A Moove-native, non-custodial recovery case that keeps payer and merchant on the same order, preserves Moove’s settlement model, applies a business fulfilment policy, and records the final business outcome rather than stopping at payment status.**

## Product principles

1. **Moove truth before LATCH inference.** Preserve the raw Moove record and clearly label LATCH-derived classifications.
2. **No silent fulfilment.** An ambiguous or insufficient payment must not become a successful order by accident.
3. **No silent loss.** A late or partial payment must become a recoverable case, not disappear into an inactive link.
4. **No custody claims.** LATCH does not hold or move funds unless a separately verified, explicitly integrated capability exists.
5. **No fake automation.** Manual review is a valid state when the public API cannot safely automate the action.
6. **Idempotent reconciliation.** Polling the same link repeatedly must append no duplicate event or fulfilment.
7. **Business outcome over payment status.** `completed` is a Moove link status; fulfilment is a merchant decision recorded by LATCH.
8. **Evidence boundaries are visible.** Distinguish live Moove evidence, LATCH calculations, merchant declarations, and test fixtures.

## Initial architecture

```text
Moove Payments API
        │
        ▼
Moove adapter
        │
        ▼
Reconciliation poller
        │
        ▼
Append-only payment-case ledger
        │
   ┌────┴────┐
   ▼         ▼
Payer UI  Merchant UI
        │
        ▼
Fulfilment policy and outcome receipt
```

### Moove adapter

Use the public `X-API-Key` integration only with the minimum required scopes. The initial adapter needs payment-link creation and reading/reconciliation.

### Reconciliation poller

The public API documentation does not establish webhook support. The initial design therefore uses polling with backoff, pagination, and durable last-observed state. A future webhook adapter may be added only after Moove confirms and documents it.

### Case ledger

Store every observed Moove state with an observation time. Do not overwrite history. A case can be reclassified as new evidence arrives, but the raw observation remains available.

### Outcome receipt

The receipt must separate:

- Moove-observed fields
- LATCH-derived state
- Merchant action
- Final business outcome

## Roadmap

### Phase 0: capability proof

- Obtain a Moove API key through the official account flow.
- Create one real payment link.
- List it through the public API.
- Confirm actual response fields and pagination.
- Make a controlled test payment.
- Observe `receivedAmount`, `status`, `expirationDate`, and `transactionUrl` behavior.
- Test partial payment and expiry behavior where safe.

### Phase 1: read-only recovery cases

- Import Moove links.
- Bind links to external orders.
- Poll and persist state changes.
- Classify underpaid and expired-with-value cases.
- Show raw evidence and LATCH interpretation.

### Phase 2: shared recovery UX

- Add payer case view.
- Add merchant policy and fulfilment decision.
- Add manual-resolution states.
- Add case and outcome receipts.

### Phase 3: verified extensions only

- Add webhook ingestion only if Moove provides a documented webhook capability.
- Add outgoing or refund actions only if Moove provides and authorizes the relevant API.
- Add multi-transfer aggregation only if the public or approved Moove data can identify the relationship safely.

## Verification status

### Verified from the public Moove OpenAPI schema

- Base API: `https://api.moove.xyz`
- Public schema: `https://api.moove.xyz/openapi.json`
- Endpoint: `GET /v1/payment-link`
- Endpoint: `POST /v1/payment-link`
- API-key authentication through `X-API-Key`
- Scopes: `payment_link:create`, `payment_link:read`
- Link listing with `status` filter and `offset` pagination
- Link fields listed above
- No fund-moving endpoint in the inspected schema
- Payment links settle to the authenticated owner’s default wallet

### Not verified and intentionally not promised

- Public webhooks
- Refund API
- Outgoing transfer API
- Payer identity API
- Per-transfer history for each link
- Native order metadata
- Guaranteed aggregation of multiple transfers
- Automatic credit or refund execution
- Production-readiness of any future LATCH integration

## Source ledger

1. [Moove live OpenAPI schema](https://api.moove.xyz/openapi.json) — public API endpoints, scopes, fields, status values, settlement behavior, and documented limitations.
2. [Moove Developer Program](https://www.moove.xyz/blog/everything-you-need-to-know-about-moove-developer-program) — developer fund, live payment infrastructure, API/SDK early access, milestones, and real-usage expectations.
3. [Moove API and SDK introduction](https://www.moove.xyz/blog/building-on-moove-an-introduction-to-web3-payment-apis-and-sdks-for-developers) — developer positioning and integration surface.
4. [Coinbase Commerce API](https://docs.cloud.coinbase.com/commerce-onchain/docs/api-reference/commerce-api/rest-api/introduction) — controlled settlement and mismatch reduction reference.
5. [BTCPay payment requests](https://docs.btcpayserver.org/PaymentRequests/) — partial payment reference.
6. [BTCPay invoice states](https://docs.btcpayserver.org/Development/ecommerce-integration-guide/) — paid-late, partial, and overpaid state reference.
7. [Cobo payment features](https://mcpnow.mintlify.app/payments/en/guides/features) — payment exception and refund reference.
8. [BVNK auto-refund behavior](https://help.bvnk.com/hc/en-us/articles/27122836779666-Auto-Refund-to-Source-Address) — configurable late, underpaid, and overpaid refund reference.
9. [MainPay reconciliation](https://mainpay.com/reconciliation) — stablecoin invoice matching and exception-operations reference.
10. [Request Finance webhooks](https://docs.request.finance/webhooks) — invoice event and webhook reference.
11. [OxaPay underpayment recovery](https://oxapay.com/blog/glossary/underpayment-recovery/) — recovery option reference.

## Status

**Locked product thesis. Research/documentation phase. Not yet a working integration.**

The next gate is live Moove capability verification. No build, bounty claim, automatic refund claim, or production-readiness claim should be made before that gate passes.

## License

Documentation and future code: to be chosen before implementation. No license is claimed by this research-only repository until the project owner selects one.

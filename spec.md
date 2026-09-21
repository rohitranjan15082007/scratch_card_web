# P2P Payment and Withdrawal System — Project Structure

**Version:** 1.0  
**Based on:** `P2P_Payment_Withdrawal_Specification_v1.2.md`  
**Purpose:** Codex-ready folder and module structure for implementing the payment/withdrawal MVP.

> This structure implements the engineering specification only. Real-money operation must remain disabled until an approved payment flow, verification capability, refund process, and legal/compliance review are complete.

## 1. Recommended technology stack

| Layer | Recommended choice |
|---|---|
| Monorepo | pnpm workspaces + Turborepo |
| Web application | Next.js + TypeScript |
| UI | Tailwind CSS + accessible component library |
| API | Node.js + NestJS + TypeScript |
| Database | PostgreSQL |
| ORM/migrations | Prisma |
| Background jobs | Redis + BullMQ |
| File evidence | Private S3-compatible object storage |
| Authentication | Adapter for host-platform auth; JWT/session validation in API |
| Validation | Zod or class-validator at every API boundary |
| Testing | Vitest/Jest, Supertest, Playwright |
| Observability | Structured logs, metrics and error tracking |
| Local development | Docker Compose |

## 2. Repository structure

```text
p2p-payment-system/
├── apps/
│   ├── web/                         # Buyer and receiver web application
│   │   ├── src/
│   │   │   ├── app/
│   │   │   │   ├── (auth)/
│   │   │   │   │   ├── login/page.tsx
│   │   │   │   │   └── callback/page.tsx
│   │   │   │   ├── dashboard/page.tsx
│   │   │   │   ├── wallet/page.tsx
│   │   │   │   ├── withdrawals/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   ├── new/page.tsx
│   │   │   │   │   └── [withdrawalId]/page.tsx
│   │   │   │   ├── products/page.tsx
│   │   │   │   ├── checkout/[productId]/page.tsx
│   │   │   │   ├── orders/[orderId]/page.tsx
│   │   │   │   ├── matches/[matchId]/
│   │   │   │   │   ├── payment/page.tsx
│   │   │   │   │   ├── submit-proof/page.tsx
│   │   │   │   │   └── confirm-receipt/page.tsx
│   │   │   │   ├── support/page.tsx
│   │   │   │   ├── layout.tsx
│   │   │   │   └── error.tsx
│   │   │   ├── components/
│   │   │   │   ├── wallet/
│   │   │   │   ├── withdrawals/
│   │   │   │   ├── payments/
│   │   │   │   ├── orders/
│   │   │   │   ├── status-timeline/
│   │   │   │   └── ui/
│   │   │   ├── lib/
│   │   │   │   ├── api-client.ts
│   │   │   │   ├── auth.ts
│   │   │   │   ├── currency.ts
│   │   │   │   ├── errors.ts
│   │   │   │   └── upload.ts
│   │   │   ├── hooks/
│   │   │   ├── types/
│   │   │   └── middleware.ts
│   │   ├── public/
│   │   ├── tests/
│   │   ├── next.config.ts
│   │   └── package.json
│   │
│   ├── admin/                       # Restricted operations and reconciliation UI
│   │   ├── src/
│   │   │   ├── app/
│   │   │   │   ├── dashboard/page.tsx
│   │   │   │   ├── withdrawals/page.tsx
│   │   │   │   ├── matches/page.tsx
│   │   │   │   ├── matches/[matchId]/page.tsx
│   │   │   │   ├── disputes/page.tsx
│   │   │   │   ├── disputes/[disputeId]/page.tsx
│   │   │   │   ├── refunds/page.tsx
│   │   │   │   ├── reconciliation/page.tsx
│   │   │   │   ├── delivery-jobs/page.tsx
│   │   │   │   ├── audit-logs/page.tsx
│   │   │   │   └── settings/page.tsx
│   │   │   ├── components/
│   │   │   ├── lib/
│   │   │   └── middleware.ts
│   │   ├── tests/
│   │   └── package.json
│   │
│   ├── api/                         # Authoritative backend and business rules
│   │   ├── src/
│   │   │   ├── main.ts
│   │   │   ├── app.module.ts
│   │   │   ├── config/
│   │   │   │   ├── env.schema.ts
│   │   │   │   ├── app.config.ts
│   │   │   │   └── feature-flags.config.ts
│   │   │   ├── common/
│   │   │   │   ├── auth/
│   │   │   │   ├── database/
│   │   │   │   ├── errors/
│   │   │   │   ├── guards/
│   │   │   │   ├── idempotency/
│   │   │   │   ├── locks/
│   │   │   │   ├── logging/
│   │   │   │   ├── money/
│   │   │   │   ├── pagination/
│   │   │   │   ├── rate-limit/
│   │   │   │   └── transitions/
│   │   │   ├── modules/
│   │   │   │   ├── users/
│   │   │   │   ├── wallets/
│   │   │   │   ├── withdrawals/
│   │   │   │   ├── products/
│   │   │   │   ├── orders/
│   │   │   │   ├── matching/
│   │   │   │   ├── payment-handles/
│   │   │   │   ├── payment-submissions/
│   │   │   │   ├── verification/
│   │   │   │   ├── receiver-confirmations/
│   │   │   │   ├── settlements/
│   │   │   │   ├── journal/
│   │   │   │   ├── disputes/
│   │   │   │   ├── refunds/
│   │   │   │   ├── delivery/
│   │   │   │   ├── notifications/
│   │   │   │   ├── provider-events/
│   │   │   │   ├── reconciliation/
│   │   │   │   ├── audit/
│   │   │   │   └── admin/
│   │   │   └── health/
│   │   ├── test/
│   │   │   ├── unit/
│   │   │   ├── integration/
│   │   │   └── e2e/
│   │   ├── Dockerfile
│   │   └── package.json
│   │
│   └── worker/                      # Durable asynchronous and scheduled jobs
│       ├── src/
│       │   ├── main.ts
│       │   ├── queues/
│       │   ├── jobs/
│       │   │   ├── payment-deadline.job.ts
│       │   │   ├── receiver-timeout.job.ts
│       │   │   ├── provider-event.job.ts
│       │   │   ├── delivery.job.ts
│       │   │   ├── notification.job.ts
│       │   │   ├── outbox-dispatch.job.ts
│       │   │   └── daily-reconciliation.job.ts
│       │   └── processors/
│       ├── test/
│       ├── Dockerfile
│       └── package.json
│
├── packages/
│   ├── database/                    # Prisma schema, migrations and seeds
│   │   ├── prisma/
│   │   │   ├── schema.prisma
│   │   │   ├── migrations/
│   │   │   └── seed.ts
│   │   ├── src/client.ts
│   │   └── package.json
│   ├── contracts/                   # Shared DTOs, schemas, enums and API types
│   │   ├── src/enums/
│   │   ├── src/schemas/
│   │   ├── src/events/
│   │   └── package.json
│   ├── payment-adapters/            # No fake production verification
│   │   ├── src/interfaces/
│   │   ├── src/sandbox/
│   │   ├── src/manual-reconciliation/
│   │   └── src/approved-provider/   # Added only after provider selection
│   ├── auth-adapter/                # Host platform authentication integration
│   ├── wallet-source-adapter/       # Traceable originating wallet credits
│   ├── product-delivery-adapters/   # Entitlement/file/service activation
│   ├── notifications/               # Email/SMS/WhatsApp abstraction
│   ├── observability/               # Logging, traces and metrics
│   ├── eslint-config/
│   └── tsconfig/
│
├── infrastructure/
│   ├── docker/
│   │   └── docker-compose.yml
│   ├── nginx/
│   ├── monitoring/
│   │   ├── dashboards/
│   │   └── alerts/
│   └── deployment/
│       ├── staging/
│       └── production/
│
├── scripts/
│   ├── bootstrap.sh
│   ├── migrate.sh
│   ├── seed-sandbox.sh
│   ├── reconcile.ts
│   └── verify-journal-balance.ts
│
├── docs/
│   ├── architecture.md
│   ├── state-machines.md
│   ├── database-schema.md
│   ├── api-contracts.md
│   ├── payment-adapter.md
│   ├── reconciliation-runbook.md
│   ├── dispute-runbook.md
│   ├── refund-runbook.md
│   ├── security-model.md
│   └── deployment.md
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── migrations-check.yml
│       └── deploy-staging.yml
├── .env.example
├── .gitignore
├── package.json
├── pnpm-workspace.yaml
├── turbo.json
└── README.md
```

## 3. Standard backend module layout

Each business module should use the same internal structure:

```text
modules/withdrawals/
├── withdrawals.module.ts
├── withdrawals.controller.ts       # HTTP endpoints only
├── withdrawals.service.ts          # Use-case orchestration
├── withdrawal-policy.service.ts     # 50% eligibility calculation
├── withdrawal-state-machine.ts      # Allowed state transitions
├── withdrawal.repository.ts         # Database access and row locking
├── dto/
│   ├── create-withdrawal.dto.ts
│   └── cancel-withdrawal.dto.ts
├── events/
├── errors/
└── tests/
```

Controllers must not contain wallet or settlement rules. Business rules belong in domain/policy services and must execute through database transactions where required.

## 4. Core database models

The Prisma schema should contain these model groups:

| Group | Models |
|---|---|
| Identity | `User`, `UserRiskProfile`, `AdminRole` |
| Wallet | `Wallet`, `WalletReservation`, `WalletCreditSource` |
| Journal | `JournalAccount`, `JournalTransaction`, `JournalPosting` |
| Withdrawal | `WithdrawalRequest`, `WithdrawalRuleSnapshot` |
| Purchase | `Product`, `PurchaseOrder`, `OrderPriceSnapshot` |
| Matching | `PaymentMatch`, `PaymentDestinationSnapshot` |
| Evidence | `PaymentSubmission`, `EvidenceUpload`, `ProviderEvent` |
| Confirmation | `ReceiverConfirmation`, `PaymentVerification` |
| Resolution | `Dispute`, `AdminResolution`, `Refund` |
| Settlement | `Settlement` |
| Fulfillment | `DeliveryJob`, `Entitlement` |
| Reliability | `IdempotencyRecord`, `OutboxEvent`, `AuditLog` |

Required database safeguards:

- Store INR in integer paise using `BIGINT`; never use floating point.
- One unresolved withdrawal per user through a partial unique index.
- One live reservation per withdrawal.
- One active match per order and per withdrawal.
- Unique settlement per match and withdrawal.
- Unique entitlement per order/product.
- Scoped uniqueness for provider/network transaction references.
- Positive-amount and currency-consistency checks.
- Immutable journal transactions and postings; corrections are reversals.
- Transaction-level row locking for creation, matching and settlement.

## 5. Main backend responsibilities

### Withdrawals

- Calculate the snapshotted eligible maximum using basis points.
- Check the configured minimum/maximum and unresolved-request restriction.
- Create the request, hold, reservation and balanced journal atomically.
- Permit cancellation only when the safe-cancellation rules allow it.

### Matching

- Match exact amount and currency only.
- Select oldest eligible request with a deterministic tie-break.
- Lock and claim the order, withdrawal and reservation atomically.
- Freeze the assigned payment destination snapshot before exposure.
- Never split or automatically resize a withdrawal.

### Verification

- Treat a UTR or screenshot only as a claim.
- Use a sandbox adapter in non-production.
- Use manual reconciliation when authoritative provider verification is unavailable.
- Validate signed provider webhooks and prevent replay/duplicate processing.

### Settlement and journal

- Require verified evidence plus receiver confirmation, or an evidenced admin override.
- Debit held wallet liability exactly once.
- Mark the order paid and delivery pending in the same transaction.
- Insert a durable outbox event before commit.
- Never wait for an external provider or delivery API while holding database locks.

### Delivery

- Consume settlement outbox events asynchronously.
- Create one entitlement using a unique fulfillment key.
- Retry failures without charging or settling again.

### Refunds and disputes

- Keep external refunds separate from wallet hold releases.
- Record funding source, destination validation, executor and payout proof.
- Preserve original settlement history after a post-settlement refund.
- Require reason, evidence and audit trail for admin overrides.

## 6. API groups

```text
/api/v1
├── /wallet
│   └── GET  /balance
├── /withdrawals
│   ├── POST /
│   ├── GET  /:withdrawalId
│   └── POST /:withdrawalId/cancel
├── /products
│   └── GET  /
├── /orders
│   ├── POST /
│   └── GET  /:orderId
├── /matches
│   ├── GET  /:matchId
│   ├── POST /:matchId/payment-submissions
│   └── POST /:matchId/receiver-confirmation
├── /payments
│   └── POST /provider-webhook
└── /admin
    ├── GET  /dashboard
    ├── POST /disputes/:disputeId/resolve
    ├── POST /matches/:matchId/close-unpaid
    ├── POST /orders/:orderId/retry-delivery
    └── POST /refunds
```

All mutation endpoints require an `Idempotency-Key` except verified provider callbacks, which use the provider's stable event identifier. Authorization, ownership, role, state transition and amount checks are always server-enforced.

## 7. Environment file structure

```dotenv
NODE_ENV=development
APP_ENV=sandbox

DATABASE_URL=postgresql://...
REDIS_URL=redis://...

AUTH_ISSUER=
AUTH_AUDIENCE=
AUTH_JWKS_URL=

PAYMENT_ADAPTER=sandbox
PAYMENT_WEBHOOK_SECRET=
PAYMENT_WINDOW_MINUTES=15
RECEIVER_CONFIRMATION_WINDOW_MINUTES=30

WITHDRAWAL_PERCENTAGE_BPS=5000
WITHDRAWAL_MIN_PAISE=
WITHDRAWAL_MAX_PAISE=
THRESHOLD_OVERRIDE_ENABLED=false
THRESHOLD_PAISE=

EVIDENCE_BUCKET=
EVIDENCE_MAX_BYTES=

ENCRYPTION_KEY=
AUDIT_HASH_SECRET=
```

Secrets must never be committed. Production startup should fail if `PAYMENT_ADAPTER=sandbox` or required approved-provider settings are missing.

## 8. Implementation order for Codex

### Phase 1 — Foundation

1. Initialize monorepo and shared TypeScript configuration.
2. Add PostgreSQL, Redis and Docker Compose.
3. Create Prisma models, constraints and initial migration.
4. Add authentication/authorization guards and structured errors.
5. Add idempotency, audit and transactional-outbox infrastructure.

### Phase 2 — Wallet and withdrawal

1. Implement balanced journal and cached wallet reconciliation.
2. Implement the basis-points eligibility function.
3. Implement withdrawal creation and atomic reservation.
4. Implement safe cancellation and withdrawal timeline.
5. Add concurrency and rule-snapshot tests.

### Phase 3 — Orders and exact matching

1. Implement immutable product/price snapshot.
2. Implement order creation and waiting state.
3. Implement exact-amount matching with deterministic locking.
4. Freeze the receiver payment destination.
5. Build buyer/receiver payment status screens.

### Phase 4 — Evidence, confirmation and timers

1. Add private proof uploads and metadata scanning.
2. Add payment submission and scoped reference conflict checks.
3. Add receiver confirmation/denial.
4. Add payment and receiver timeout workers.
5. Route expiry and uncertainty to reconciliation, never auto-success.

### Phase 5 — Settlement and delivery

1. Implement atomic, idempotent settlement.
2. Add outbox delivery worker.
3. Implement entitlement creation and retry behavior.
4. Add lost-response, duplicate-event and delivery-failure tests.

### Phase 6 — Admin operations

1. Build dispute/reconciliation dashboard.
2. Implement evidence-based admin resolution with maker-checker support.
3. Implement unpaid closure, refund cases and payout evidence.
4. Add journal and audit-log inspection.

### Phase 7 — Hardening and launch gate

1. Implement all 20 acceptance tests from specification v1.2.
2. Run security, authorization and upload tests.
3. Add reconciliation monitors and alerts.
4. Configure backups, secret management and deployment.
5. Keep real-money mode locked until provider and compliance prerequisites are approved.

## 9. Minimum MVP screens

### Receiver

- Wallet balance: total, available and locked.
- Eligible withdrawal amount and request form.
- Current withdrawal status/timeline.
- Assigned buyer-payment notification.
- Receipt confirmation or denial screen.

### Buyer

- Product list and checkout.
- Waiting-for-match status.
- Frozen UPI destination, exact amount and countdown.
- UTR/reference and proof submission.
- Verification, delivery and refund status.

### Admin

- Aged holds and expired/unreconciled matches.
- Complete match evidence and timeline.
- Dispute resolution and recorded override.
- Refund case management.
- Delivery retry controls.
- Journal, reconciliation and audit views.

## 10. Testing layout

```text
tests/
├── unit/
│   ├── withdrawal-eligibility.spec.ts
│   ├── state-transitions.spec.ts
│   └── journal-balancing.spec.ts
├── integration/
│   ├── withdrawal-concurrency.spec.ts
│   ├── matching-race.spec.ts
│   ├── settlement-idempotency.spec.ts
│   ├── provider-event-deduplication.spec.ts
│   └── delivery-retry.spec.ts
├── e2e/
│   ├── normal-settlement.e2e-spec.ts
│   ├── expiry-reconciliation.e2e-spec.ts
│   ├── receiver-dispute.e2e-spec.ts
│   ├── late-payment.e2e-spec.ts
│   └── refund-resolution.e2e-spec.ts
└── security/
    ├── authorization.spec.ts
    ├── evidence-access.spec.ts
    ├── webhook-replay.spec.ts
    └── upload-validation.spec.ts
```

The original specification's 20 acceptance tests remain mandatory and should be mapped to named automated test cases before implementation is considered complete.

## 11. Codex starting instruction

Use the following instruction when beginning implementation:

```text
Use P2P_Payment_Withdrawal_Specification_v1.2.md as the authoritative business specification and P2P_System_Project_Structure_v1.0.md as the repository blueprint. Implement Phase 1 only. Use TypeScript, pnpm, Turborepo, NestJS, Next.js, PostgreSQL/Prisma and Redis/BullMQ. Keep payment mode sandbox-only. Add database constraints, tests and a clear README. Do not implement split matching, returns, referrals, multi-currency or real-money provider behavior. Stop after Phase 1, run the tests, and report created files, commands and remaining issues.
```

Implement one phase at a time. Review and test each phase before authorizing Codex to continue to the next one.

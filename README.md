# DEN Authenticated order and team communication policy

Private service package, version 0.1.0, JSON contract version `1.0.0`. Plain ESM with no third-party runtime dependencies. Backend-compatible exports from `src/index.js`: `authorizeChannel, appendMessage, readMessages`.

Run `npm test` and `npm run check` on Node 24. CI runs both. Tests cover successful state transitions and authorization/idempotency failure paths; they do not establish production readiness.

## Integration boundary

Resolve active actor, organization membership and current order participants from DB on EVERY call. Store messages by channel and paginate after authorization. Consumers cannot privately contact organization staff. Order channels close when active fulfillment ends. React renders text as text, never raw HTML.

State transitions return new state; callers must store results transactionally with compare-and-swap/unique constraints. In-memory arrays are command fixtures, not production storage. The parent backend vendors a pinned copy plus checksum manifest for private-repository builds; this package is the authoritative source. Client-provided actors, statuses, receipt flags and balances have no authority.

## Current readiness

Domain authorization and immutable message commands work locally. No websocket transport, message persistence, moderation/reports, attachments, retention implementation, push delivery or E2E encryption.

No external resources are provisioned, no credentials are present, and no live money is moved. See `contract.json` for the version envelope; domain payload validation is exported by the functions above.

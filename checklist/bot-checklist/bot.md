---
title: Chatbot & Messaging Platform Checklist
checklist_type: standalone
version: 1
status: active
scope: " bots on messaging platforms, webhooks, tokens, rate limits, conversation flows, broadcast ops,"
last_updated: 2026-11-15
---

# Chatbot & Messaging Platform Checklist

> Framework-agnostic checklist for bots on messaging platforms (LINE, Telegram, Slack, Discord, WhatsApp).
> Webhooks, tokens, rate limits, and conversation flows — the platform is a third-party API you don't control.
> Last updated: 2026-11-15 (created)

---

## 1. Channel & Webhook Setup

- [ ] **Official account/app registered through the platform's own console:** each platform (LINE, Telegram, Slack, Discord, WhatsApp) has its own account type, review, and approval path (register before building against assumed features).
- [ ] **Webhook endpoint served over HTTPS on an owned/verified domain:** platforms require valid TLS before enabling push delivery (no self-signed certificates in production).
- [ ] **Dev/test bot or channel kept separate from production:** separate app registration and tokens per environment (test traffic never touches the production account or its quotas).
- [ ] **Delivery mode chosen and documented:** webhook push where a public HTTPS endpoint exists, long-polling where not (Telegram's `getUpdates` is the polling alternative to webhook registration — run exactly one mode per bot).
- [ ] **Platform retry/redelivery semantics understood:** unacknowledged or failed events are resent within a retry window (LINE redelivers failed webhooks and flags repeats with `isRedelivery`); the design assumes duplicates → [[data-messaging]].
- [ ] **Webhook registration state is reproducible:** endpoint URL, secret, and subscribed event types live in code/config and are re-registrable (console clicks are not the source of truth).

---

## 2. Webhook Security

- [ ] **Platform signature verified on EVERY request:** HMAC-style digest over the raw body with the channel/signing secret compared to the platform's signature header (LINE's `x-line-signature`, Slack's `X-Slack-Signature`, Telegram's `X-Telegram-Bot-Api-Secret-Token` header check).
- [ ] **Unverified requests rejected outright:** missing or non-matching signature → error response and stop (never "log and process anyway") → [[security]].
- [ ] **Raw body verified before any parsing or mutation:** compute over the exact received bytes (LINE documents that re-serializing or escaping the body before verification breaks the signature), e.g. `hmac_sha256(channel_secret, raw_body) == header_signature`.
- [ ] **Replay window enforced where the platform sends a timestamp:** Slack-style request-timestamp header checked against a small window (five minutes per Slack's docs) with constant-time signature comparison.
- [ ] **Endpoint path unguessable and the endpoint itself rate-limited:** random path segment plus edge-level throttling (the signature is the real gate; the path is hygiene).
- [ ] **Idempotency/dedup by event ID:** replayed or re-delivered events are recognized and not double-processed → [[data-messaging]].

---

## 3. Event Processing Reliability

- [ ] **ACK fast, then process asynchronously:** return 2xx inside the platform's short response window and hand off to a queue (slow handlers trigger platform retries and duplicate deliveries), e.g. `ack 2xx → enqueue(event_id) → worker → reply via API` → [[data-messaging]].
- [ ] **Consumer queue in front of all business logic:** the webhook handler only validates and enqueues (spike absorption, retries, and scaling belong to the consumer).
- [ ] **Dedup by platform event/message ID:** at-least-once delivery from the platform is the norm; a processed-event store or unique constraint guarantees exactly one reply per event.
- [ ] **Outbox pattern for replies:** reply intent written in the same transaction as the state change, a relay sends through the platform API (a crash between "decided to reply" and "sent" must not lose the reply).
- [ ] **Dead-letter queue for poison events:** malformed or persistently failing events land in a DLQ after N retries (inspectable and replayable, never silently dropped).
- [ ] **Ordering handled per event type:** per-user message events usually need order (sequence checks or per-user keying); membership/status events can be unordered (document which class each event type is in).

---

## 4. Rate Limits & Quotas

- [ ] **Per-platform rate limits and quota tiers mapped:** endpoint-level and account-level limits documented per platform and re-checked periodically (platforms change them without notice) → [[api]].
- [ ] **Client-side token-bucket shaping on every outbound call:** never discover limits by hitting them (all sends flow through a rate-shaped sender).
- [ ] **Broadcast/push quotas modeled into the send queue:** periodic push quotas (LINE push-quota class) and messaging tiers (WhatsApp template-tier class) budgeted across campaigns before scheduling.
- [ ] **Exponential backoff with jitter on 429/limit responses:** honor `Retry-After` where provided (never tight-loop a throttled API).
- [ ] **Graceful degradation when throttled:** queue and delay sends instead of dropping them (delayed state visible to operators) → [[ops]].

---

## 5. Tokens & Credentials

- [ ] **Token lifecycle understood per platform:** long-lived vs rotating channel access tokens and the reissue/refresh path documented per platform (reissuing a secret invalidates the old one — plan the cutover).
- [ ] **Secrets stored in a secret manager:** channel secrets, bot tokens, and signing secrets live in a vault and are injected at runtime → [[02 Secrets Management]].
- [ ] **Webhook signing secret distinct from API token:** separate credentials for inbound verification and outbound calls (compromising one must not hand over the other).
- [ ] **Scopes minimized:** request only the permissions the implemented flows need (broad scopes widen the blast radius of a leaked token).
- [ ] **Never in logs or source:** tokens redacted from logs, error traces, and URLs; no secrets in client-reachable code → [[security]].

---

## 6. Conversation Design

- [ ] **Explicit state machine for flows:** intents and branches modeled as states with defined transitions (implicit in-memory "just remember" state breaks on restart or multi-instance).
- [ ] **Session state with timeout:** sessions expire after inactivity and resume or restart cleanly when the user returns.
- [ ] **Platform-native affordances used:** quick replies, menus, buttons, and rich cards per the platform's model (don't fake menus with typed text where buttons exist).
- [ ] **Processing feedback shown:** typing indicator or immediate ack for slow operations (users resend when the bot looks dead — which doubles load).
- [ ] **Friendly error message plus human-handoff fallback:** errors phrased for end users in their language; stuck or confused users escalate to a human queue instead of looping.
- [ ] **Unicode/i18n rendering tested on real clients:** Thai text, emoji, stickers, and line-breaking render correctly per platform (golden-screenshot checks where feasible).
- [ ] **Conversation resumable after abandonment:** a returning user picks up mid-flow or restarts without corrupting state.

---

## 7. User Data & Consent

- [ ] **Subscriber IDs and chat logs treated as personal data:** platform user IDs are persistent identifiers and transcripts contain PII → [[privacy]].
- [ ] **Opt-in recorded before marketing/broadcast messages:** consent captured per purpose (platform ToS and local law both apply to promotional messaging).
- [ ] **Block/unfollow honored immediately:** follow/unfollow/leave events synced to do-not-contact the moment they arrive (a user who blocked the bot is never messaged again).
- [ ] **Retention window on chat logs documented:** how long transcripts, session state, and derived data are kept, and what is purged on schedule.
- [ ] **Data export and deletion on request:** subject-request flows cover chat history and derived data end-to-end (tested, not assumed).

---

## 8. Broadcast & Messaging Ops

- [ ] **Small-segment test before every full send:** each campaign proves itself on a small audience first (wrong audience or broken template is unsendable, not unsent).
- [ ] **Send windows per timezone:** messages land at reasonable local hours (a 3 a.m. push is how a bot gets blocked).
- [ ] **Delivery-rate monitoring per campaign:** sent/delivered/failed/replied tracked per push so drop-offs are visible mid-campaign → [[ops]].
- [ ] **Template and approval workflows respected:** platforms requiring template review (WhatsApp template class) or quota approval (LINE push-quota class) have their lead time built into campaign planning.
- [ ] **Throttling protects users as well as platform limits:** pacing chosen for user experience (no burst-spam), not merely to stay under quotas.

---

## 9. LLM Turns (If Used)

- [ ] **User messages treated as untrusted input:** prompt injection assumed possible in every turn; tool and retrieval scope kept minimal → [[llm-security]].
- [ ] **Output moderation layer on every LLM reply:** content checks or allowlists run before send (an injected instruction never reaches users verbatim).
- [ ] **Hallucination handling in high-stakes flows:** money/health/legal topics get bounded, sourced answers or handoff to the scripted flow (the model never freelances there).
- [ ] **Per-user cost caps and token budgets:** runaway context (long threads, injected payloads) cannot blow the per-user budget.
- [ ] **Scripted fallback when the model fails:** timeout or overload routes to the deterministic flow or human handoff (the bot still works without the LLM) → [[ai]].

---

## 10. Observability

- [ ] **Conversation funnel metrics tracked:** delivered/replied/converted per command and per campaign (a bot that "works" but converts nothing is broken).
- [ ] **Webhook failure rate and reply latency are core SLOs:** 4xx/5xx on the webhook endpoint and time-from-event-to-reply monitored per platform.
- [ ] **Queue depth and DLQ alerting:** alerts on webhook failure spikes and consumer lag fire before users notice → [[ops]].
- [ ] **Logs respect message content:** no message bodies in wide-access logs without an explicit reason; PII redacted from traces → [[privacy]].

---

## 11. Testing

- [ ] **Webhook replay tests with captured payloads:** recorded platform events (with valid signatures) replayed against the endpoint in CI.
- [ ] **Mock platform API for tests plus a live smoke per release:** integration tests run against a mock; a scripted smoke on the real test channel catches platform-side behavior changes before users do.
- [ ] **Message-format regression tests:** rich message JSON renders as intended per platform (golden screenshots on real clients where feasible).
- [ ] **Burst/load test on campaign sends:** send queue and rate shaper survive a full-campaign burst without tripping platform limits.
- [ ] **Idempotency tests for duplicate delivery:** a platform retry (same event ID) produces exactly one reply and one side effect.

---

## 12. Platform Policy & Compliance

- [ ] **Platform ToS on content and promotional messaging reviewed:** what the platform forbids (mass DMs, restricted content) is a product constraint, not a legal footnote.
- [ ] **Account verification tiers and age restrictions understood:** verified-account requirements and minimum-age rules per platform before launch planning.
- [ ] **Brand and asset rules followed:** profile images, display names, and message templates meet platform branding rules (rejection here blocks launch).
- [ ] **Vendor review of platform data handling:** what the platform stores on its infrastructure, and where → [[privacy]].

---

## Quick Sanity Check Before Release

- [ ] Every webhook request is signature-verified; unverified requests are rejected before any processing.
- [ ] Handler acks fast and processes via a queue; platform retries are deduped by event ID.
- [ ] Replies go out through a rate-shaped sender with an outbox; failures land in a DLQ, not on the floor.
- [ ] Tokens and channel secrets live in a secret manager; webhook secrets ≠ API tokens; nothing sensitive in logs.
- [ ] Block/unfollow is honored immediately; opt-in recorded before any broadcast.
- [ ] Live smoke on the test channel passed this release; rich messages verified on real clients.
- [ ] Delivery-rate, webhook-failure, and queue-depth alerts reach a human on-call → [[ops]].

---

## Sources

- LINE Messaging API: https://developers.line.biz/en/docs/messaging-api/
- Telegram Bot API: https://core.telegram.org/bots/api
- Slack API: https://api.slack.com/docs
- General contracts: [[api]].

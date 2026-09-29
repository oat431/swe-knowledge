---
tags:
- api
- programming
- protocols
---

# 02 WebHook

A WebHook is a user-defined HTTP callback. Instead of polling "has anything changed?", the server calls YOUR endpoint when something happens. It's the "don't call us, we'll call you" pattern.

---

## Polling vs WebHook

```
❌ Polling:
   Your App → "Anything new?" → Their Server → "Nope."
   Your App → "Anything new?" → Their Server → "Nope."
   Your App → "Anything new?" → Their Server → "Nope."
   (wasteful, slow, rate-limited)

✅ WebHook:
   Their Server → "Event happened!" → Your App
   (instant, efficient, event-driven)
```

---

## How WebHooks Work

```mermaid
%%{init: {'theme':'base','themeVariables':{'background':'#1B1717','primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','actorBkg':'#19362D','actorBorder':'#1FB854','actorTextColor':'#CDD3D1','actorLineColor':'#1FB854','signalColor':'#CDD3D1','signalTextColor':'#CDD3D1','labelBoxBkgColor':'#161212','labelBoxBorderColor':'#1FB854','labelTextColor':'#CDD3D1','loopTextColor':'#CAC9C9','noteBkgColor':'#1EB88E','noteTextColor':'#000C07','noteBorderColor':'#1EB88E','activationBkgColor':'#1EB88E','activationBorderColor':'#1FB8AB','sequenceNumberColor':'#000000','fontSize':'14px'}}}%%
sequenceDiagram
    participant GH as GitHub
    participant CI as CI Server
    participant SA as Your App
    
    Note over GH: Push to main
    GH->>CI: POST /webhook {"event":"push","ref":"main"}
    CI->>CI: Build + Test
    CI->>SA: POST /webhook {"event":"deploy","status":"success"}
    SA->>SA: Deploy
    SA-->>CI: 200 OK
    CI-->>GH: 200 OK
```

---

## Security: Verify the Sender

Anyone can POST to your webhook endpoint. You must verify it's actually from who you think.

### Signature Verification (HMAC)

```java
@PostMapping("/webhook/github")
public ResponseEntity<String> handleGithubWebhook(
        @RequestBody String payload,
        @RequestHeader("X-Hub-Signature-256") String signature) {
    
    // Compute expected signature
    String computed = "sha256=" + HmacUtils.hmacSha256Hex(webhookSecret, payload);
    
    if (!MessageDigest.isEqual(computed.getBytes(), signature.getBytes())) {
        return ResponseEntity.status(403).body("Invalid signature");
    }
    
    // Process the event
    processEvent(payload);
    return ResponseEntity.ok().build();
}
```

| Provider | Signature Header |
|----------|-----------------|
| GitHub | `X-Hub-Signature-256` |
| Stripe | `Stripe-Signature` |
| Slack | `X-Slack-Signature` |
| Shopify | `X-Shopify-Hmac-SHA256` |

---

## Idempotency: Handle Duplicates

WebHook providers may deliver the same event multiple times. Your handler must be idempotent.

```java
@PostMapping("/webhook/stripe")
public ResponseEntity<String> handleStripeWebhook(
        @RequestBody String payload,
        @RequestHeader("Stripe-Signature") String signature,
        @RequestHeader("X-Request-Id") String requestId) {
    
    // Already processed?
    if (eventRepository.existsByExternalId(requestId)) {
        return ResponseEntity.ok().build();  // Acknowledge, skip processing
    }
    
    Event event = constructEvent(payload, signature);
    processEvent(event);
    
    // Mark as processed
    eventRepository.save(new ProcessedEvent(requestId));
    return ResponseEntity.ok().build();
}
```

---

## Retry & Timeout

| Provider Behavior | Your Responsibility |
|------------------|-------------------|
| Retries on failure (non-2xx) | Always return 2xx quickly, process async if needed |
| Retries on timeout | Respond in < 5 seconds |
| May deliver out of order | Handle events independently (or sequence them) |
| May deliver duplicates | Be idempotent |

---

## Sources

- GitHub Webhooks: https://docs.github.com/en/webhooks
- Stripe Webhooks: https://stripe.com/docs/webhooks

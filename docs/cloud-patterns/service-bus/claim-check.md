# Claim Check with Azure Service Bus

The **Claim Check** pattern stores a large message payload in external storage and sends only a reference (a “claim check”) through the messaging system.

This is useful when message size limits, cost, or performance make it undesirable to pass large payloads through a queue/topic.

---

## When to use

Use this pattern when:
- payloads are large (documents, images, batch files),
- multiple steps need access to the same payload,
- you want to reduce message bus cost and avoid hitting size limits.

Avoid it when:
- the payload is small and can be safely included in the message.

---

## Reference architecture

```mermaid
flowchart LR
  Producer --> Blob[Blob Storage\n(payload)]
  Producer --> SB[Service Bus Message\n(claim check: URL/id)]
  SB --> Consumer
  Consumer --> Blob
```

### Azure mapping
- Store payload in **Azure Storage (Blob)** or another durable store.
- Send a **Service Bus** message that contains:
  - a blob URL or ID,
  - content type,
  - correlation ID,
  - optional integrity metadata (hash).

---

## Key considerations (L200–L300)

- **Security**: avoid putting long-lived secrets in messages; use controlled access (managed identity, short-lived SAS where appropriate).
- **Lifecycle management**: define retention/cleanup for stored payloads.
- **Consistency**: handle the two-step write carefully (store payload, then send message) and design for retries.

---

## References

- Claim Check pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/claim-check
- Cloud design patterns catalog: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Azure Service Bus documentation: https://learn.microsoft.com/en-us/azure/service-bus-messaging/
- Azure Blob Storage documentation: https://learn.microsoft.com/en-us/azure/storage/blobs/
